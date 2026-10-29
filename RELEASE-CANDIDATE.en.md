# Download the current RC

[Italiano](RELEASE-CANDIDATE.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

| Component | Version | Download |
| --- | --- | --- |
| Graphical Linux amd64 installer | 1.5.2 | [installer-v1.5.2](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2) |
| X04G firmware | 0.1.20 (032) | [firmware-v0.1.20](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/firmware-v0.1.20) |
| Integrated launcher and separate APK | 0.26.5 / code 58 | [launcher-v0.26.5](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) |

For a stock device, use the DEB and the [complete guide](Documentazione/en/08-Installation.md). The installer downloads the explicit 032 firmware; offline import requires the ZIP, `firmware-manifest.json` and `firmware-manifest.sig` together. The GUI verifies authenticity and compatibility before writing. Dependency sources are not needed for normal installation.

A clean first installation requires a **verified personal backup, unlocking, complete flash and subsequent manual reset**, which erases user data. Use a physical Linux PC meeting the [requirements](Documentazione/en/07-Linux-Requirements.md). Do not reset before flashing, change AVB or relock the bootloader.

The preparation README inside the signed firmware ZIP does not describe every current step; follow the guide linked here. Release ZIPs, signatures and manifests remain unchanged.

The APK RC is not automatically offered by `releases/latest`; an APK-only update does not update firmware or require a reset. [Compatibility and updates](Documentazione/en/10-APK-Updates.md).

**Qualification:** software and integrity checks complete; operation on three clocks reported by the owner; new complete physical testing of installer 1.5.2 and recovery pending (`hardware_qualified=false`). A possible HOME chooser and accumulation of root-bridge zombie processes are also documented, without an established causal link to freezes. Read [evidence](Documentazione/en/01-Release.md), [limitations](Documentazione/en/11-Security-Limits.md) and [signature verification](Documentazione/en/12-Verification.md).
