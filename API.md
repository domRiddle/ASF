# API

ASF supports API-like response through ```!api``` command, which can be used for easily integrating ASF process status with third-party wrappers. If you can implement ASF's **[IWCF](https://github.com/JustArchi/ArchiSteamFarm/blob/master/ArchiSteamFarm/WCF.cs#L34)** interface, then you can access it even easier with ```string GetStatus()``` operation contract. Otherwise, you can use ASF built-in **[WCF client](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF#client)** for executing ```!api``` command, and parse its standard output.

---

## Notice

API is a subject to change and can contain more/less information between different ASF versions. The objective of the API is to provide enough information about what ASF is doing, but not exposing every single property used by the program. It should work as equivalent to ```!status ASF``` done in API-like way. Of course, due to the fact that the output is supposed to be parsed and not read by humans, it also includes far more info.

---

## Structure

The example response of latest version has following form:

```
{
	"Bots": {
		"primary": {
			"CardsFarmer": {
				"GamesToFarm": [{
					"AppID": 403850,
					"GameName": "Sky To Fly: Faster Than Wind",
					"HoursPlayed": 0.7,
					"CardsRemaining": 2
				}, {
					"AppID": 467460,
					"GameName": "ZombieRush",
					"HoursPlayed": 0.0,
					"CardsRemaining": 3
				}],
				"CurrentGamesFarming": [{
					"AppID": 403850,
					"GameName": "Sky To Fly: Faster Than Wind",
					"HoursPlayed": 0.7,
					"CardsRemaining": 2
				}],
				"TimeRemaining":"02:30:00",
				"Paused": false
			},
			"AccountFlags":271073413,
			"SteamID": 76561198006963719,
			"BotConfig": {
				"SteamLogin": null,
				"SteamPassword": null
			},
			"KeepRunning": true
		},
		"secondary": {
			"CardsFarmer": {
				"GamesToFarm": [],
				"CurrentGamesFarming": [],
				"TimeRemaining":"00:00:00",
				"Paused": false
			},
			"AccountFlags":268976261,
			"SteamID": 0,
			"BotConfig": null,
			"KeepRunning": true
		}
	}
}
```

---

## Documentation

```Bots``` is a ```ConcurrentDictionary<string, Bot>``` object which maps bot name to its reference. In JSON, bot instances are displayed with their unique names.

---

### Bot

```CardsFarmer``` is specialized C# object used by Bot for cards-farming purpose. It provides information related to cards farming progress of given bot instance. Its structure is explained **[below](#cardsfarmer)**.

```AccountFlags``` is ```EAccountFlags``` (```uint``` flags) type (which is defined by SK2 **[here](https://github.com/SteamRE/SteamKit/blob/afda0753a3894c5c1fc4056aaf27ecc3f83426cb/Resources/SteamLanguage/enums.steamd#L81)**) that specifies Steam account flags of given account. This property can be used for getting more information about the status of Steam account being in ASF, for example if it's **[limited](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, by checking if ```LimitedUser``` or ```LimitedUserForce``` flags are set.

```SteamID``` is ```ulong``` unique steamID identificator of currently logged in account in 64-bit form. This property will have a value of ```0``` if bot is not logged in to Steam Network (therefore it can be used for telling if account is logged in or not).

```BotConfig``` is specialized C# object used by Bot for accessing to its config. It has exactly the same structure as **[bot config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** explained in configuration, and it also exposes all config variables available. This property can be used for determining with what options the bot is configured to work. Keep in mind that bot might also have invalid/broken config, in this case this property will be ```null```.

```KeepRunning``` is a ```bool``` type that specifies if bot is active. Active bot is a bot that has been ```!start```ed, either by ASF on startup, or by user later during execution. If bot is stopped, this property will be ```false```. Keep in mind that this property has nothing to do with bot being connected to Steam network, or not (that is what ```SteamID``` can be used for).

---

### CardsFarmer

```GamesToFarm``` is a ```ConcurrentHashSet<Game>``` (collection of ```Game``` elements) object that contains games pending to farm in current farming session. Please note that collection is updated on as-needed basis regarding performance. For example, in ```Simple``` cards farming algorithm ASF won't bother checking if we got any new games to farm when new game gets added (as we'd do that check anyway when we're out of queue, and by not doing so immediately we save requests and bandwidth). Therefore, this is data regarding current farming session, that might be different from overall data.

```CurrentGamesFarming``` is a ```ConcurrentHashSet<Game>``` (collection of ```Game``` elements) object that contains games being farmed right now. In comparison with ```GamesToFarm```, this property defines current status instead of pending queue, and it's heavily affected by currently selected cards farming algorithm. This collection can contain only up to ```32``` games (```MaxGamesPlayedConcurrently``` enforced by Steam Network).

```TimeRemaining``` is a ```TimeSpan``` type that specifies approximated time required to farm all games specified in ```GamesToFarm``` collection. This is nowhere close to the actual time that will be required, but it's a nice indicator with accuracy that might be improved in future, therefore it can be used for various display purposes. It's not updated in real-time, but calculated from current ```GamesToFarm``` status, therefore it's re-calculated the moment ```CardsRemaining``` of any game is modified.

```Paused``` is a ```bool``` type that specifies if ```CardsFarmer``` is currently paused. CardsFarmer can be paused due to various events, mainly ```!pause``` and ```!play``` commands. Paused CardsFarmer will not attempt to farm anything in automatic mode, neither will check badges every ```IdleFarmingPeriod``` hours.

---

### Game

```AppID``` is ```uint``` type that in unique way identifies game being played. ASF uses this identifier in ```PlayGames()``` request. ASF enforces this to be greater than ```0```.

```GameName``` is ```string``` type that provides name of game identified by ```AppID```. This is data returned by Steam Community. ASF enforces this to be ```non-null``` and ```non-empty```.

```HoursPlayed``` is ```float``` type that provides information how many hours the game has been played. This property is not updated in real time, but on as-needed basis, at least once per ```FarmingDelay``` minutes. Please note that initially this data is retrieved from Steam Community, but then updated according to ASF built-in timers, therefore it might not match what Steam Community is returning - this is because Steam Community data is not provided in real time either, and ASF requires such data for stopping farming for hours game as soon as it reaches ```2.0``` value. ASF enforces this property to be at least ```0.0```.

```CardsRemaining``` is ```ushort``` type that tells how many cards are remaining for the game. This property is updated as soon as possible and it should always have a value greater than ```0```. However, it is possible for this property to have ```0``` value for a short moment when ASF is switching game.

---

**Notice:** You can use standard ```Dictionary``` and ```HashSet``` types in place of ASF concurrent versions, as underlying data model is the same. This is especially useful if you use your data in single-threaded environment and you don't need concurrent access - there is no need for concurrent overhead then. ASF uses concurrent versions as it's multi-threaded and those collections can be accessed by multiple threads at the same time - you should serialize the data to types you find appropriate for your application. In documentation we use C# types from ASF source that are being serialized to JSON response, so you can better understand output structure (and why it looks like that).