# API

ASF supports API-like response through ```!api``` command, which can be used for easily integrating ASF process status with third-party wrappers.

---

## Notice

API is a subject to change and can contain more/less information between different ASF versions. The objective of the API is to provide enough information about what ASF is doing, but not exposing every single property used by the program. It should work as equivalent to ```!statusall``` done in API-like way.

---

## Structure

The example response of latest version has following form:

```
{
	"Bots": {
		"archi": {
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
				"ManualMode": false
			},
			"KeepRunning": true
		},
		"1": {
			"CardsFarmer": {
				"GamesToFarm": [],
				"CurrentGamesFarming": [],
				"ManualMode": false
			},
			"KeepRunning": true
		}
	}
}
```

---

## Documentation

```Bots``` is a ```ConcurrentDictionary<string, Bot>``` object which maps bot name to it's reference. In JSON, bot instances are displayed with their unique names.

---

### Bot

```CardsFarmer``` is specialized C# object used by Bot for cards-farming purpose. It provides information related to cards farming progress of given bot instance.

```KeepRunning``` is a ```bool``` type that specifies if bot is active. Active bot is a bot that has been ```!start```ed, either by ASF on startup, or by user later during execution. If bot is stopped, this property will be ```false```. Keep in mind that this property has nothing to do with bot being connected to Steam network, or not.

---

### CardsFarmer

```GamesToFarm``` is a ```ConcurrentHashSet<Game>``` (collection of ```Game``` elements) object that contains games pending to farm in current farming session. Please note that collection is updated on as-needed basis regarding performance. For example, in ```Simple``` cards farming algorithm ASF won't bother checking if we got any new games to farm when new game gets added (as we'd do that check anyway when we're out of queue, and by not doing so immediately we save requests and bandwidth). Therefore, this is data regarding current farming session, that might be different from overall data.

```CurrentGamesFarming``` is a ```ConcurrentHashSet<Game>``` (collection of ```Game``` elements) object that contains games being farmed right now. In comparison with ```GamesToFarm```, this property defines current status instead of pending queue, and it's heavily affected by currently selected cards farming algorithm. This collection can contain only up to ```32``` games (```MaxGamesPlayedConcurrently``` enforced by Steam Network).

```ManualMode``` is a ```bool``` type that specifies if ```CardsFarmer``` is running in manual mode. Manual mode means that user is either playing his own specified game through ```!play``` command, or bot is ```!pause```d.

---

### Game

```AppID``` is ```uint``` type that in unique way identifies game being played. ASF uses this identifier in ```PlayGames()``` request. ASF enforces this to be greater than ```0```.

```GameName``` is ```string``` type that provides name of game identified by ```AppID```. This is data returned by Steam Community. ASF enforces this to be ```non-null``` and ```non-empty```.

```HoursPlayed``` is ```float``` type that provides information how many hours the game has been played. This property is not updated in real time, but on as-needed basis, at least once per ```FarmingDelay``` minutes. Please note that initially this data is retrieved from Steam Community, but then updated according to ASF built-in timers, therefore it might not match what Steam Community is returning - this is because Steam Community data is not provided in real time either, and ASF requires such data for stopping farming for hours game as soon as it reaches ```2.0``` value. ASF enforces this property to be at least ```0.0```.

```CardsRemaining``` is ```ushort``` type that tells how many cards are remaining for the game. This property is updated as soon as possible and it should always have a value greater than ```0```. However, it is possible for this property to have ```0``` value for a short moment when ASF is switching game.

---

**Notice:** You can use standard ```Dictionary``` and ```HashSet``` types in place of ASF concurrent versions, as underlying data model is the same. This is especially useful if you use your data in single-threaded environment and you don't need concurrent access - there is no need for concurrent overhead then. ASF uses concurrent versions as it's multi-threaded and those collections can be accessed by multiple threads at the same time - you should serialize the data to types you find appropriate for your application.