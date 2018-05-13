# Deprecation

Starting with ASF V3.1.2.2, we'll be following consistent deprecation policy in order to make both development as well as usage far more consistent.

---

## What is deprecation?

Deprecation is the process of doing smaller or bigger breaking changes that render previously used options, arguments, functionalities or usage cases obsolete. Deprecation usually means that given thing was simply rewritten into another (similar) form, and you should ensure in timely manner that you'll make appropriate switch to it. In this case, it's simply moving given functionality to more appropriate place.

ASF changes rapidly and always strikes for becoming better. This sadly means that we might change or move some existing functionality into another segment of the program in order for it to benefit from new features, compatibility or stability. Thanks to that we don't need to stick with obsolete or simply painfully wrong development decisions that we made years ago. We're always trying to provide reasonable replacement that fits expected usage of previously-available functionality, which is why deprecation is mostly harmless and requires small fixes to previous usage.

---

## Deprecation stages

ASF will follow 3 stages of deprecation, making transition much easier and less troublesome.

### Stage 1

Stage 1 happens once given feature becomes deprecated, with immediate availability of another solution (or none if there are no plans of re-introducing it).

During this stage, ASF will print appropriate warning when deprecated function is being used. As long as it's possible, ASF will try to mimic the old behaviour and keep being compatible with it. ASF will keep being in stage 1 regarding that functionality at least until next stable version. This is the moment when, hopefully without breaking compatibility, you can make appropriate switch in all your tools and patterns to satisfy new behaviour. You can confirm whether you did all appropriate changes by no longer seeing the deprecation warning.

### Stage 2

Stage 2 is scheduled for the very next stable version, after above stage takes place. This will change previous deprecation warning into **an error**, that will cause given functionality to stop working. ASF will still acknowledge existance of deprecated feature, but will no longer try to mimic old behaviour or logic. During this stage, transition is **required** in order to make use of now-deprecated feature. Since ASF can't guarantee mimicing old behaviour (like done during stage 1) anymore, this will effectively cause module directly depending on deprecated feature to stop working with an error.

### Stage 3

Stage 3 is final one and like second one, is scheduled for the very next stable release after above. This stage introduces complete removal of deprecated feature existance, which means that ASF will not even acknowledge that you're attempting to use deprecated feature, since it simply doesn't exist in current code. ASF will no longer print any warning or error, since it's simply unaware of what you're attempting to do.

---

## Summary

You have more or less **full 2 months** in order to make appropriate switch, which should be more than enough even if you're a casual ASF user. After that period, ASF no longer guarantees that old settings will have any effect (stage 3), effectively making certain features to stop functioning altogether without you noticing. If you're launching ASF after more than 2 months of inactivity, it's recommended for you to **[start from scratch](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** again, or read all the changelogs you've missed and manually adapt your usage to current one.

---

## Example

We moved pre-V3.1.2.2 `--server` **[command-line argument](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments)** into `IPC` **[global configuration property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)**.

### Stage 1

Stage 1 happened in version V3.1.2.2 where we added appropriate warning to usage of `--server`. Now-obsolete `--server` argument was automatically mapped into `IPC: true` global config property, effectively acting exactly the same as old `--server` switch for time being. This allowed everybody to do appropriate switch before ASF stops accepting old argument.

### Stage 2

Stage 2 happened in version V3.1.3.0, right after V3.1.2.9 stable with stage 1 explained above. Stage 2 caused ASF to display an error and exit with non-zero error code when `--server` argument is passed, effectively making program impossible to run when `--server` argument is passed.

### Stage 3

Stage 3 is yet to happen and will include complete removal of `--server` argument, making it non-existant.