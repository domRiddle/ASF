# Mono

This page covers Mono-related issues, instructions, questions and provides general help, that might be useful if you're running ASF under Mono. If you're running ASF natively on Windows, most likely you won't find anything useful here.

# How to install latest Mono?

On Debian-like Linux distributions (including Ubuntu-like), add to your ```/etc/apt/sources.list``` following line:

```
deb http://download.mono-project.com/repo/debian wheezy main # Yes, wheezy also covers other versions and distributions
```

Then execute (as root, or with sudo):
```
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
apt-get update
apt-get install mono-complete
apt-get dist-upgrade # Not needed usually, but good to do
```

Afterwards, you should notice that ```mono --version``` command returns the same version that is currently marked as latest stable on **[official Mono page](http://www.mono-project.com/download/)**.

Keep in mind that instructions above are also available on **[official Mono installation page for debian-like distributions](http://www.mono-project.com/docs/getting-started/install/linux/#debian-ubuntu-and-derivatives)**. If you're not using Debian-like Linux distribution, and can't follow above ```apt-get``` tips, either start using one, or head over to **[official Mono installation page for other distributions](http://www.mono-project.com/download/#download-lin)** to get instructions that match your Linux distribution.

Officially, ASF requires only .NET 4.6.1 framework, which is supported since Mono 4.0+, which should be available in all current Linux distributions. However, only latest Mono guarantees bugless experience. In case of uncovered bugs without any available solution, or bugs with solutions that don't help, the only suggestion is to use latest Mono.

**All hard-to-explain issues without obvious cause are automatically ignored, if your Mono version is not latest stable one.**

It's your responsiblity to ensure that ASF is run in bugless environment, and not mine to workaround all possible Mono issues, in all possible versions, on all possible variants. Latest stable ASF is always tested on up-to-date stable Mono version, and always includes all crucial code changes that are required for working properly on Mono (if needed).

---

# Alpha

Apart from ```stable``` channel, Mono's repo also offers ```alpha``` channel, with more recent, not yet stable Mono versions. This is the Mono version that is officially tested before releasing stable ASF, as it's a Mono version that offers a good balance between recent codebase, and stability.

Switching to alpha is not recommended in general, as Mono included there may not be always stable, but it may be needed to update if you for some reason require e.g. bugfix which was not yet released on stable channel.

To switch to alpha versions, modify the line that you added to ```/etc/apt/sources.list```:

```
deb http://download.mono-project.com/repo/debian wheezy main
```

To:

```
deb http://download.mono-project.com/repo/debian alpha main
```

Then issue ```apt-get update && apt-get install mono-complete``` to update.

Remember that alpha versions are unstable and might not work, therefore you should avoid them unless you want to live on the bleeding edge.

**TIP:** There are also other versions available: ```nightly``` > ```alpha``` > ```beta``` > ```stable```

```nightly``` is usually very unstable and has many issues, but includes also most up-to-date bleeding edge codebase, while ```stable``` is carefully tested in order to work on as many setups as possible.

```alpha``` and ```beta``` are a balance between ```nightly``` and ```stable```. Usually ```alpha``` versions are quite stable and should work on **most** machines.

---

# Issues and solutions

Most (if not all) of the issues are directly caused by outdated Mono. Mono shipped with your Linux distribution is almost never the latest released Mono version, and the newer Mono you have, the bigger chance that everything will run out of the box.

**Solutions available here are workarounds for outdated Mono versions. Those issues do not exist on up-to-date Mono, and you should already know that you're on your own regarding outdated Mono versions, so consider it more like a helpful hand.**

**Problem:**
```
Unhandled Exception:
System.Security.Cryptography.CryptographicException: Private/public key mismatch
```
```
Error getting response stream (Write: The authentication or decryption has failed.): SendFailure
```

**Solution:**
- Update Mono to latest version, this issue is fixed since Mono 3.12+.
- Alternatively you can execute ```mozroots --import --sync``` manually.

---

**Problem:**
```
Unhandled Exception: System.TypeLoadException: A type load exception has occurred
```

**Solution:**
Make sure that you have ```mono-complete``` package installed. This issue is mostly caused by lack of some core Mono libraries required by ASF, that should be natively available. If installing ```mono-complete``` doesn't help, install latest Mono version.