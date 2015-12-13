# Mono-only
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
Make sure that you have ```mono-complete``` package installed, and latest Mono version. On debian-like operating systems, ```apt-get update && apt-get dist-upgrade && apt-get install mono-complete``` should do the trick.