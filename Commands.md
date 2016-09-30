ASF supports variety of commands, which can be used to control behaviour of the process and bot instances.

|Command                          | Description                                            | Examples                    |
| ------------------------------- |:-------------------------------------------------------|:----------------------------|
`!2fa`                            | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for current bot instance
`!2fa <BOT>`                      | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for given bot instance
`!2fano`                          | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for current bot instance
`!2fano <BOT>`                    | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instance
`!2faok`                          | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for current bot instance
`!2faok <BOT>`                    | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instance
`!addlicense <appID1,appID2...>`  | Activates given ```appIDs``` on current bot instance (free games only) | ```!addlicense 440,570```
`!addlicense <BOT> <appID1,appID2...>` | Activates given ```appIDs``` on given bot instance (free games only)
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
`!pause`                          | Pauses automatic farming of current bot instance. Also see ```!resume```
`!pause <BOT>`                    | Pauses automatic farming of given bot instance. Also see ```!resume```
`!play <appID1,appID2,...>`       | Switches to manual farming - launches given ```appIDs``` on current bot instance. Use ```!resume``` for returning to automatic farming | ```!play 440,570```, ```!play 570```
`!play <BOT> <appID1,appID2,...>` | Switches to manual farming - launches given ```appIDs``` on given bot instance. Use ```!resume``` for returning to automatic farming
`!redeem <key1,key2,...>`         | Redeems given ```cd-keys``` on current bot instance
`!redeem <BOT> <key1,key2,...>`   | Redeems given ```cd-keys``` on given bot instance
`!redeem^ <key1,key2,...>`        | Redeems given ```cd-keys``` on current bot instance, never forwards keys to other bots (like ```ForwardKeysToOtherBots``` and ```DistributeKeys``` of ```false```)
`!redeem^ <BOT> <key1,key2,...>`  | Redeems given ```cd-keys``` on given bot instance, never forwards keys to other bots (like ```ForwardKeysToOtherBots``` and ```DistributeKeys``` of ```false```)
`!rejoinchat`                     | Forces all bots with unlimited accounts to rejoin the groupchat
`!restart`                        | Restarts ASF process
`!resume`                         | Resumes automatic farming of current bot instance. Also see ```!pause```, ```!play```
`!resume <BOT>`                   | Resumes automatic farming of given bot instance. Also see ```!pause```, ```!play```
`!start <BOT>`                    | Starts given bot instance
`!status`                         | Prints status of current bot instance
`!status <BOT>`                   | Prints status of given bot instance
`!statusall`                      | Prints status of all bot instances and ASF itself
`!stop`                           | Stops current bot instance
`!stop <BOT>`                     | Stops given bot instance
`!update`                         | Checks GitHub for ASF updates (this is done automatically every 24 hours if ```AutoUpdates```)
`!version`                        | Prints version of ASF

Above commands can be sent to the bot through three different ways:
- Through steam private chat, by ```SteamMasterID```
- Through steam group chat, by ```SteamMasterID```
- Through **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

In addition to that, ```!pause``` command can also be executed by all users that have **[access to our shared library](https://store.steampowered.com/account/managedevices)**.

When using **WCF**, keep in mind that:
- Commands should **NOT** be prefixed by ```!```
- When using commands that are based on ```current bot instance```, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use ```given bot instance``` commands instead

Commands affecting ASF as a process, or more than one bot, typically require ```SteamOwnerID``` permission, for example ```!statusall```, ```!lootall```, ```!update``` or ```!exit```.