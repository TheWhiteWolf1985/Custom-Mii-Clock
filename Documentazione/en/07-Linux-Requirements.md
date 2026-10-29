# Linux requirements and application installation

[Italiano](../it/07-Linux-Requirements.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

## Actual requirements

- **Physical Linux x86_64 PC** with glibc ≥ 2.39.
- systemd ≥ 249 running as the system manager, Polkit/pkexec and system D-Bus; a desktop authentication agent and administrator account.
- Required system tools present and protected against unprivileged modification; compatible Qt/XCB libraries, native ABI and `libfuse.so.2`.
- Direct USB, a reliable data cable, the same port throughout the session and stable power.
- Persistent POSIX filesystem, preferably ext4; no FAT/exFAT for the personal project folder.
- At least **32 GiB free** when the personal folder and staging share a disk; otherwise **20 GiB on the system and 12 GiB on the backup disk**.

The supplied package is a `.deb` for compatible Debian/Ubuntu systems; Ubuntu 24.04 LTS needs up-to-date dependencies. Checks use actual capabilities, not a distribution-name whitelist. This does not qualify every distribution or make the DEB suitable for Fedora/Arch. ARM, macOS, Windows, WSL, VMs, containers and musl/Alpine are not the supported USB workflow.

## Install and launch

1. Download the DEB and verification materials from the [installer release](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2).
2. [Verify signatures and checksums](12-Verification.md).
3. Open the DEB with the package manager and install it. System desktop libraries may require Internet.
4. Launch MiiClock Installer from the application menu as a normal user. Select IT, EN, FR, DE or ES at the top right.
5. Run the PC check; red requirements cannot be ignored.

The required Python, private environment, Qt and MTKClient/ADB/Fastboot/AVB tools are bundled. No global Python/pip installation or dependency compilation is required. Firmware is not included in the DEB.

Do not launch the GUI as root: Polkit authorizes the privileged worker when required. Do not run another ADB/Fastboot/MTKClient alongside the session. All five sections remain browsable; prerequisites gate actions only.

## Upgrade the application

Close the GUI and wait for active operations to finish. Open the newer DEB and select Install/Upgrade without uninstalling first. Personal sessions and backups are preserved. Upgrading the application does not send commands to the clock.

Software checks cover installation/GUI in Xubuntu 26.04 and an isolated Ubuntu 24.04.4 rootfs. They do not certify USB, desktop Polkit and flashing on every system. [RC status](01-Release.md).
