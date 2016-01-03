# Common Mono issues

This page covers Mono-only issues, that might happen if you're running ASF under Mono. If you're running ASF natively on Windows, most likely you won't find anything useful here.

Most (if not all) of the issues are directly caused by outdated Mono version. Mono shipped with your Linux distribution is almost never the latest released Mono version, and the newer Mono you have, the bigger chance that everything will run out of the box.

# How to install latest Mono?

On Debian-like Linux distributions (including Ubuntu-like), add to your ```/etc/apt/sources.list``` following line:

```
deb http://download.mono-project.com/repo/debian wheezy main
```

Then execute (as root, or with sudo):
```
apt-get update
apt-get install mono-complete
```

Afterwards, you should notice that ```mono --version``` command returns the same version that is currently marked as latest stable on **[official Mono page](http://www.mono-project.com/download/)**

If you're not using Debian-like Linux distribution, and can't follow above ```apt-get``` tips, either start using one, or head over to **[official Mono installation page](http://www.mono-project.com/download/#download-lin)** to get instructions that match your Linux distribution.

Officially, ASF requires only .NET 4.5 framework, whish is supported since Mono 3.0+, which should be available in all current Linux distributions. However, only latest Mono guaranteed bugless experience.

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
Make sure that you have ```mono-complete``` package installed, and latest Mono version. On debian-like operating systems, ```apt-get update && apt-get install mono-complete``` should do the trick.