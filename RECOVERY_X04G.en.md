# X04G recovery

[Italiano](RECOVERY_X04G.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

Installer 1.5.2's Recovery section uses only the verified personal backup of the same X04G and restores the seven specified firmware partitions. It is not full-memory or encrypted-data restoration and does not relock the bootloader.

**If a write fails or is uncertain, stop without rebooting, erasing or formatting.** Keep the folder, journal and logs. Closing the GUI lets only the current command finish, then stops at a checkpoint; it is not automatic rollback.

[Complete procedure and failure cases](Documentazione/en/09-Recovery.md) · [Installation and post-flash reset](Documentazione/en/08-Installation.md) · [Support](SUPPORT.en.md).
