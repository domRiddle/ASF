# Trading

ASF includes support for Steam non-interactive (offline) trades. Both receiving (accepting/declining) as well as sending trades is available right away and doesn't require special configuration, but obviously requires unrestricted Steam account (the one that spent 5$ in the store already). Trading module is unavailable for restricted accounts.

---

## Logic

ASF will always accept all trades, regardless of items, sent from user with `Master` (or higher) access to the bot. This allows not only easily looting steam cards farmed by the bot instance, but also allows to easily manage Steam items that bot stashes in the inventory - including those from other games (such as CS:GO).

ASF will reject trade offer, regardless of content, from any (non-master) user that is blacklisted from trading module. Blacklist is stored in standard `BotName.db` database, and can be managed via `tb`, `tbadd` and `tbrm` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. This should work as an alternative to standard user block offered by Steam - use with caution.

ASF will accept all `loot`-like trades being sent across bots, unless `DontAcceptBotTrades` is specified in `TradingPreferences`. In short, default `TradingPreferences` of `None` will cause ASF to automatically accept trades from user with `Master` access to the bot (explained above), as well as all donation trades from other bots that are taking part in ASF process. If you want to disable donation trades from other bots, then that's what `DontAcceptBotTrades` in your `TradingPreferences` is for.

When you enable `AcceptDonations` in your `TradingPreferences`, ASF will also accept any donation trade - a trade in which bot account is not losing any items. This property affects only non-bot accounts, as bot accounts are affected by `DontAcceptBotTrades`. `AcceptDonations` allows you to easily accept donations from other people, and also bots that are not taking part in ASF process.

It's nice to note that `AcceptDonations` doesn't require **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, as there is no confirmation needed if we're not losing any items.

You can also further customize ASF trading capabilities by modifying `TradingPreferences` accordingly. One of the main `TradingPreferences` features is `SteamTradeMatcher` option which will cause ASF to use built-in logic for accepting trades that help you complete missing badges, which is especially useful in cooperation with public listing of **[SteamTradeMatcher](https://www.steamtradematcher.com)**, but can also work without it. It's further described below.