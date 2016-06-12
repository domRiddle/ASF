# Trading

ASF included support for Steam non-interactive (offline) trades. Receiving trades as well as responding to them (accepting/rejecting) requires valid ```SteamApiKey``` set for given bot instance. Sending trades doesn't have any special requirements.

***

## Logic

ASF will always allow ```SteamMasterID``` of given bot instance to ```!loot``` it, as well as accept all trades, regardless of items, sent from ```SteamMasterID``` to the bot. This allows not only easily looting steam cards farmed by the bot instance, but also allows to easily manage Steam items that bot stashes in the inventory.

In addition to ```SteamMasterID```, ASF will also accept any donation trade - a trade in which bot account is not losing any items. This allows you to not only accept donations from other people, but also send bot -> bot trades even when other bot doesn't match configured ```SteamMasterID```.

Apart from that, you can also extend ASF trading capabilities by switching ```SteamTradeMatcher``` to ```true```. When this option is active, ASF will also use built-in logic for accepting trades that help you complete missing badges, which is especially useful in cooperation with public listing of **[SteamTradeMatcher](http://www.steamtradematcher.com/)**, but can also work without it.

When ```SteamTradeMatcher``` is active, ASF will also accept trades that:
- We're losing only normal (non-foil) steam trading cards
- TODO...