Current commands (as of ASF v1.4):

- `!2fa` Generates temporary 2FA token for current bot instance
- `!2fa <BOT>` Generates temporary 2FA token for given bot instance
- `!2faoff` Deactivates 2FA for current bot instance
- `!2faoff <BOT>` Deactivates 2FA for given bot instance
- `!addlicense <appID>` Activates given ```appID``` on current bot instance (free games only)
- `!addlicense <BOT> <appID>` Activates given ```appID``` on given bot instance (free games only)
- `!exit` Stops whole ASF
- `!loot` Sends all Steam items of current bot instance to ```SteamMasterID```
- `!loot <BOT>` Sends all Steam items of given bot instance to ```SteamMasterID```
- `!play <appID>` Switches to manual farming - launches given ```appID``` on current bot instance.
- `!play <BOT> <appID>` Switches to manual farming - launches given ```appID``` on given bot instance.
- `!redeem <KEY>` Redeems cd-key on current bot instance
- `!redeem <BOT> <KEY>` Redeems cd-key on given bot instance
- `!start <BOT>` Starts given bot instance
- `!status` Prints status of current bot instance
- `!status <BOT>` Prints status of given bot instance
- `!statusall` Prints status of all bot instances and ASF itself
- `!stop` Stops current bot instance
- `!stop <BOT>` Stops given bot instance

Above commands can be sent to the bot through three different ways:
- Through steam private chat, by ```SteamMasterID```
- Through steam group chhat, by ```SteamMasterID```
- Through **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

When using **WCF**, keep in mind that:
- Commands should **NOT** be prefixed by ```!```
- When using commands that are based on ```current bot instance```, ASF will choose **any** of currently enabled bots, therefore I highly recommend using ```given bot instance``` commands instead