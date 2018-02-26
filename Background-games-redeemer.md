# Background games redeemer

Background games redeemer is a special built-in ASF feature that allows you to import given set of Steam cd-keys (together with their names) to be redeemed in background. This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit `RateLimited` **[status](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** before you're done with your entire batch.

## Purpose

The purpose of background games redeemer is to load a batch of cd-keys to redeem **on a single account**, then get the output containing keys that failed to redeem properly, for example because our account already owned selected games. This feature does not make use of `RedeemingPreferences` and is made to have a single bot scope, for simplicity reason.

---

## Import

The import process can be done through two ways - either by using a file, or IPC.

### File

ASF will recognize in its `config` directory a file named `BotName.keys` where `BotName` is the name of your bot. That file has expected and fixed structure of name of the game, separated by a tab character from cd-key, ending with a newline character. If multiple tabs are used, for example in game name, then last tab counts, while previous tabs are considered to be a part of game's name, and will be converted to spaces instead. For example:

```
POSTAL 2	ABCDE-EFGHJ-IJKLM
Domino Craft VR	12345-67890-ZXCVB
A Week of Circus Terror	POIUY-KJHGD-QWERT
```

ASF will import such file, either on bot startup, or later during execution. After successful parse of your file and eventual omit of invalid entries, all properly detected games will be added to the queue.

### IPC

In addition to using keys file mentioned above, ASF also exposes **[GamesToRedeemInBackground API endpoint](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)** which can be executed by any IPC tool, including our IPC GUI. Using IPC might be more powerful, as you can do appropriate parsing yourself, such as using a custom delimiter instead of being forced to a tab character.

---

## Queue

Once games are successfully imported, they're added to the queue. ASF automatically goes through its background games to redeem queue as long as bot is connected to Steam network. All games that are successfully redeemed are automatically removed from the queue without further info. Games that failed to redeem properly because of a specific account condition (such as `AlreadyPurchased`) are appended to `BotName.keys.owned` file, in expected file format, with appended redeem status on the same line.

If during the process our account hits `RateLimited` status, the action is temporarily suspended for a full hour in order to wait for cooldown to end. Afterwards, the process continues where it left, until the entire queue is empty.