# Launcher-only updates

[Italiano](../it/10-APK-Updates.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

The APK RC is **0.26.5 / versionCode 58**, package `local.miiclock.launcher`. It is byte-identical to the copy embedded in firmware 032: a clock already running that pair does not need reinstalling to obtain the same version.

## Availability and compatibility

The [launcher release](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) is a prerelease. The integrated check based on `releases/latest` does not offer it automatically. The signed APK is available for manual download with `ota-manifest.json`, `ota-manifest.sig` and `SHA256SUMS` from the same release. The UI does not provide manual manifest import.

The signed range permits firmware builds 027–032. This is a compatibility requirement of the current APK, not an instruction to download other firmware. Native bootstrap/HOME fixes require 032 and cannot be obtained through the APK alone.

An APK update affects `/data/app` and does not change `product`, `super`, `boot`, AVB or other partitions. It does not require the factory reset used for first firmware installation. An APK cannot prepare a stock clock or grant new firmware privileges.

## Verification and consent

The OTA workflow checks the manifest's RSA signature, schema, package, official Android certificate, SHA-256, size, increasing versionCode, firmware range, space and transaction state. Update checks run at most every 24 hours; downloading and installation require explicit consent when a compatible update is offered.

Verify manually downloaded APKs against the authentic manifest. A matching filename is insufficient. Do not bypass checks, re-sign the APK or use another release's manifest. GitHub transports files; trust comes from signatures and local verification.

## Post-install verification and rollback

The engine preserves a verified copy of the preceding APK. The health check requires the correct package/version/signature, expected MainActivity, HOME role, a stable PID for 30 seconds, a consistent journal and absence of safe mode or blocking crashes/ANRs. The privileged watchdog verifies recovery using transaction-bound results; errors are not reported as successes.

Rollback targets the verified preceding APK; the copy embedded in `product` remains a fallback through removal of the system-app update. First migration from a legacy watchdog initially depends on the installed watchdog: recovery is not retroactively guaranteed and physical testing remains pending. Keep your backup and [recovery](09-Recovery.md) path available.
