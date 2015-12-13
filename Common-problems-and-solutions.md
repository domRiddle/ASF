# Mono-only
```
Unhandled Exception:
System.Security.Cryptography.CryptographicException: Private/public key mismatch
```
```
The authentication or decryption has failed
```

Solution:
- Update Mono to latest version, this issue is fixed since Mono 3.12+ OR
- Execute ```mozroots --import --ask-remove``` manually.