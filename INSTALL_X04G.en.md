# X04G firmware installation

[Italiano](INSTALL_X04G.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

Use MiiClock Installer 1.5.2 on a compatible physical Linux PC to install firmware 0.1.20 (032) with launcher 0.26.5/58. This workflow is for X04G / `mico_x04g` only.

1. [Read requirements and install the DEB](Documentazione/en/07-Linux-Requirements.md).
2. [Verify assets](Documentazione/en/12-Verification.md).
3. [Follow every step of the complete guide](Documentazione/en/08-Installation.md): identity check, personal backup, verified unlocking, signed firmware, seven writes and a manual reset after the complete flash.
4. Complete Google/Home provisioning and check 4.7, then test functions on your own clock.

The reset erases user data. Do not perform it before flashing or follow conflicting preparation instructions inside the signed ZIP. Do not improvise Fastboot/erase commands or use another device's backup.

On failure, follow [Recovery](Documentazione/en/09-Recovery.md). [APK-only updating](Documentazione/en/10-APK-Updates.md) is separate and does not require this reset.
