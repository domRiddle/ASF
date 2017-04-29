# Mono

This page covers Mono-related issues, instructions, questions and provides general help, that might be useful if you're running ASF under Mono. If you're running ASF natively on Windows, most likely you won't find anything useful here.

---

# Compatibility

ASF is officially compatible (and supported) only with latest ```stable``` and ```nightly``` versions of Mono. The program is supposed to compile, as well as run correctly with both self-compiled and **[released](https://github.com/JustArchi/ArchiSteamFarm/releases)** binary on those two version variants of Mono. The support also includes all necessary hacks/workarounds in the code that ensure stability and compatibility of ASF on Mono making it possible to run.

Every version that is older than latest ```stable``` is **not** supported and may, or may not, work correctly. ASF cannot guarantee 100% compatibility and stability in this case, but unofficially it's known that ASF usually tends to work fine even on older versions, as long as runtime doesn't contain major bugs. That's why it's not technically "required" to always run ASF with latest stable/nightly version, but with every major revision behind you risk running into issues - issues that might not be visible right away. Therefore, make sure you're running latest Mono if you spot some nasty thing going on before reporting an issue - using Mono at least in version declared as **required** during launching ASF is the absolute minimum, but we recommend latest stable regardless.

---

# How to install latest Mono?

On Debian-like Linux distributions (including Ubuntu-like), add to your ```/etc/apt/sources.list``` following line:

```
deb http://download.mono-project.com/repo/debian wheezy main # Yes, wheezy also covers other versions and distributions
```

Then execute (as root, or with sudo):
```
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
apt-get update
apt-get install ca-certificates-mono mono-complete
```

If you get an issue during installing ```mono-complete```, similar to below:

```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 mono-complete(...)
```

Then additionally, add this to your ```sources.list```:

```
deb http://download.mono-project.com/repo/debian wheezy-libjpeg62-compat main
```

And execute ```apt-get update``` and ```apt-get install``` like stated above once again.

---

After successful installation, you should notice that ```mono --version``` command returns the same version that is currently marked as latest stable on **[official Mono page](http://www.mono-project.com/download/)**.

Keep in mind that instructions above are also available on **[official Mono installation page for debian-like distributions](http://www.mono-project.com/docs/getting-started/install/linux/#debian-ubuntu-and-derivatives)**. If you're not using Debian-like Linux distribution, and can't follow above ```apt-get``` tips, either start using one, or head over to **[official Mono installation page for other distributions](http://www.mono-project.com/download/#download-lin)** to get instructions that match your Linux distribution.

If you're using OS X, then you can find instructions for your Mac **[here](http://www.mono-project.com/docs/getting-started/install/mac/)**.

Officially, ASF requires only latest .NET framework (currently 4.6.1, which is supported since Mono 4.0+) and it should be available in all current Linux distributions. However, only latest Mono guarantees bugless experience. In case of uncovered bugs without any available solution, or bugs with solutions that don't help, the only suggestion is to use latest Mono.

**All hard-to-explain issues without obvious cause are automatically ignored, if your Mono version is not latest stable one.**

It's your responsiblity to ensure that ASF is run in bugless environment, and not mine to workaround all possible Mono issues, in all possible versions, on all possible variants. Latest stable ASF is always tested on latest nightly and stable Mono version, and always includes all crucial code changes that are required for working properly on Mono (if needed).

---

# Usage

Using Mono is pretty straightforward, the most basic usage is navigating to the directory where ASF is located (e.g. with ```cd``` command), then executing:

```
mono ASF.exe
```

The above command can be also used for creating various shortcuts or aliases, according to your needs and possibilities.

In the same way you can execute other ```.exe``` files provided by ASF. Keep in mind that some of them might require GUI to be available (e.g. ```ASF-ConfigGenerator.exe```).

Instead of calling mono manually each time, you might also want to use a script. This is especially useful for people who want to run ASF on their servers (or virtual machines).

Example script provided below will ensure that ASF is always running, unless it crashes with unhandled exception. **Notice:** you should ensure that ```AutoRestart``` is ```false``` when running this script, as ASF might restart itself otherwise (which will result in 2 processes running):

```
#!/bin/bash
set -eu # Immediately abort script in case of error

cd "$(dirname "$(readlink -f "$0")")" # Navigate to script directory

while [[ -f ASF.exe ]]; do # While ASF.exe binary exists
    if ! mono ASF.exe; then # Start ASF, and if it exited with error
        break # Abort the loop
    fi # Otherwise
    sleep 1 # Wait a second
done # And Start ASF.exe again
```

You might want to save above script for example as ```asf.sh```, chmod it ```755```, and execute it for example through ```screen```, which would result in command ```screen -dmS ASF /path/to/asf.sh``` or similar. This way you'll always have ASF running in your ```screen``` terminal, and you can easily check ASF output via ```screen -r```, or minimize it again with ```CTRL + A + D```. Keep in mind that those instructions are based on Linux distributions with ```bash``` shell available, instructions for other distributions or OS X might be different.

Please notice that we wrap mono command in ```if``` instruction. This is important as ASF should be restarted only if it decided to exit by itself (error code 0) and not when it exited due to crash or exception (non-zero exit code). Of course nothing is stopping you from restarting ASF when it exited due to error, but you should add your own logic for keeping ```log.txt``` from the crash, so you're able to report the problem. Default behaviour assumes that you do not want to restart ASF if crash happened, and restart it only if ASF decided to exit by itself, e.g. due to update or ```!exit``` command.

Also keep in mind that ASF automatically exits (with zero error code) when all bots are stopped. Therefore you should ensure that you have at least one ```Enabled = true``` and ```ShutdownOnFarmingFinished = false``` bot. Otherwise you might run into the situation such as starting ASF in infinite loop even if there's nothing to do.

If you want to run ASF in non-interactive way such as on start of your server without your attention, I strongly suggest to switch ```Headless``` property to ```true```, so ASF will be aware of the fact that it can't expect any response from you.

Everything above is specific to your configuration. This is only example of usage, feel free to further modify the script however you wish. If you're looking for something more advanced, check out ASF **[run.sh](https://github.com/JustArchi/ArchiSteamFarm/blob/master/run.sh)**. It's a bit more advanced way of running ASF, allowing you to pass extra **[command-line arguments](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-Line-Arguments)** as well as other options.

If you just want to launch ASF in background, without dealing with a script, it's probably best to start it via ```screen -dmS ASF "mono /path/to/ASF.exe"``` instead, although in this scenario you should still use ```AutoRestart``` of ```false```. This is because restarted ASF will detach from your screen session on self-restart. Setting ```Headless``` to ```true``` is not mandatory in this case, as ```screen``` has capabilities of standard input, but if you don't plan to return back to it, it's wise to set it to ```true``` as well.

**Notice:** If you're using ASF on a machine with low amount of memory (512 MB or less), you might be interested in tuning the Mono to your needs. Visiting **[Low-memory setup](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)** might help.

---

# Nightly

Apart from ```stable``` channel (currently: ```wheezy```), Mono's repo also offers ```nightly``` channel, with more recent, not yet stable Mono versions. This is the Mono version that is officially tested before releasing stable ASF, as it's a Mono version that is most recent at the time of release.

Switching to nightly is not recommended in general, as Mono included there may not be always stable, but it may be needed to update if you for some reason require e.g. bugfix which was not yet released on stable channel.

To switch to nightly versions, modify the line that you added to ```/etc/apt/sources.list```:

```
deb http://download.mono-project.com/repo/debian wheezy main
```

To:

```
deb http://download.mono-project.com/repo/debian nightly main
```

Then issue ```apt-get update && apt-get install mono-complete``` to update.

Remember that nightly versions are unstable and might not work, therefore you should avoid them unless you want to live on the bleeding edge.

**TIP:** There are also other versions available: ```nightly``` > ```alpha``` > ```beta``` > ```stable```

```nightly``` is usually very unstable and has many issues, but includes also most up-to-date bleeding edge codebase, while ```stable``` is carefully tested in order to work on as many setups as possible.

```alpha``` and ```beta``` are a balance between ```nightly``` and ```stable```. Usually ```alpha``` versions are quite stable and should work on **most** machines, but it's not always the case. If you're not feeling comfortable with broken packages and Linux, you should avoid using any branch other than ```stable```.

---

# Issues and solutions

Most (if not all) of the issues are directly caused by outdated Mono. Mono shipped with your Linux distribution is almost never the latest released Mono version, and the newer Mono you have, the bigger chance that everything will run out of the box.

---

**Problem:** I'm unable to edit `SteamUserPermissions` property in ASF-ConfigGenerator when launching the program with Mono.

**Solution:** This is known Mono problem with no available workaround right now. I suggest to edit `SteamUserPermissions` property in JSON file manually, valid JSON structure for `Dictionary` type is specified in **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#types)**.

---

**Problem:**
```
System.Net.WebException: Error: TrustFailure (The authentication or decryption has failed.) ---> System.IO.IOException: The authentication or decryption has failed. ---> System.IO.IOException: The authentication or decryption has failed. ---> Mono.Security.Protocol.Tls.TlsException: Invalid certificate received from server.
```

**Solution:**
This issue is caused by invalid/outdated cert-store manager, and is most often caused by people who compile themselves and don't bother installing `ca-certificates-mono` as stated in official instruction - in this case you need to sync your certificates yourself, using e.g. `cert-sync --user /etc/ssl/certs/ca-certificates.crt` or `mozroots --import --sync`.

---

**Problem:**
```
System.TypeLoadException: A type load exception has occurred
```
```
System.TypeInitializationException: The type initializer for 'ArchiSteamFarm.Program' threw an exception. ---> System.IO.FileNotFoundException: Could not load file or assembly
```

**Solution:**
Make sure that you have ```mono-complete``` package installed. This issue is mostly caused by lack of some core Mono libraries required by ASF, that should be natively available. Usual reason for such situation is installing ```mono``` instead of ```mono-complete```, which includes all natively available assemblies. If installing ```mono-complete``` doesn't help, install latest Mono version.

**Problem:**
```
System.DllNotFoundException: /usr/lib/libMonoPosixHelper.so
```

**Solution:**
The problem is caused by lack of symbolic link, and it should be solved since version 4.6+. For now, I suggest to do:

```
ln -s /usr/lib64/libMonoPosixHelper.so /usr/lib/libMonoPosixHelper.so
```

---

**Problem:**
```
System.Security.Cryptography.CryptographicException: Private/public key mismatch
```
```
Error getting response stream (Write: The authentication or decryption has failed.): SendFailure
```

**Solution:**
- Make sure that you have ```ca-certificates-mono``` (or equivalent on your OS) installed.
- Update Mono to latest version, this issue is fixed since Mono 3.12+.
- Alternatively you can execute ```mozroots --import --sync``` manually. This should **never** be needed if you have ```ca-certificates-mono``` package installed.

---

**Problem:**
```
System.TypeLoadException: A type load exception has occurred
```
```
System.TypeInitializationException: The type initializer for 'ArchiSteamFarm.Program' threw an exception. ---> System.IO.FileNotFoundException: Could not load file or assembly
```

**Solution:**
Make sure that you have ```mono-complete``` package installed. This issue is mostly caused by lack of some core Mono libraries required by ASF, that should be natively available. Usual reason for such situation is installing ```mono``` instead of ```mono-complete```, which includes all natively available assemblies. If installing ```mono-complete``` doesn't help, install latest Mono version.