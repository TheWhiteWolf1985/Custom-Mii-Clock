# Recovery, failures and interruptions

[Italiano](../it/09-Recovery.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

The installer's Recovery section is optional and separate from installation. It uses only the verified personal backup of the same clock. It does not guarantee universal recovery or restoration of encrypted data.

## Failure before a write

If prerequisites, identity, signature, hash or AVB verification fail, stop and resolve the requirement. Do not replace loaders, manifests, images or tools with copies found online. A software report or an old backup does not authorize writing to the wrong device.

## Failure while flashing

Do not reboot, disconnect experimentally or run erase/format commands. Preserve the journal and read the error. The installer blocks subsequent operations and reboot after a failed or uncertain write. Use explicit recovery for the same device or request support with a redacted report.

Recovery restores only seven stock firmware partitions from the personal backup: `boot`, `recovery`, `dtbo`, `vbmeta_vendor`, `vbmeta_system`, `super`, `vbmeta`. Fastboot or BROM with verification readback is supported; `vbmeta` is written last. It does not restore the entire memory, GPT, preloader, bootloader, `seccfg`, `userdata` or `metadata`, and does not relock the bootloader.

## Incomplete boot or connection

First boot may take time. If Android does not start, distinguish a required post-flash reset that has not yet been performed from a write failure. Perform the reset only after a successful complete flash as described in [installation](08-Installation.md). Do not use a reset to bypass failed verification.

If Fastboot enumerates but does not respond, enter Recovery and select **Reboot to bootloader**. Do not assume that **Enter fastboot**, VID/PID or a port number establish a valid session. BROM requires the sequence and backup defined by the installer, not an improvised procedure.

## Closing or upgrading the GUI

Closing the GUI lets only the current command finish, then the worker stops at the checkpoint. It does not perform automatic rollback, retries or reboot. Reopen with the same folder and inspect state before resuming. Do not delete or replace a `recovery_required` state to force continuation.

The app reset in MiiClock settings affects its configuration and does not restore stock firmware. It is separate from Android Recovery and the installer's Recovery section.

Open the `log` folder, read the redacted report and follow [Support](../../SUPPORT.en.md). Do not publish dumps, serials, MAC addresses or credentials.
