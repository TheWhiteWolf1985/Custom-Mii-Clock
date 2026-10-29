# RC verification

[Italiano](VERIFY_RELEASE.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

The three releases use separate installer, firmware and APK manifests. Verify checksums and signatures with the official public key, then compare filename, version, size and digest. Checksums alone do not authenticate the publisher.

[Complete Linux commands and APK identity](Documentazione/en/12-Verification.md) · [Versions and downloads](Documentazione/en/01-Release.md).

The installer also checks clock identity, geometry, LP metadata and AVB before writing. Software integrity and a successful check do not replace physical testing of every function and recovery. `hardware_qualified=false` remains the installer 1.5.2 limitation.
