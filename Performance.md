# Performance

***

The primary objective of ASF is to farm as effectively as possible, based on two types of data it can operate on - small set of user-provided data that is impossible for ASF to guess/check on it's own, and larger set of data which can be automatically checked by ASF.

In automatic mode, ASF does not allow you to choose the games that should be farmed, neither allows you to change cards farming algorithm. **ASF knows better than you what it should do and what decisions it should make in order to farm as fast as possible**. Your objective is to set config properties properly, as ASF can't guess them on it's own, everything else is covered.

***

Some time ago Valve changed the algorithm for card drops. From that point onwards, we can categorize steam accounts by two categories: those **with** card drops restricted, and those **without**. The only difference between those two types is the fact that accounts with restricted card drops can't get any card from given game until they play given game for at least 2 hours. It seems that older accounts that never asked for refund have **unrestricted card drops**, while new accounts and those who did ask for refund have **restricted card drops**. This is however only theory, and should not be taken as a rule. That's why there is **no obvious answer**, and ASF relies on **you** telling it which case is appropriate for your account.

***

ASF currently includes two farming algorithms:

**Simple** algorithm works best for accounts which are not restricted by 2 hours cards drop. This is primary and default algorithm used by ASF. Bot finds games to farm, and farms them one-by-one until all cards are dropped.

**Complex** is new algorithm that has been implemented to help restricted accounts to maximize their profits as well. ASF will firstly use standard **Simple** algorithm on all games that passed 2 hours of playtime, then, if no games with >= 2 hours are left, it will farm all games (up to ```32``` limit) with < 2 hours left simultaneously, until any of them hits 2 hours mark, then ASF will continue the loop from beginning (use **Simple** on that game, return to simultaneous on < 2 etc.)

Currently, ASF chooses cards farming algorithm based purely on ```CardDropsRestricted``` config property (which is  set by **you**). If ```CardDropsRestricted``` is set to ```true```, **Complex** algorithm will be used, if not, **Simple** algorithm will be used instead.

***

### **There is no obvious answer which algorithm is better for you**.

This is one of the reasons why you do not choose cards farming algorithm, instead, you tell ASF if account has restricted drops or not. If account has non-restricted drops, **Simple** algorithm will **work better** on that account, as we won't be wasting time on bringing all games to 2 hours - cards drop ratio is close to 0% when farming multiple games. On the other hand, if your account has card drops restricted, **Complex** algorithm will be better for you, as there's no point in farming solo if game didn't reach 2 hours yet - so we'll farm **playtime** first, **then** cards in solo mode.

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

By default, ASF assumes that ```CardDropsRestricted``` is ```true```, as the negative effect of setting this to ```true``` when it should be ```false``` is smaller than done the other way. However, you should still tune this variable to match your account for maximum efficiency, as this is only a blind guess based on potential drawbacks and majority of users (so we're trying to choose "lesser evil" by default).

At the moment two above algorithms are enough for all currently possible account scenarios, in order to farm as effectively as possible, therefore it's not planned to add any other ones.

It's nice to note that ASF also includes ```Manual``` farming mode that can be activated by ```!play``` command. You can read more about it in **[Commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

---

## Steam glitches

Cards drop algorithm does not always work the way it should, and it's entirely possible for various Steam glitches to happen, such as cards being dropped on restricted accounts, cards being dropped on closing/switching the game, cards not dropping at all when game is being played, and likewise.

This section is mainly for people that are wondering why ASF doesn't do **X**, such as rapidly switching games to idle cards faster.

What is a **Steam glitch** - a specific action triggering **undefined** behaviour, which is **not intended, undocumented, and considered as a logic flaw**. It's **unreliable by definition**, which means that it can't be reproduced reliably with clean testing environment, and therefore, coded without resorting to hacks that are supposed to guess when glitch is happening and how to fight with it / abuse it. Typically it's temporary until developers fix the logic flaw, although some misc glitches can go unnoticed for a very long period of time.

A good example of what is considered as a **Steam glitch** is not that uncommon situation of dropping a card when game is being closed, which can be abused to some degree with idle master's game skip function.

- **Undefined behaviour** - you can't say if there will be 0 or 1 cards being dropped when you trigger the glitch.
- **Not intended** - based on past experience and behaviour of Steam network that doesn't result in same behaviour when sending a single request.
- **Undocumented** - it's clearly documented on Steam website how cards are being obtained, and **in every single place** it's clearly stated that it's obtained through **playing**, NOT closing games, getting achievements, games switching or launching 32 games concurrently.
- **Considered as a logic flaw** - closing game(s) or switching them should have no outcome on cards being dropped which are clearly stated to be obtained through **gaining playtime**.
- **Unreliable by definition, can't be reproduced reliably** - it doesn't work for everybody, and even if it did work for you once, it might no longer work for the second time.

Now once we realized what Steam glitch is, and the fact that cards being dropped when game gets closed **is** one, we can move on to the second point - **ASF is not abusing Steam network in any way by definition, and it's doing its best to comply with Steam ToS, its protocols and what is generally accepted**. Spamming Steam network with constant game opening/closing requests can be considered a **[DoS attack](https://en.wikipedia.org/wiki/Denial-of-service_attack)** and **directly violates [Steam Online Conduct](http://store.steampowered.com/online_conduct/?l=english)**.

> As a Steam subscriber you agree to abide by the following conduct rules.
>
> You will not:
>
> Institute attacks upon a Steam server or otherwise disrupt Steam.

It doesn't matter whether you're able to trigger Steam glitch with other programs (such as IM), and it also doesn't matter if you consider such behaviour as DoS attack, or not - it's up to Valve to judge this, but if we consider it as exploiting/abusing non-intended behaviour through excessive Steam network requests, then you can be pretty sure that Valve will have similar view on this.

ASF is **never** going to take advantage of Steam exploits, abuses, hacks or any other activity that we see as **illegal or unwanted** according to Steam ToS, Steam Online Conduct or any other trusted source that could indicate that ASF activity is unwanted by Steam network, as stated in **[contributing](https://github.com/JustArchi/ArchiSteamFarm/blob/master/CONTRIBUTING.md)** section.

If you want at all cost to risk your Steam account for idling a few cent cards faster than usual, then sadly ASF will never offer something like this in automatic mode, although you still have ```!play``` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** that can be used as a tool for doing whatever you want in terms of Steam network interaction. We do not recommend taking advantage of Steam glitches and exploiting them for your own gain - not only with ASF, but with any other tool as well. In the end however, it's your account, and your choice what you want to do with it - just keep in mind that we warned you.