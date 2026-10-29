# Verify files and results

[Italiano](../it/12-Verification.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

There are three separate manifests: `installer-manifest.json/.sig`, `firmware-manifest.json/.sig` and `ota-manifest.json/.sig`. They are not interchangeable. Download files, signature and checksums from the same release. The official public key is `keys/miiclock-ota-manifest-v1-public.pem` in the repository; do not trust only a key shipped alongside an unknown download.

## Manual Linux checks

Run these examples in the folder containing the downloaded assets and the `keys` subfolder copied from the official repository. For the installer:

```bash
sha256sum --ignore-missing -c SHA256SUMS
openssl dgst -sha256   -verify keys/miiclock-ota-manifest-v1-public.pem   -signature installer-manifest.sig installer-manifest.json
```

For firmware:

```bash
sha256sum -c miiclock-x04g-firmware-0.1.20-SHA256SUMS.txt
openssl dgst -sha256   -verify keys/miiclock-ota-manifest-v1-public.pem   -signature firmware-manifest.sig firmware-manifest.json
```

For the APK, in a separate folder with all four assets from the launcher release:

```bash
sha256sum --ignore-missing -c SHA256SUMS
openssl dgst -sha256   -verify keys/miiclock-ota-manifest-v1-public.pem   -signature ota-manifest.sig ota-manifest.json
```

Files you intend to use must report `OK`; finding no checksums or missing a file you will install is not success. Signature verification must report `Verified OK`. Also check filename, version, size and digest inside the authenticated manifest. Checksums alone do not authenticate the publisher.

## RC APK identity

- Package: `local.miiclock.launcher`.
- Version: `0.26.5`, versionCode `58`.
- APK SHA-256: `3b03ed8eb03972b589a032388f3e233f6e66eddff73688b4d8880f17b2655c5d`.
- Android certificate SHA-256: `7340f6343323e0474c4b6afc0e5891aa9e7f5d5176606be82b6124a39631c029`.
- Firmware ZIP SHA-256: `d7b5db518c5468d9ad01e04af6495692b9bc8883fe28680d124033eb70a4f9bb`.

## What verification establishes

Before writing, the installer checks signatures/hashes, LP metadata, geometry and AVB. Comparison with an authentic manifest identifies files; it does not prove that your clock completed flashing, reset or provisioning.

Check 4.7 verifies observable boot/image state, while touch, Wi-Fi, audio, alarms and Home Assistant interaction require manual testing. Full physical qualification of the installer and recovery paths remains pending. [Status and evidence](01-Release.md).
