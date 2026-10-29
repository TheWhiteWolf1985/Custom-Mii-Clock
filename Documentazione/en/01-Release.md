# RC versions, downloads and validation status

[Italiano](../it/01-Release.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

MiiClock consists of firmware derived from X04G stock software, an integrated launcher and a graphical Linux installer. This is a tester prerelease for `mico_x04g`; LX04 and other smart displays are incompatible.

| Component | Version | Official release |
| --- | --- | --- |
| Linux amd64 installer | 1.5.2 | [Installer](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2) |
| Firmware | 0.1.20, build 032 | [Firmware](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/firmware-v0.1.20) |
| Integrated launcher and separate APK | 0.26.5, versionCode 58 | [Launcher](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) |

The package is `local.miiclock.launcher`. The separately released APK and the copy embedded in the firmware are byte-identical. The installer targets this firmware/launcher pair.

## What to download

For installation from stock: `miiclock-installer-1.5.2-amd64.deb`, `installer-manifest.json`, `installer-manifest.sig` and `SHA256SUMS` from the same installer release. Private application dependencies are bundled; the installer downloads firmware separately.

For offline firmware import: `miiclock-x04g-firmware-0.1.20.zip`, `firmware-manifest.json` and `firmware-manifest.sig`, all from the firmware release. `miiclock-x04g-firmware-0.1.20-SHA256SUMS.txt` is available for manual verification.

The separate APK is for an existing compatible MiiClock installation. It cannot prepare a stock clock or install native firmware changes. Follow [APK updates](10-APK-Updates.md).

## Verified status and limitations

- Installer Linux/Qt software tests: 550 passed; installation, upgrade and integrity checks completed.
- Firmware and APK: signatures, hashes, compatibility, AVB and embedded APK identity verified; 19 assets across the three releases downloaded again and checked in the October 24, 2026 audit.
- On October 29, 2026, the owner reports that the software works on three personal clocks. This is personal usage evidence, not certification of every unit.
- Full new physical testing of installer 1.5.2 and all recovery paths remains pending: `hardware_qualified=false`.
- First OTA migration from a legacy watchdog remains unqualified. The APK RC is not automatically offered by the stable channel.

Current instructions take precedence over the preparation README preserved inside the signed firmware ZIP: a clean first installation requires a **manual reset after the complete flash**, erasing user data. Signed assets remain unchanged. Full procedure: [installation](08-Installation.md).
