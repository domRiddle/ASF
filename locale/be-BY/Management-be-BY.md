# Management

This section covers subjects related to managing the ASF process in optimal way. While not strictly mandatory for usage, it includes bunch of tips, tricks and good practices that we'd like to share, especially for system administrators, people packaging the ASF for usage in third-party repositories, as well as advanced users and alike.

---

## `systemd` service for Linux

In `generic` and `linux` variants, ASF comes with `ArchiSteamFarm@.service` file, which is a configuration file of the service for **[`systemd`](https://systemd.io)**. If you'd like to run ASF as a service, for example in order to launch it automatically after startup of your machine, then a proper `systemd` service is arguably the best way to do it, therefore we highly recommend it instead of managing it on your own through `nohup`, `screen` or alike.

Firstly, create the account for the user you want to run ASF under, assuming it doesn't exist yet. We'll use `asf` user for this example, if you decided to use a different one, you'll need to substitute `asf` user in all of our examples below with your selected one. Our service does not allow you to run ASF as `root`, since it's considered a **[bad practice](#never-run-asf-as-administrator)**.

```sh
su # or sudo -i
useradd -m asf
```

Next, unpack ASF to `/home/asf/ArchiSteamFarm` folder. The folder structure is important for our service unit, it should be `ArchiSteamFarm` folder in your `$HOME`, so `/home/<user>`. If you did everything correctly, there will be `/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service` file existing. If you're using `linux` variant and didn't unpack the file on Linux, but for example used file transfer from Windows, then you'll also need to `chmod +x /home/asf/ArchiSteamFarm/ArchiSteamFarm`.

We'll do all below actions as `root`, so get to its shell with `su` or `sudo -i`.

Firstly it's a good idea to ensure that our folder still belongs to our `asf` user, `chown -hR asf:asf /home/asf/ArchiSteamFarm` executed once will do it. The permissions could be wrong e.g. if you've downloaded and/or unpacked the zip file as `root`.

Next, `cd /etc/systemd/system` and execute `ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service .`, this will create a symbolic link to our service declaration and register it in `systemd`. Symbolic link will allow ASF to update your `systemd` unit automatically as part of ASF update - depending on your situation, you may want to use that approach, or simply `cp` the file and manage it yourself however you'd like.

Afterwards, ensure that `systemd` recognizes our service:

```
systemctl status ArchiSteamFarm@asf

○ ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
```

Pay special attention to the user we declare after `@`, it's `asf` in our case. Our systemd service unit expects from you to declare the user, as it influences the exact place of the binary `/home/<user>/ArchiSteamFarm`, as well as the actual user systemd will spawn the process as.

If systemd returned output similar to above, everything is in order, and we're almost done. Now all that is left is actually starting our service as our chosen user: `systemctl start ArchiSteamFarm@asf`. Wait a second or two, and you can check the status again:

```
systemctl status ArchiSteamFarm@asf

● ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: active (running) since (...)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
   Main PID: (...)
(...)
```

If `systemd` states `active (running)`, it means everything went well, and you can verify that ASF process should be up and running, for example with `journalctl -r`, as ASF by default also writes to its console output, which is recorded by `systemd`. If you're satisfied with the setup you have right now, you can tell `systemd` to automatically start your service during boot, by executing `systemctl enable ArchiSteamFarm@asf` command. That's all.

If by any chance you'd like to stop the process, simply execute `systemctl stop ArchiSteamFarm@asf`. Likewise, if you want to disable ASF from being started automatically on boot, `systemctl disable ArchiSteamFarm@asf` will do that for you, it's very simple.

Please note that, as there is no standard input enabled for our `systemd` service, you won't be able to input your details through the console in usual way. Running through `systemd` is equivalent to specifying **[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** setting and comes with all its implications. Fortunately for you, it's very easy to manage your ASF through **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, which we recommend in case you need to supply additional details during login or otherwise manage your ASF process further.

### Environment variables

It's possible to supply additional environment variables to our `systemd` service, which you'll be interested in doing in case you want to for example use a custom `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)**, therefore specifying `ASF_CRYPTKEY` environment variable.

In order to provide custom environment variables, create `/etc/asf` folder (in case it doesn't exist), `mkdir -p /etc/asf`. We recommend to `chmod 700 /etc/asf` to ensure that only `root` user has access to read those files, because they might contain sensitive properties such as `ASF_CRYPTKEY`. Afterwards, write to a `/etc/asf/<user>` file, where `<user>` is the user you're running the service under (`asf` in our example above, so `/etc/asf/asf`).

The file should contain all environment variables that you'd like to provide to the process:

```sh
# Declare only those that you actually need
ASF_CRYPTKEY="my_super_important_secret_cryptkey"
ASF_NETWORK_GROUP="my_network_group"

# And any other ones you're interested in
```

---

## Never run ASF as administrator!

ASF includes its own validation whether the process is being run as administrator (`root`) or not. Running as root is **not** required for any kind of operation done by the ASF process, assuming properly configured environment it's operating in, and therefore should be regarded as a **bad practice**. This means that on Windows, ASF should never be executed with "run as administrator" setting, and on Unix ASF should have a dedicated user account for itself, or re-use your own in case of a desktop system.

For further elaboration on *why* we discourage running ASF as root, refer to **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** and other resources. If you're still not convinced, ask yourself what would happen to your machine if ASF process executed `rm -rf /*` command right after its launch.

### I run as `root` because ASF can't write to its files

This means that you have wrongly configured permissions of the files ASF is trying to access. You should login as `root` account (either with `su` or `sudo -i`) and then **correct** the permissions by issuing `chown -hR asf:asf /path/to/ASF` command, substituting `asf:asf` with the user that you'll run ASF under, and `/path/to/ASF` accordingly. If by any chance you're using custom `--path` telling ASF user to use the different directory, you should execute the same command again for that path as well.

After doing that, you should no longer get any kind of issue related to ASF not being able to write over its own files, as you've just changed the owner of everything ASF is interested in to the user the ASF process will actually run under.

### I run as `root` because I don't know how to do it otherwise

```sh
su # or sudo -i
useradd -m asf
chown -hR asf:asf /path/to/ASF
su asf -c /path/to/ASF/ArchiSteamFarm # or sudo -u asf /path/to/ASF/ArchiSteamFarm
```

That would be doing it manually, it's much easier to use our **[`systemd` service](#systemd-service-for-linux)** explained above.

### I know better and I still want to run as `root`

As of V5.2.0.10, ASF no longer stops you from doing so, only displays a warning with a short notice. Just don't be shocked if one day due to a bug in the program it'll blow up your whole OS with complete data loss - you've been warned.

---

## Multiple instances

ASF is compatible with running multiple instances of the process on the same machine. The instances can be completely standalone or derived from the same binary location (in which case, you want to run them with different `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**).

When running multiple instances from the same binary, keep in mind that you should typically disable auto-updates in all of their configs, as there is no synchronization between them in regards to auto-updates. If you'd like to keep having auto-updates enabled, we recommend standalone instances, but you can still make updates work, as long as you can ensure that all other ASF instances are closed.

ASF will do its best to maintain a minimum amount of OS-wide, cross-process communication with other ASF instances. This includes ASF checking its configuration directory against other instances, as well as sharing core process-wide limiters configured with `*LimiterDelay` **[global config properties](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, ensuring that running multiple ASF instances will not cause a possibility to run into a rate-limiting issue. In regards to technical aspects, all platforms use our dedicated mechanism of custom ASF file-based locks created in temporary directory, which is `C:\Users\<YourUser>\AppData\Local\Temp\ASF` on Windows, and `/tmp/ASF` on Unix.

It's not required for running ASF instances to share the same `*LimiterDelay` properties, they can use different values, as each ASF will add its own configured delay to the release time after acquiring the lock. If the configured `*LimiterDelay` is set to `0`, ASF instance will entirely skip waiting for the lock of given resource that is shared with other instances (that could potentially still maintain a shared lock with each other). When set to any other value, ASF will properly synchronize with other ASF instances and wait for its turn, then release the lock after configured delay, allowing other instances to continue.

ASF takes into account `WebProxy` setting when deciding about shared scope, which means that two ASF instances using different `WebProxy` configurations will not share their limiters with each other. This is implemented in order to allow `WebProxy` setups to operate without excessive delays, as expected from different network interfaces. This should be good enough for majority of use cases, however, if you have a specific custom setup in which you're e.g. routing requests yourself in a different way, you can specify network group yourself through `--network-group` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**, which will allow you to declare ASF group that will be synchronized with this instance. Keep in mind that custom network groups are used exclusively, which means that ASF will no longer use `WebProxy` for determining the right group, as you're in charge of grouping in this case.

If you'd like to utilize our **[`systemd` service](#systemd-service-for-linux)** explained above for multiple ASF instances, it's very simple, just use another user for our `ArchiSteamFarm@` service declaration and follow with the rest of the steps. This will be equivalent of running multiple ASF instances with distinct binaries, so they can also auto-update and operate independently of each other.