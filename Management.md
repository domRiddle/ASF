# Management

This section covers subjects related to managing the ASF process in optimal way. While not strictly mandatory for usage, it includes bunch of tips, tricks and good practices that we'd like to share, especially for system administrators, people packaging the ASF for usage in third-party repositories, as well as advanced users and alike.

---

## `systemd` service for Linux

todo

---

## Never run ASF as administrator!

ASF includes its own validation whether the process is being run as administrator (`root`) or not. Running as root is **not** required for any kind of operation done by the ASF process assuming properly configured environment it's operating in, and therefore should be regarded as a bad practice. This means that on Windows, ASF should never be executed with "run as administrator" setting, and on Unix ASF should have a dedicated user account for itself, or re-use your own in case of a desktop system.

For further elaboration on *why* we discourage running ASF as root, refer to **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** and other resources. If you're still not convinced, ask yourself what would happen to your machine if ASF process executed `rm -rf --no-preserve-root /` command right after its launch.

### I run as `root` because ASF can't write to files

This means that you have wrongly configured permissions of the files ASF is trying to access. You should login as `root` account (either with `su` or `sudo -i`) and then **correct** the permissions by issuing `chown -hR 1000:1000 /path/to/ASF` command, substituting `1000:1000` with `UID` and `GID` that you'll run ASF under, and `/path/to/ASF` accordingly. If by any chance you're using custom `--path` telling ASF user to use the different directory, you should execute the same command again for that path as well.

After doing that, you should no longer get any kind of issue related to ASF not being able to write over its own files, as you've just changed the owner of everything ASF is interested in to the user the ASF process will actually run under.

### I run as `root` because I don't know how to do it otherwise

```sh
su # or sudo -i
adduser asf
chown -hR asf:asf /path/to/ASF
su asf -c /path/to/ASF/ArchiSteamFarm # or sudo -u asf /path/to/ASF/ArchiSteamFarm
```

That would be doing it manually, it's much easier to use our `systemd` service explained above.

---

## Multiple instances

ASF is compatible with running multiple instances of the process on the same machine. The instances can be completely standalone or derived from the same binary location (in which case, you want to run them with different `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**).

When running multiple instances from the same binary, keep in mind that you should typically disable auto-updates in all of their configs, as there is no synchronization between them in regards to auto-updates. If you'd like to keep having auto-updates enabled, we recommend standalone instances, but you can still make updates work, as long as you can ensure that all other ASF instances are closed.

ASF will do its best to maintain a minimum amount of OS-wide, cross-process communication with other ASF instances. This includes ASF checking its configuration directory against other instances, as well as sharing core process-wide limiters configured with `*LimiterDelay` **[global config properties](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, ensuring that running multiple ASF instances will not cause a possibility to run into a rate-limiting issue. In regards to technical aspects, all platforms use our dedicated mechanism of custom ASF file-based locks created in temporary directory, which is `C:\Users\<YourUser>\AppData\Local\Temp\ASF` on Windows, and `/tmp/ASF` on Unix.

It's not required for running ASF instances to share the same `*LimiterDelay` properties, they can use different values, as each ASF will add its own configured delay to the release time after acquiring the lock. If the configured `*LimiterDelay` is set to `0`, ASF instance will entirely skip waiting for the lock of given resource that is shared with other instances (that could potentially still maintain a shared lock with each other). When set to any other value, ASF will properly synchronize with other ASF instances and wait for its turn, then release the lock after configured delay, allowing other instances to continue.

ASF takes into account `WebProxy` setting when deciding about shared scope, which means that two ASF instances using different `WebProxy` configurations will not share their limiters with each other. This is implemented in order to allow `WebProxy` setups to operate without excessive delays, as expected from different network interfaces. This should be good enough for majority of use cases, however, if you have a specific custom setup in which you're e.g. routing requests yourself in a different way, you can specify network group yourself through `--network-group` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**, which will allow you to declare ASF group that will be synchronized with this instance. Keep in mind that custom network groups are used exclusively, which means that ASF will no longer use `WebProxy` for determining the right group, as you're in charge of grouping in this case.