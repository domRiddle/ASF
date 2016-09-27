# Performance

***

The primary objective of ASF is to farm as effectively as possible, based on two types of data it can operate on - small set of user-provided data that is impossible for ASF to guess/check on it's own, and larger set of data which can be automatically checked by ASF.

In automatic mode, ASF does not allow you to choose the games that should be farmed, neither allows you to change cards farming algorithm. ASF knows better than you what it should do and what decisions it should make in order to farm as fast as possible. Your objective is to set config properties properly, as ASF can't guess them on it's own, everything else is covered.

***

Some time ago Valve changed the algorithm for card drops. From that point onwards, we can categorize steam accounts by two categories: those **with** card drops restricted, and those **without**. The only difference between those two types is the fact that accounts with restricted card drops can't get any card from given game until they play given game for at least 2 hours. It seems that older accounts that never asked for refund have **unrestricted card drops**, while new accounts and those who did ask for refund have **restricted card drops**. This is however only theory, and should not be taken as a rule. That's why there is **no obvious answer**, and ASF relies on **you** telling it which case is appropriate for your account.

***

ASF currently includes two farming algorithms:

**Simple** algorithm works best for accounts which are not restricted by 2 hours cards drop. This is primary and default algorithm used by ASF. Bot finds games to farm, and farms them one-by-one until all cards are dropped.

**Complex** is new algorithm that has been implemented to help restricted accounts to maximize their profits as well. ASF will firstly use standard **Simple** algorithm on all games that passed 2 hours of playtime, then, if no games with >= 2 hours are left, it will farm all games (up to ```32``` limit) with < 2 hours left simultaneously, until any of them hits 2 hours mark, then ASF will continue the loop from beginning (use **Simple** on that game, return to simultaneous on < 2 etc.)

Currently, ASF chooses cards farming algorithm based purely on ```CardDropsRestricted``` config property (which is  set by **you**). If ```CardDropsRestricted``` is set to ```true```, **Complex** algorithm will be used, if not, **Simple** algorithm will be used instead.

***

**There is no obvious answer which algorithm is better for you**.

This is one of the reasons why you do not choose cards farming algorithm, instead, you tell ASF if account has restricted drops or not. If account has non-restricted drops, **Simple** algorithm will **work better** on that account, as we won't be wasting time on bringing all games to 2 hours. On the other hand, if your account has card drops restricted, **Complex** algorithm will be better for you, as there's no point in farming solo if game didn't reach 2 hours yet.

Don't blindly set ```CardDropsRestricted``` only because somebody told you to - do tests, compare results, and based on data you get, decide which option should be better for you. If you put some minimal effort into that, you'll ensure that ASF is working with maximum possible efficiency for your account, which is probably what you want, considering that you're reading this wiki page right now. If there was a solution that works for everybody, you'd not be given a choice - ASF would decide itself.

***

### What is the best way to find out if your account is restricted?

Make sure you have some games to farm, preferably 5+, and run ASF with ```CardDropsRestricted``` of ```false```. It would be a good idea if you didn't play anything during farming period for more accurate results (best to run ASF during the night). Let ASF farm those 5 games, and after that check out the log for results.

If you notice that **at least one** game took less than 2 hours to farm:

```
[*] INFO: FarmSolo() <archi> Done farming: 269250 after 01:30 hours of playtime!
```

Then it means that your account is **not** restricted, so you should keep ```CardDropsRestricted``` at ```false```. Keep in mind that result a bit higher such as 2:30-2:40 can also be acceptable and suggest that your account is not restricted.

However, if you notice that **every** game takes more than 2-4 hours to farm, and you're not getting any card drops before game hits those 2 hours, then your account is **probably** restricted and you should set ```CardDropsRestricted``` to ```true```. Keep in mind I said **probably** - every game has different "difficulty" of farming, and time required for farming varies from game to game. That's why you should not make a decision based on one game only - one game being farmed in less than 2 hours is enough to say that your account is not restricted, but to confirm that your account is restricted you need to test at least 5 or more games, to ensure that it's truly a case and not a coincidence of farming specific game with harder difficulty.

ASF also tells you status of card drops remaining, so you can easily check if any card dropped in less than 2 hours since start.

```
2016-08-03 04:40:46|INFO|archi|ShouldFarm() Status for 440540 (Ara Fell): 3 cards remaining
(...)
2016-08-03 05:10:54|INFO|archi|ShouldFarm() Status for 440540 (Ara Fell): 2 cards remaining
```

The mentioned game didn't pass 2 hours yet, and the first card dropped after around 30 minutes, so **definitely** our ```CardDropsRestricted``` should be ```false```. If on the other hand you'd see that no card dropped before 2h mark, and you can reproduce that with several other games (preferably 5+), then **probably** you should set ```CardDropsRestricted``` to ```true```.

In the end, of course, decision is up to you.

And to make it even worse - I experienced cases when people switched from restricted to unrestricted state and vice versa - either because of Steam bug (oh yeah, we have many of those), or because of some logic adjustments by Valve. So even if you confirmed that your account is restricted (or not), do not believe that it'll stay like that - in order to switch from unrestricted to restricted it's enough to ask for a refund. If you're not sure, you can always switch to ```false``` and test again if your account is restricted (or not) using the same method explained above whenever you feel like it.

***

By default, ASF assumes that ```CardDropsRestricted``` is ```true```, as the negative effect of setting this to ```true``` when it should be ```false``` is smaller than done the other way. However, you should still tune this variable to match your account for maximum efficiency.

At the moment two above algorithms are enough for all currently possible account scenarios, in order to farm as effectively as possible, therefore it's not planned to add any other ones.

It's nice to note that ASF also includes ```Manual``` farming mode that can be activated by ```!play``` command. You can read more about it in **[Commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.