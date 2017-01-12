ASF supports variety of commands, which can be used to control behaviour of the process and bot instances.

Below commands can be sent to the bot through three different ways:
- Through steam private chat, by ```SteamMasterID```
- Through steam group chat, by ```SteamMasterID```
- Through **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

|Command                          | Description                                            | Examples                    |
| ------------------------------- |:-------------------------------------------------------|:----------------------------|
`!2fa`                            | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for current bot instance
`!2fa <BOT>`                      | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for given bot instance
`!2fano`                          | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for current bot instance
`!2fano <BOT>`                    | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instance
`!2faok`                          | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for current bot instance
`!2faok <BOT>`                    | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instance
`!addlicense <appID1,appID2...>`  | Activates given ```appIDs``` (Steam Network) or ```subIDs``` (Steam Store) on current bot instance (free games only) | ```!addlicense 440,570```
`!addlicense <BOT> <appID1,appID2...>` | Activates given ```appIDs``` (Steam Network) or ```subIDs``` (Steam Store) on given bot instance (free games only)
`!api`                            | Returns ASF process status in JSON, check **[API](https://github.com/JustArchi/ArchiSteamFarm/wiki/API)** for more info
`!exit`                           | Stops whole ASF
`!farm`                           | Restarts cards farming module for current bot instance
`!farm <BOT>`                     | Restarts cards farming module for given bot instance
`!help`                           | Shows help (link to this page)
`!leave`                          | Makes bot leave the current group chat. For obvious reasons, this command works only in group chats
`!loot`                           | Sends all booster packs and Steam trading cards (including foils if ```IsBotAccount```) of current bot instance to ```SteamMasterID```
`!loot <BOT>`                     | Sends all booster packs and Steam trading cards (including foils if ```IsBotAccount```) of given bot instance to ```SteamMasterID```
`!lootall`                        | Issues ```!loot``` on all currently enabled ASF bots
`!owns <appID,gameName...>`       | Checks if current bot instance already owns given ```appIDs``` and/or ```gameNames``` (can be part of the game's name) | ```!owns 440,570```, ```!owns 440,dota```, ```!owns roach```
`!owns <BOT> <appID,gameName...>` | Checks if given bot instance already owns given ```appIDs``` and/or ```gameNames``` (can be part of the game's name)
`!ownsall <appID,gameName...>`    | Checks all currently enabled ASF bots for owning given ```appIDs``` and/or ```gameNames``` (can be part of the game's name)
`!password`                       | Prints encrypted password of current bot instance (in use with ```PasswordFormat```)
`!password <BOT>`                 | Prints encrypted password of given bot instance (in use with ```PasswordFormat```)
`!pause`                          | Temporarily pauses automatic farming of current bot instance. Farming will be automatically resumed on the next playing event, or bot disconnect. You can ```!resume``` farming to unpause it.
`!pause <BOT>`                    | Temporarily pauses automatic farming of given bot instance. Farming will be automatically resumed on the next playing event, or bot disconnect. You can ```!resume``` farming to unpause it.
`!pause^`                         | Permanently pauses automatic farming of current bot instance. ASF will not attempt to farm current account in this session, unless you manually ```!resume``` it, or restart the process. Also called sticky pause.
`!pause^ <BOT>`                   | Permanently pauses automatic farming of given bot instance. ASF will not attempt to farm current account in this session, unless you manually ```!resume``` it, or restart the process. Also called sticky pause.
`!play <appID1,appID2,...>`       | Switches to manual farming - launches given ```appIDs``` on current bot instance. Use ```!resume``` for returning to automatic farming | ```!play 440,570```, ```!play 570```
`!play <BOT> <appID1,appID2,...>` | Switches to manual farming - launches given ```appIDs``` on given bot instance. Use ```!resume``` for returning to automatic farming
`!redeem <key1,key2,...>`         | Redeems given ```cd-keys``` on current bot instance
`!redeem <BOT> <key1,key2,...>`   | Redeems given ```cd-keys``` on given bot instance
`!redeem^ <key1,key2,...>`        | Redeems given ```cd-keys``` on current bot instance, never forwards keys to other bots (like ```ForwardKeysToOtherBots``` and ```DistributeKeys``` of ```false```)
`!redeem^ <BOT> <key1,key2,...>`  | Redeems given ```cd-keys``` on given bot instance, never forwards keys to other bots (like ```ForwardKeysToOtherBots``` and ```DistributeKeys``` of ```false```)
`!redeem& <key1,key2,...>`        | Redeems given ```cd-keys``` on **any** bot instance **apart from** current one (forces ```ForwardKeysToOtherBots``` of ```true```)
`!redeem& <BOT> <key1,key2,...>`  | Redeems given ```cd-keys``` on **any** bot instance **apart from** given one (forces ```ForwardKeysToOtherBots``` of ```true```)
`!rejoinchat`                     | Forces all bots with unlimited accounts to rejoin the ```SteamMasterClanID``` groupchat
`!restart`                        | Restarts ASF process
`!resume`                         | Resumes automatic farming of current bot instance. Also see ```!pause```, ```!play```
`!resume <BOT>`                   | Resumes automatic farming of given bot instance. Also see ```!pause```, ```!play```
`!start <BOT>`                    | Starts given bot instance
`!startall`                       | Starts all inactive bot instances
`!status`                         | Prints status of current bot instance
`!status <BOT>`                   | Prints status of given bot instance
`!statusall`                      | Prints status of all bot instances and ASF itself
`!stop`                           | Stops current bot instance
`!stop <BOT>`                     | Stops given bot instance
`!update`                         | Checks GitHub for ASF updates (this is done automatically every 24 hours if ```AutoUpdates```)
`!version`                        | Prints version of ASF

---

### Notes

All commands are case-insensitive, but their arguments (such as bot names) are usually case-sensitive.

---

```!pause``` command can also be executed by up to 5 users that **[have access to our shared library](https://store.steampowered.com/account/managedevices)**, in addition to usual ```SteamMasterID```.

---

Commands affecting ASF as a process, or more than one bot, typically require ```SteamOwnerID``` permission, for example ```!statusall```, ```!lootall```, ```!update``` or ```!exit```. ```SteamMasterID``` has access only to his bot instances, while entire ASF process is owned by ```SteamOwnerID```.

---

Please note that sending a command to the group chat acts like a relay - if you're saying ```!redeem X``` to 3 of your bots sitting together with you on the group chat, it'll result in the same as you'd say ```!redeem X``` to every single one of them privately. In most cases **this is not what you want**, and instead you should use ```given bot``` command that is being sent to a single bot privately. ASF supports group chat, as in many cases it can be useful source for communication with it, but you should almost never execute any command on the group chat if there are 2 or more ASF bots sitting there, unless you fully understand ASF behaviour written here and you in fact want to relay the same command to every single bot that is listening to you.

---

Some commands are also available with their aliases, to save you on typing:

| Command  | Alias  |
|----------|--------|
`!redeem`  | `!r`   |
`!redeem^` | `!r^`  |
`!redeem&` | `!r&`  |

---

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set ```SteamMasterClanID``` properly to that newly created group, then set ```SteamMasterID``` as yourself. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

---

When using **WCF**, keep in mind that:
- Commands should **NOT** be prefixed by ```!```
- When using commands that are based on ```current bot instance```, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use ```given bot instance``` commands instead.