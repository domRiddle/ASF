# Mono

This page covers Mono-only issues and provides general help, that might be useful if you're running ASF under Mono. If you're running ASF natively on Windows, most likely you won't find anything useful here.

Most (if not all) of the issues are directly caused by outdated Mono. Mono shipped with your Linux distribution is almost never the latest released Mono version, and the newer Mono you have, the bigger chance that everything will run out of the box.

# How to install latest Mono?

On Debian-like Linux distributions (including Ubuntu-like), add to your ```/etc/apt/sources.list``` following line:

```
deb http://download.mono-project.com/repo/debian wheezy main
```

Then execute (as root, or with sudo):
```
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
apt-get update
apt-get install mono-complete
```

Afterwards, you should notice that ```mono --version``` command returns the same version that is currently marked as latest stable on **[official Mono page](http://www.mono-project.com/download/)**.

Keep in mind that instructions above are also available on **[official Mono installation page for debian-like distributions](http://www.mono-project.com/docs/getting-started/install/linux/#debian-ubuntu-and-derivatives)**. If you're not using Debian-like Linux distribution, and can't follow above ```apt-get``` tips, either start using one, or head over to **[official Mono installation page for other distributions](http://www.mono-project.com/download/#download-lin)** to get instructions that match your Linux distribution.

Officially, ASF requires only .NET 4.5 framework, which is supported since Mono 3.0+, which should be available in all current Linux distributions. However, only latest Mono guarantees bugless experience. In case of uncovered bugs without any available solution, or bugs with solutions that don't help, the only suggestion is to use latest Mono.

---

# Issues and solutions

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
- Alternatively you can execute ```mozroots --import --ask-remove``` manually.

---

**Problem:**
```
Unhandled Exception: System.TypeLoadException: A type load exception has occurred
```

**Solution:**
Make sure that you have ```mono-complete``` package installed. This issue is mostly caused by lack of some core Mono libraries required by ASF, that should be natively available. If installing ```mono-complete``` doesn't help, install latest Mono version.