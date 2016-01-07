Current commands (as of ASF v1.3):

- `!2fa` Generates temporary 2FA token for current bot instance
- `!2fa <BOT>` Generates temporary 2FA token for given bot instance
- `!2faoff` Deactivates 2FA for current bot instance
- `!2faoff <BOT>` Deactivates 2FA for given bot instance
- `!exit` Stops whole ASF
- `!redeem <KEY>` Redeems cd-key on current bot instance
- `!redeem <BOT> <KEY>` Redeems cd-key on given bot instance
- `!start <BOT>` Starts given bot instance
- `!status` Prints current status of ASF
- `!stop` Stops current bot instance
- `!stop <BOT>` Stops given bot instance

Above commands can be sent to the bot through three different ways:
- Through steam private chat, by ```SteamMasterID```
- Through steam group chhat, by ```SteamMasterID```
- Through **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

When using **WCF**, keep in mind that:
- Commands should **NOT** be prefixed by '''!'''
- When using commands that are based on ```current bot instance```, ASF will choose **any** of currently enabled bots, therefore I highly recommend using ```given bot instance``` commands instead