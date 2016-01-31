# Performance

***

The primary objective of ASF is to farm as effectively as possible, based on two types of data it can operate on - small set of user-provided data that is impossible for ASF to guess/check on it's own, and larger set of data which can be automatically checked by ASF.

In automatic mode, ASF does not allow you to choose the games that should be farmed, neither allows you to change cards farming algorithm. ASF knows better than you what it should do and what decisions it should make in order to farm as fast as possible. Your objective is to set config properties properly, as ASF can't guess them on it's own, everything else is covered.

***

ASF currently includes two farming algorithms:

**Simple** algorithm works best for accounts which are not restricted by 2 hours cards drop. This is primary and default algorithm used by ASF. Bot finds games to farm, and farms them one-by-one until all cards are dropped.

**Complex** is new algorithm that has been implemented to help restricted accounts to maximize their profits as well. ASF will firstly use standard **Simple** algorithm on all games that passed 2 hours of playtime, then, if no games with >= 2 hours are left, it will farm all games with < 2 hours left simultaneously, until any of them hits 2 hours mark, then ASF will continue the loop from beginning (use **Simple** on that game, return to simultaneous on < 2 etc.)

Currently, ASF chooses cards farming algorithm based purely on ```CardDropsRestricted``` config property (which is  set by **you**). If ```CardDropsRestricted``` is set to ```true```, **Complex** algorithm will be used, if not, **Simple** algorithm will be used instead.

***

**There is no obvious answer which algorithm is better**.

This is one of the reasons why you do not choose cards farming algorithm, instead, you tell ASF if account has restricted drops or not. If account has non-restricted drops, which is default setting, **Simple** algorithm will **work better** on that account, as we won't be wasting time on bringing all games to 2 hours. On the other hand, if your account has card drops restricted, **Complex** algorithm will be better for you, as there's no point in farming solo if game didn't reach 2 hours yet.

By default, ASF assumes positive scenario - our account doesn't have restricted card drops, so ```CardDropsRestricted``` is ```false``` by default.

At the moment two above algorithms are enough for all currently possible account scenarios, in order to farm as effectively as possible.