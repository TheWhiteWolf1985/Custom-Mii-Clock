# Inventario attivo

## Device e runtime verificati

- Xiaomi Mi Smart Clock X04G (`mico_x04g`), Android 10, identificativi univoci sempre redatti nei report versionati.
- Partizione dinamica `super`, nessuno slot A/B.
- Bootloader verificato `unlocked: yes`, `secure: no`; AVB runtime `orange` con verity `enforcing` sulla baseline 014.
- Firmware v0.1.2: Magisk attivo, ADB USB immediato con shell `uid=0(root)`, SELinux `Permissive`, ADB TCP disabilitato.
- Launcher `local.miiclock.launcher/.MainActivity`, versione 0.1.0, verificato sul display.

## Artefatti conservati

- Stock compresso: `Firmware stock/dump_mico_x04g.tar.xz`, 857491844 byte, SHA-256 `9ffd2a4b1b66d7f1ae0496b5e7e2f4db3e1d14cad74d03802f1df30d64dde214`.
- Backup fisico nel vault: `/mnt/miiclock-vault/backups/x04g-physical-20260912-001/`, inclusi full-flash, boot1, boot2, preloader, partizioni tecniche e super sparse di rollback.
- Baseline modificata: `Nuovo firmware/Fase 1 - Modifica firmware/candidates/x04g-magisk-visible-launcher-014/`, autonoma e verificata da `SHA256SUMS`.
- Baseline duplicata nel vault con seconda build indipendente del boot e `VAULT_SHA256SUMS`.
- Chiave AVB privata soltanto nel vault; nel repository sono presenti esclusivamente chiavi pubbliche e fingerprint.

## Ambiente operativo

- VM Xubuntu: `codex@192.168.1.248`, USB passthrough diretto.
- Toolchain congelata: `/opt/miiclock-toolchain` e copia offline sotto `Nuovo firmware/Fase 1 - Modifica firmware/toolchain/`.
- Vault LUKS: `/var/lib/miiclock-vault/miiclock-vault.luks`, montato soltanto tramite wrapper in `/mnt/miiclock-vault`.
- Webcam: `fswebcam` per verificare lo stato reale del display.
- Fastboot valido soltanto dopo Recovery -> **Reboot to bootloader** e risposta positiva di `sudo fastboot getvar product`.

## Struttura Git

- `Base_version`: baseline immutabile v0.1.2.
- `develop`: integrazione.
- `main`: stabile.
- Ogni feature futura nasce su un branch dedicato e viene unita a `develop` solo dopo i propri gate.

## Cosa non assumere

- Che la semplice enumerazione fastboot dimostri un trasporto operativo.
- Che un artefatto community sia un rollback del device.
- Che una validazione offline dimostri USB, ADB o UI runtime.
- Che GSI/Lineage o la strategia `system` della release 013 siano basi riutilizzabili.
- Che una futura modifica possa scrivere partizioni non elencate in un manifest committato e pushato.
