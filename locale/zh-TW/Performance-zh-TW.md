# 效能

ASF的主要目標是盡可能地高效掛卡，它基於兩種可操作的資料──少部分由使用者提供、ASF無法自行猜測／檢查的資料，與大部分可由ASF自動獲得的資料。

在自動模式下，ASF不允許您選擇要掛卡的遊戲，您也無法更改掛卡演算法。 **ASF比您更清楚它該做什麼，與做什麼能最快地掛卡**&#8203;。 您的目標是正確設定設定屬性，因為ASF無法自行猜測它，而其他東西都已涵蓋在內。

---

在前段時間，Valve修改了交換卡片的掉落演算法。 自那時起，我們可以把Steam帳號分成兩種：交換卡片掉落&#8203;**受限**&#8203;與&#8203;**不受限**&#8203;。 兩種帳號間的唯一區別在於，掉卡受限的帳號在遊玩指定遊戲至少&#8203;`X`&#8203;小時之前，都無法獲得任何交換卡片。 看起來，從未要求退款的老帳號&#8203;**掉卡不受限**&#8203;，而曾經要求退款的新帳號則會&#8203;**受限**&#8203;。 然而，這只是理論，不應將它視為規則。 這就是為什麼&#8203;**沒有絕對的判斷**&#8203;，所以ASF需要&#8203;**您**&#8203;來告訴它，您的帳號符合哪種情形。

---

目前ASF有兩種掛卡演算法：

**簡單**&#8203;演算法適用於掉卡不受限制的帳號。 這是ASF使用的主要演算法。 Bot檢測需掛卡的遊戲，逐個掛卡直到所有交換卡片都掉落。 這是因為同時掛卡多個遊戲時，掉卡率會接近零使效率低落。

**複雜**&#8203;是種新型演算法，幫助受到限制的帳號最大化效益。 ASF首先會對所有遊玩時數超過&#8203;`HoursUntilCardDrops`&#8203;小時的遊戲使用標準&#8203;**簡單**&#8203;演算法，然後若沒有遊戲的時間剩餘>= &#8203;`HoursUntilCardDrops`&#8203;小時，它會同時掛所有時間剩餘< &#8203;`HoursUntilCardDrops`&#8203;小時的遊戲（限制最多&#8203;`32`&#8203;個），直到其中一個達到&#8203;`HoursUntilCardDrops`&#8203;小時，之後ASF將從頭開始循環此過程（對遊戲使用&#8203;**簡單**&#8203;，且剩餘時間< &#8203;`HoursUntilCardDrops`&#8203;小時，依此類推）。 在這種情形下，我們可以同時掛卡多個遊戲，來增加遊戲的時間，使它們先達到適當的遊戲時長。 請注意，在掛卡時，ASF&#8203;**不會**&#8203;掛交換卡片，因此，它不會檢查這期間是否有卡片掉落（原因如上所述）。

目前，ASF完全依據&#8203;`HoursUntilCardDrops`&#8203;設定屬性（是由&#8203;**您**&#8203;所設定的）來選擇掛卡演算法。 若&#8203;`HoursUntilCardDrops`&#8203;設定為&#8203;`0`&#8203;，就會使用&#8203;**簡單**&#8203;演算法，否則，會使用&#8203;**複雜**&#8203;演算法──也就是設定成先把所有遊戲的遊玩時數掛到指定的小時數，然後再掛取卡片。

---

### **哪種演算法更適合您並沒有明確的答案**&#8203;。

這也是您不用選擇掛卡演算法的原因之一，而是告訴ASF您的帳號是否有掉落限制。 若帳號不受限制，&#8203;**簡單**&#8203;演算法在該帳號上的效果會&#8203;**更好**&#8203;，因為我們不需要浪費時間把遊戲掛至&#8203;`X`&#8203;小時──在掛多個遊戲時，掉卡率會接近0%。 反之，若您的帳號受到掉卡限制，&#8203;**複雜**&#8203;演算法會更適合您，因為若遊玩時數未達到&#8203;`HoursUntilCardDrops`&#8203;小時，那麼單獨掛卡並無意義──所以我們將先掛&#8203;**遊玩時數**&#8203;，&#8203;**然後**&#8203;才掛單一遊戲。

不要聽信其他人的說法盲目設定&#8203;`HoursUntilCardDrops`&#8203;，您需要進行測試、比較結果，並依據您獲得的資料，來決定何值適合您。 鑒於您正閱讀本Wiki頁面，您應該很需要提高ASF的效率。只要您為此付出些許努力，您就能確保ASF以最大可能的效率為您的帳號運作。 若存在適用於所有人的解決方案，您就不需要選擇了──ASF會自動決定。

---

### 確認您的帳號是否為受限的最好的方法是什麼？

確認您有一些&#8203;**無遊玩時數**&#8203;的遊戲以供掛卡，最好有5款或以上，並以&#8203;`HoursUntilCardDrops`&#8203;為&#8203;`0`&#8203;的值來執行ASF。 掛卡期間不遊玩任何遊戲是個好主意，能獲得更加準確的結果（最好是在晚上執行ASF）。 讓ASF掛卡這5款遊戲，然後再查看紀錄來獲得結果。

ASF清楚說明了給定遊戲的交換卡片會於何時掉落。 您需要關注ASF最早掉落的交換卡片。 舉例來說，若您的帳號不受限制，那麼您在開始掛卡後的30分鐘內會掉第一張卡。 若您發現&#8203;**至少一款**&#8203;遊戲在開始的30分鐘內掉卡，就代表您的帳號&#8203;**不受限制**&#8203;，且&#8203;`HoursUntilCardDrops`&#8203;應使用&#8203;`0`&#8203;。

反之，若您發現&#8203;**每款**&#8203;遊戲均需至少&#8203;`X`&#8203;小時才能掉落第一張卡，那就代表您應該將&#8203;`HoursUntilCardDrops`&#8203;設定成該小時數。 大多數（但不是全部）的受限使用者需要至少&#8203;`3`&#8203;小時的遊玩時數才會開始掉卡，而這也是&#8203;`HoursUntilCardDrops`&#8203;設定的預設值。

記住，遊戲可能有不同的掉落速率，這就是為什麼您應以&#8203;**至少**&#8203;3款，最好5款或以上的遊戲來測試您的猜測，來保證您遇到的結果並非巧合。 A card drop of one game in less than an hour is a confirmation that your account **is not** restricted and can use `HoursUntilCardDrops` of `0`, but for confirming that your account **is** restricted, you need at least several games that are not dropping cards until you hit a fixed mark.

It's important to note that in the past `HoursUntilCardDrops` was only `0` or `2`, and this is why ASF had a single `CardDropsRestricted` property that allowed to switch between those two values. With recent changes we noticed that not only majority of users require `3` hours in place of previous `2` now, but also that `HoursUntilCardDrops` is now dynamic and can hit any value on per-account basis.

In the end, of course, decision is up to you.

And to make it even worse - I experienced cases when people switched from restricted to unrestricted state and vice versa - either because of Steam bug (oh yeah, we have many of those), or because of some logic adjustments by Valve. So even if you confirmed that your account is restricted (or not), do not believe that it'll stay like that - in order to switch from unrestricted to restricted it's enough to ask for a refund. If you feel like previously set value is no longer appropriate, you can always do a re-test and update it accordingly.

---

By default, ASF assumes that `HoursUntilCardDrops` is `3`, as the negative effect of setting this to `3` when it should be less is smaller than done the other way. This is because of the fact that in the worst possible case we'll waste `3` hours of farming per `32` games, compared to wasting `3` hours of farming per every single game if `HoursUntilCardDrops` was set to `0` by default. However, you should still tune this variable to match your account for maximum efficiency, as this is only a blind guess based on potential drawbacks and majority of users (so we're trying to choose "lesser evil" by default).

At the moment two above algorithms are enough for all currently possible account scenarios, in order to farm as effectively as possible, therefore it's not planned to add any other ones.

It's nice to note that ASF also includes manual farming mode that can be activated by `play` command. You can read more about it in **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

---

## Steam 故障

Cards drop algorithm does not always work the way it should, and it's entirely possible for various Steam glitches to happen, such as cards being dropped on restricted accounts, cards being dropped on closing/switching the game, cards not dropping at all when game is being played, and likewise.

This section is mainly for people that are wondering why ASF doesn't do **X**, such as rapidly switching games to farm cards faster.

What is a **Steam glitch** - a specific action triggering **undefined** behaviour, which is **not intended, undocumented, and considered as a logic flaw**. It's **unreliable by definition**, which means that it can't be reproduced reliably with clean testing environment, and therefore, coded without resorting to hacks that are supposed to guess when glitch is happening and how to fight with it / abuse it. Typically it's temporary until developers fix the logic flaw, although some misc glitches can go unnoticed for a very long period of time.

A good example of what is considered as a **Steam glitch** is not that uncommon situation of dropping a card when game is being closed, which can be abused to some degree with idle master's game skip function.

- **Undefined behaviour** - you can't say if there will be 0 or 1 cards being dropped when you trigger the glitch.
- **Not intended** - based on past experience and behaviour of Steam network that doesn't result in same behaviour when sending a single request.
- **Undocumented** - it's clearly documented on Steam website how cards are being obtained, and **in every single place** it's clearly stated that it's obtained through **playing**, NOT closing games, getting achievements, games switching or launching 32 games concurrently.
- **Considered as a logic flaw** - closing game(s) or switching them should have no outcome on cards being dropped which are clearly stated to be obtained through **gaining playtime**.
- **Unreliable by definition, can't be reproduced reliably** - it doesn't work for everybody, and even if it did work for you once, it could no longer work for the second time.

Now once we realized what Steam glitch is, and the fact that cards being dropped when game gets closed **is** one, we can move on to the second point - **ASF is not abusing Steam network in any way by definition, and it's doing its best to comply with Steam ToS, its protocols and what is generally accepted**. Spamming Steam network with constant game opening/closing requests can be considered a **[DoS attack](https://en.wikipedia.org/wiki/Denial-of-service_attack)** and **directly violates [Steam Online Conduct](https://store.steampowered.com/online_conduct/?l=english)**.

> As a Steam subscriber you agree to abide by the following conduct rules.
> 
> 您將不會：
> 
> Institute attacks upon a Steam server or otherwise disrupt Steam.

It doesn't matter whether you're able to trigger Steam glitch with other programs (such as IM), and it also doesn't matter if you agree with us and consider such behaviour as DoS attack, or not - it's up to Valve to judge this, but if we consider it as exploiting/abusing non-intended behaviour through excessive Steam network requests, then you can be pretty sure that Valve will have similar view on this.

ASF is **never** going to take advantage of Steam exploits, abuses, hacks or any other activity that we see as **illegal or unwanted** according to Steam ToS, Steam Online Conduct or any other trusted source that could indicate that ASF activity is unwanted by Steam network, as stated in **[contributing](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/.github/CONTRIBUTING.md)** section.

If you want at all cost to risk your Steam account for farming a few cent cards faster than usual, then sadly ASF will never offer something like this in automatic mode, although you still have `play` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** that can be used as a tool for doing whatever you want in terms of Steam network interaction. We do not recommend taking advantage of Steam glitches and exploiting them for your own gain - not only with ASF, but with any other tool as well. In the end however, it's your account, and your choice what you want to do with it - just keep in mind that we warned you.