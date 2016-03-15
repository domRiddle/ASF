ASF supports variety of commands, which can be used to control behaviour of the process and bot instances.

|Command                                     | Description                                                               |
| ------------------------------------------ |:--------------------------------------------------------------------------|
`!2fa`                            | Generates temporary 2FA token for current bot instance
`!2fa <BOT>`                      | Generates temporary 2FA token for given bot instance
`!2faok`                          | Accepts all pending confirmations for current bot instance
`!2faok <BOT>`                    | Accepts all pending confirmations for given bot instance
`!2faoff`                         | Deactivates 2FA for current bot instance
`!2faoff <BOT>`                   | Deactivates 2FA for given bot instance
`!addlicense <appID>`             | Activates given ```appID``` on current bot instance (free games only)
`!addlicense <BOT> <appID>`       | Activates given ```appID``` on given bot instance (free games only)
`!farm`                           | Restarts cards farming module for current bot instance
`!farm <BOT>`                     | Restarts cards farming module for given bot instance
`!exit`                           | Stops whole ASF
`!leave`                          | Makes bot leave the current group chat. For obvious reasons, this command works only in group chats
`!loot`                           | Sends all Steam items of current bot instance to ```SteamMasterID```
`!loot <BOT>`                     | Sends all Steam items of given bot instance to ```SteamMasterID```
`!owns <appID>`                   | Checks if current bot instance already owns given ```appID```
`!owns <BOT> <appID>`             | Checks if given bot instance already owns given ```appID```
`!owns <gameName>`                | Checks if current bot instance already owns any games named ```gameName``` (can be part of the game's name)
`!owns <BOT> <gameName>`          | Checks if given bot instance already owns any games named ```gameName``` (can be part of the game's name)
`!play <appID1,appID2,...>`       | Switches to manual farming - launches given ```appIDs``` on current bot instance. Use ```0``` appID to return to automatic farming
`!play <BOT> <appID1,appID2,...>` | Switches to manual farming - launches given ```appIDs``` on given bot instance. Use ```0``` appID to return to automatic farming
`!redeem <KEY>`                   | Redeems cd-key on current bot instance
`!redeem <BOT> <KEY>`             | Redeems cd-key on given bot instance
`!rejoinchat`                     | Forces all bots with unlimited accounts to rejoin the groupchat
`!restart`                        | Restarts ASF process
`!start <BOT>`                    | Starts given bot instance
`!status`                         | Prints status of current bot instance
`!status <BOT>`                   | Prints status of given bot instance
`!statusall`                      | Prints status of all bot instances and ASF itself
`!stop`                           | Stops current bot instance
`!stop <BOT>`                     | Stops given bot instance
`!update`                         | Checks GitHub for ASF updates (this is done automatically every 24 hours)

Above commands can be sent to the bot through three different ways:
- Through steam private chat, by ```SteamMasterID```
- Through steam group chat, by ```SteamMasterID```
- Through **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

When using **WCF**, keep in mind that:
- Commands should **NOT** be prefixed by ```!```
- When using commands that are based on ```current bot instance```, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use ```given bot instance``` commands instead