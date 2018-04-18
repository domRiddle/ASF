# Deprecation

Starting with ASF V3.1.2.2, we'll be following consistent deprecation policy in order to make both development as well as usage far more consistent.

---

## What is deprecation?

Deprecation is the process of doing smaller or bigger breaking changes that render previously used options, arguments, functionalities or usage cases obsolete. Deprecation usually means that given thing was simply rewritten into another (similar) form, and you should ensure in timely manner that you'll make appropriate switch to it.

ASF changes rapidly and always strikes for becoming better. This sadly means that we might change or move some existing functionality into another segment of the program in order for it to benefit from new features, compatibility or stability. We're always trying to provide reasonable replacement that fits expected usage of previously-available functionality, which is why deprecation is mostly harmless and requires small fixes to previous usage.

---

## Deprecation stages

ASF will follow 3 stages of deprecation, making transition much easier and less troublesome.

### Stage 1

Stage 1 happens immediately once given feature is deprecated, with immediate availability of another solution (or none if there are no plans of re-introducing it).

During this stage, ASF will print appropriate warning when deprecated function is being used. As long as it's possible, ASF will try to mimic the old behaviour and keep compatible with previous behaviour. ASF will keep being in stage 1 regarding that functionality at least until next stable version. This is the moment when, hopefully without breaking compatibility, you can make appropriate switch in all your tools and patterns to satisfy new behaviour.

### Stage 2

Stage 2 is scheduled for the very next stable version, after above stage takes place. This will change previous deprecation warning into **an error**, that will cause given functionality to stop working. ASF will still acknowledge existance of deprecated feature, but will no longer try to mimic old behaviour or logic. During this stage, transition is **required** in order to make use of now-deprecated feature.

### Stage 3

Stage 3 is final one and like second one, is scheduled for the very next stable release after above. This stage introduces complete removal of deprecated feature existance, which means that ASF will not even acknowledge that you're attempting to use deprecated feature, since it simply doesn't exist in current code. ASF will no longer print any warning or error, since it's simply unaware of what you're attempting to do.

---

## Example

We splitted pre-V3.1.2.2 `--service` **[command-line argument](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments)** into 3 independent options: `--no-restart`, `--process-required` and `--system-required`.

### Stage 1

Stage 1 happened in version V3.1.2.2 where we added appropriate warning to usage of `--service`. Now-obsolete `--service` argument is automatically converted to new `--no-restart --process-required --system-required` settings, effectively acting exactly the same as old `--service` switch.

### Stage 2

Stage 2 is yet to happen and will make ASF exit with non-zero error code when `--service` argument is passed.

### Stage 3

Stage 3 is yet to happen and will include complete removal of `--service` argument, making it non-existant.