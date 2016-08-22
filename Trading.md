# Trading

ASF includes support for Steam non-interactive (offline) trades. Receiving trades as well as responding to them (accepting/declining) requires valid ```SteamApiKey``` set for given bot instance, and will not work without it. Sending trades doesn't have any special requirements.

***

## Logic

ASF will always allow ```SteamMasterID``` of given bot instance to ```!loot``` it, as well as accept all trades, regardless of items, sent from ```SteamMasterID``` to the bot. This allows not only easily looting steam cards farmed by the bot instance, but also allows to easily manage Steam items that bot stashes in the inventory.

In addition to ```SteamMasterID```, ASF will also accept any donation trade - a trade in which bot account is not losing any items. This allows you to not only accept donations from other people, but also send bot -> bot trades even when other bot doesn't match configured ```SteamMasterID```.

Apart from that, you can also extend ASF trading capabilities by switching ```SteamTradeMatcher``` to ```true```. When this option is active, ASF will also use built-in logic for accepting trades that help you complete missing badges, which is especially useful in cooperation with public listing of **[SteamTradeMatcher](http://www.steamtradematcher.com/)**, but can also work without it.

***

## SteamTradeMatcher

When ```SteamTradeMatcher``` is active, ASF will use quite complex algorithm of checking if trade passes STM rules and is at least neutral towards us. The actual logic is following:

- Reject the trade if we're losing anything but non-foil Steam trading cards.
- Reject the trade if we're not receiving at least the same number of cards on per-game basis.
- Reject the trade if user asks for special Steam summer/winter sale cards, and has a trade hold.
- Reject the trade if trade hold duration exceeds ```MaxTradeHoldDuration``` global config property.
- Reject the trade if it's worse than neutral for us.

Notice: "Reject" of the trade will be either ignore, or decline, depending on configured ```IsBotAccount``` property. It's nice to note that ASF also supports overpaying - the logic will work properly when user is adding something extra to the trade, as long as all above conditions are met.

First 4 predicates should be obvious for everyone. Last predicate includes actual dupes logic which checks current state of our inventory and decides what is the status of the trade.

- Trade is good for us if our progress towards badge completion advances. Example: A A -> A B
- Trade is neutral for us if our progress towards badge completion doesn't change. Example: A B -> A C
- Trade is bad for us if our progress towards badge completion declines. Example: A B -> A A

STM operates only on good trades, which means that user using STM for dupes matching should always suggest only good trades for us. However, ASF is liberal, and it also accepts neutral trades, because in those trades we're not actually losing anything, so there is no real reason why to not accept such trade. ASF will, however, reject any bad trade for us.

Although using ASF STM module doesn't mean that you can't accept such trades. If you kept default value of ```IsBotAccount``` which is ```false```, ASF will just ignore those trades - allowing you to decide yourself if you're interested in it or not. Same goes for backgrounds/emoticons trades, as well as everything else - the module is supposed to help you automate STM trades, not decide for you what is worth for you and what is not.

It's highly recommended to use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** when you enable this option, as this function loses it's whole potential if you decide to manually confirm every trade. ```SteamTradeMatcher``` will work properly even without ability to confirm trades, but manually confirming everything defeats the whole purpose of this option.