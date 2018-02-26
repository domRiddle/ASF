# Background games redeemer

Background games redeemer is a special built-in ASF feature that allows you to import given set of Steam cd-keys (together with their names) to be redeemed in background. This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit `RateLimited` **[status](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** before you're done with your entire batch.

---

## Purpose

The purpose of background games redeemer is to load a batch of cd-keys to redeem **on a single account**, then get the output including keys that failed to redeem properly, for example because our account already owned selected games. This feature does not make use of `RedeemingPreferences` and is made to have a single bot scope, for simplicity reason. It can be used together with (or instead of) `!redeem` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**, if needed.

---

## Import

The import process can be done through two ways - either by using a file, or IPC.

### File

ASF will recognize in its `config` directory a file named `BotName.keys` where `BotName` is the name of your bot. That file has expected and fixed structure of name of the game with cd-key, separated by a tab character and ending with a newline. If multiple tabs are used, for example in a game name, then last tab counts, while previous tabs are considered to be a part of game's name, and will be converted to spaces instead. For example:

```
POSTAL 2	ABCDE-EFGHJ-IJKLM
Domino Craft VR	12345-67890-ZXCVB
A Week of Circus Terror	POIUY-KJHGD-QWERT
```

ASF will import such file, either on bot startup, or later during execution. After successful parse of your file and eventual omit of invalid entries, all properly detected games will be added to the background queue.

### IPC

In addition to using keys file mentioned above, ASF also exposes `GamesToRedeemInBackground` **[API endpoint](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)** which can be executed by any IPC tool, including our IPC GUI. Using IPC might be more powerful, as you can do appropriate parsing yourself, such as using a custom delimiter instead of being forced to a tab character.

---

## Queue

Once games are successfully imported, they're added to the queue. ASF automatically goes through its background queue as long as bot is connected to Steam network. All games that are successfully redeemed are automatically removed from the queue without further info. Games that failed to redeem properly because of a specific account condition (such as `AlreadyPurchased`) are appended to `BotName.keys.owned` file, in expected file format, with redeem status on the same line. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if wanted.

If during the process our account hits `RateLimited` status, the queue is temporarily suspended for a full hour in order to wait for cooldown to disappear. Afterwards, the process continues where it left, until the entire queue is empty.

---

## Example

Let's assume that we have a list of 100 keys. Firstly we should create a new `BotName.keys.new` file in ASF `config` directory. We appended `.new` extension in order to let ASF know that it shouldn't pick up this file immediately the moment it's created.

Now we can open our new file and copy-paste list of our 100 keys there, fixing the format if needed. After fixes our `BotName.keys.new` file will have exactly 100 (or 101, with last newline) lines, each line having a structure of `GameName\tcd-key\n`, where `\t` is tab character and `\n` is newline.

Now we can rename this file from `BotName.keys.new` to `BotName.keys` in order to let ASF know that it's ready to be picked up. The moment we do this, ASF will automatically import the file (without a need of restart) and delete it afterwards, confirming that all our games were parsed and added to the queue.

After some time, at least a few solid hours due to our cd-keys amount, a new `BotName.keys.owned` file might be generated. This file will contain those cd-keys from our original list that failed to redeem properly, for example because we owned some of the games already. We can copy-paste all of those entries into some new file and re-use them, since we can be sure that those cd-keys are still valid. Cd-keys that were successfully redeemed are not included in our output list.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

Instead of using a file, you could also use IPC API endpoint, or even combining both if you want to.

---

## Remarks

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file or API call. This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed before, but not after. For example, this means that if you have DLC `X` that requires game `A` to be activated firstly, then cd-key for game `A` should **always** be included before cd-key for DLC `X`. Likewise, if game `X` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp` in `BotName.keys.owned`, even if your account would be eligible for activating it after going through its entire queue. The entire purpose of using `OrderedDictionary` instead of typical non-ordered one was to provide you with a tool being capable to deal with potential dependencies.