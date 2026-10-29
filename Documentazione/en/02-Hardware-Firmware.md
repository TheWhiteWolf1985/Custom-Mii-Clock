# X04G hardware and firmware

[Italiano](../it/02-Hardware-Firmware.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

## Supported device

Xiaomi Mi Smart Clock X04G, Fastboot product `mico_x04g`, Android 10/API 29, 800 × 480 display and low-RAM profile. The commercial name or a single USB enumeration does not prove identity: the installer verifies the product, identity and geometry of the same unit throughout the session.

## Firmware composition

Firmware 0.1.20 (032) embeds launcher 0.26.5/58 as a privileged application in `product/priv-app`, inside the dynamic `super` partition. `system` and `vendor` remain stock. AVB and its chains must remain consistent with the project's public keys; the manifest signature and hashes identify the expected bundle.

The Google launcher is removed from the candidate. Assistant Core, MediaShell and provisioning components are preserved. The RC includes first-boot bootstrap, a privileged bridge limited to the MiiClock package and an OTA recovery watchdog. An APK alone cannot install these native components.

The device uses an unlocked bootloader, USB root ADB and SELinux Permissive. ADB TCP is disabled. These firmware security compromises remain despite AVB and signature verification.

## Activation and partitions

Google/Home provisioning must complete before MiiClock takes the HOME role. A chooser may appear: select MiiClock and Always. The reset required for a clean first installation happens after flashing, not before.

The installer writes only this authorized sequence: `boot`, `recovery`, `dtbo`, `vbmeta_vendor`, `vbmeta_system`, `super`, `vbmeta`. `vbmeta` is last. It does not rewrite GPT, preloader, bootloader, `userdata` or `metadata` as a shortcut to the procedure.

A visible Fastboot USB device is insufficient: select **Reboot to bootloader** in Recovery. **Enter fastboot** may enumerate an interface that does not answer the required protocol. Follow [installation](08-Installation.md) and [recovery](09-Recovery.md).
