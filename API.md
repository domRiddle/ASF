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
   "Bots":{
      "1":{
         "CardsFarmer":{
            "GamesToFarm":[

            ],
            "CurrentGamesFarming":[

            ],
            "ManualMode":false
         },
         "KeepRunning":true
      },
      "archi":{
         "CardsFarmer":{
            "GamesToFarm":[
               // TODO
            ],
            "CurrentGamesFarming":[
               429280
            ],
            "ManualMode":false
         },
         "KeepRunning":true
      }
   }
}
```

---

## Documentation

```Bots``` is a ```Dictionary<string, Bot>``` object which maps bot name to it's reference. In JSON, bot instances are displayed with their unique names.

---

### Bot

```CardsFarmer``` is specialized C# object used by Bot for cards-farming purpose. It provides information related to cards farming progress of given bot instance.

```KeepRunning``` is a ```bool``` type that specifies if bot is active. Active bot is a bot that has been ```!start```ed, either by ASF on startup, or by user later during execution. If bot is stopped, this property will be ```false```.

---

### CardsFarmer

```GamesToFarm``` is a ```Dictionary<uint, float>``` object that maps appIDs to their current playtime, and contains games left to farm in this session. Please note that playtime is initially retrieved from Steam Community and updated only in ```Complex``` cards farming algorithm, until game reaches 2.0+. ASF won't bother updating data retrieved from Steam if there is no reason to (so when we don't need to farm hours for given appID).

```CurrentGamesFarming``` is a ```HashSet<uint>``` object that contains appIDs of the games we're farming right now. In comparison with ```GamesToFarm```, ```CurrentGamesFarming``` contains only appIDs that are being farmed at this moment (either for hours, or solo). It also doesn't contain playtime, which can be found in ```GamesToFarm```.

```ManualMode``` is a ```bool``` type that specifies if ```CardsFarmer``` is running in manual mode. Manual mode means that user is either playing his own specified game through ```!play``` command, or bot is ```!pause```d.