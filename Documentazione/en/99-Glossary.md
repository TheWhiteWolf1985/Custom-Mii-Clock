# Glossary

[Italiano](../it/99-Glossary.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

| Term | Meaning in this RC |
| --- | --- |
| X04G / mico_x04g | Supported model and product; LX04 is incompatible |
| RC / prerelease | Tester candidate, not universal stable qualification |
| Firmware 032 | 0.1.20 bundle with native components and embedded launcher |
| APK | MiiClock Android application 0.26.5/58 |
| HOME | Launcher role assigned after provisioning |
| Recovery | Android environment for bootloader entry and manual reset |
| Fastboot | Protocol used by the installer for identity and permitted writes |
| BROM | MediaTek mode used for acquisition and authorized operations |
| product / super | Logical embedded-app partition and its dynamic container |
| AVB | Firmware image-chain verification; not equivalent to a locked bootloader |
| Manifest / detached signature | Authenticated metadata and a separate signature; distinct for installer, firmware and APK |
| SHA-256 | Integrity digest to compare against the authentic manifest |
| OTA | APK-only update without firmware flashing or clean-install reset |
| WebView | Android component displaying Home Assistant in the carousel |
| MQTT Discovery | Configuration allowing Home Assistant to create clock entities |
| MediaSession | State and controls exposed by an Android media provider |
| MiiPulse | Visual overlay of Assistant conversation state |
| Journal / checkpoint | Persistent operation state for explicit resumption or recovery |
| hardware_qualified=false | Complete physical qualification of the installer has not been obtained |
| Zombie | Terminated process whose parent has not collected its exit status |
