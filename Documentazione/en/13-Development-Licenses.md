# Development, distribution and licensing

[Italiano](../it/13-Development-Licenses.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

## System roles

Private Forgejo holds the canonical source, pipelines, signing material and audits. GitHub publishes IT/EN documentation, public keys, descriptive metadata and assets for the three coordinated releases. It is not a private-source mirror and does not build firmware publicly. The installer release includes a separate source archive for reproducibility and licensing.

Manifest signatures, the APK certificate and AVB govern verification; GitHub is transport, not the root of trust. Private keys are not distributed and must not enter repositories, firmware, APKs, logs or public reports.

## Current technical contracts

Java launcher for Android 10/API 29 and an 800 × 480 canvas; separate components for Home, local services, persistent WebView, MQTT, MediaSession and overlay. Firmware embeds the APK, required privileges and bootstrap; builds verify LP geometry, ext4, sparse format and AVB chain. The canonical APK, firmware copy and separate distribution must be identical.

The installer uses an unprivileged GUI and a Polkit-authorized root worker, a local socket with peer verification, named actions and a journal. It does not expose arbitrary shells, offsets, loaders or partitions. Python/Qt runtime and tools are application-private; the installer does not replace the system's libc, kernel or desktop.

Releases require software testing, reproducible double builds, signature/hash verification and explicit acceptance of the physical scope. The current RC retains `hardware_qualified=false`. Documentation changes do not rebuild binaries or revalidate physical workflows.

## Contributions

Open an issue before changes to firmware, OTA, MQTT, Assistant or privileges. Include model, version and reproducible results with redacted data. Preserve APIs, schema versions, compatibility and complete translations; assess memory/CPU on the low-RAM profile. Binary changes require new checks and a consistent release rather than silent replacement of signed assets.

## Licensing

Original MiiClock code: Apache-2.0. Original documentation and images: CC BY 4.0, with attribution `MiiClock project — TheWhiteWolf1985 and contributors — CC BY 4.0.` Indicate modifications and do not imply endorsement of reuse.

Xiaomi/Google stock components, Android/AOSP, installer dependencies, MET Norway data and Open-Meteo/GeoNames location search retain their own licences and terms. MiiClock licences do not grant rights to third-party components. Read [Notice](../../NOTICE.en.md), the software licence and notices distributed with the installer. Notices are not legal certification of the derived firmware.
