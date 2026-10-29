# Security, privacy and known limitations

[Italiano](../it/11-Security-Limits.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

## Firmware security trade-offs

The RC uses an unlocked bootloader, USB root ADB and SELinux Permissive. Physical USB access can provide privileged access; signatures and AVB do not remove this risk. ADB TCP remains disabled. Do not expose debugging or root-bridge ports to the network, change AVB keys or relock the bootloader.

A clean first firmware installation requires a verified backup and a manual post-flash reset, erasing data and configuration. Recovery is limited to the documented partitions and does not guarantee encrypted data. No procedure is risk-free.

## Validation status

Software and artifact checks are complete. The owner reports that the RC works on three personal clocks; installer revision 1.5.2, recovery on additional units and first legacy OTA migration do not have complete physical qualification. `hardware_qualified=false` remains explicit. Do not describe the candidate as universally qualified stable software.

## Known limitations and issues

- A HOME chooser may appear: select MiiClock and Always after provisioning. Do not reset just for the chooser.
- The persistent WebView shares the launcher process and the device has limited RAM. Modern Home Assistant frontend/card compatibility and extended sessions require testing on your own setup.
- Accumulation of terminated child processes (zombies) under the root bridge has been observed. The RC does not yet include a specific fix; its impact in extended tests remains to be assessed. It is not established as the cause of touch freezes.
- PC-to-clock audio mirroring is not qualified as working. The Media screen controls only sessions exposed by Android.
- Calendar viewing is read-only: no event creation/editing UI or cloud synchronization.
- The installer requires compatible physical Linux; VM/rootfs testing does not certify USB or desktop Polkit on a tester's PC.
- Google services, weather and dashboard also depend on networking and external services. MiiClock's alarm engine is separate from Google voice alarms.

## Personal data

Full backups, journals and raw logs can contain private data. Keep them out of repositories and public posts; read a redacted report before sharing it. Do not publish accounts, passwords, tokens, cookies, serials, MAC addresses or private keys.

Home Assistant uses its own session in the WebView profile. MQTT uses locally entered credentials: choose a trusted broker and appropriate privileges. Keep real credentials out of configuration screenshots. The installer does not automatically upload backups or collect telemetry.

[Security reporting](../../SECURITY.en.md) · [Support](../../SUPPORT.en.md).
