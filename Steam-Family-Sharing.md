# Steam Family Sharing

ASF supports Steam Family Sharing since version 2.1.5.5+. In order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](http://store.steampowered.com/promotion/familysharing)**, which is available on Steam store.

---

## ASF

Support for that feature in ASF is transparent, which means that it doesn't introduce any new bot/process config properties - it just works out of the box as an extra.

First important logic bit is family sharing recognition - ASF is aware of library being locked by family sharing users, therefore it won't "kick" them out of playing session due to launching a game. ASF will act exactly the same as with primary account holding the lock, therefore if that lock is being held either by your steam client, or by one of your family sharing users, ASF will not attempt to farm, instead, it will wait for the lock to be released.

Second logic bit is extra support - After logging in, ASF will access your **[games sharing settings](https://store.steampowered.com/account/managedevices)**, from which it'll extract up to 5 `steamID`s allowed to use your library. Those users are permitted to use `!pause~` command on bot account that is sharing games with them, which allows them to pause automatic cards farming module in order to launch a game that can be shared. After they're done playing, automatic cards farming module will automatically issue `!resume` command in order to keep farming.

Connecting both bits described above allows your friends to `!pause~` your cards farming process, start a game, play as long as they wish, then after they're done playing, cards farming process is automatically resumed by ASF. Of course, issuing `!pause~` is not needed if ASF is currently not farming anything actively, because your friends can launch the game right away, and bit #1 ensures that they won't be kicked out of the session.

---

## Limitations

Steam network loves to mislead ASF by broadcasting false status updates, which might lead to ASF believing it's fine to resume process, and in result kick your friend too soon. In order to fight with that issue, it's recommended for you to have your friend on your Steam friendlist - this way ASF in addition to steam network events, can check Steam status of your friend, and guess from playing status if he's indeed done playing yet, or not. This is not mandatory, but because Steam sometimes talks trash, it's recommended, especially if you notice such issues.

---

ASF support for Steam Family Sharing is still in "beta" status - it should work properly, but if you notice something strange, please don't hesitate to let us know, preferably through Steam group.