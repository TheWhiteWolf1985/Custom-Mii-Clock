# Pulizia e baseline firmware 014

Data: 2026-09-14

## Obiettivo

Ridurre il progetto a tre insiemi non ambigui:

1. firmware stock compresso e immutabile nel repository;
2. backup fisico completo e rollback nel vault LUKS;
3. prima versione modificata funzionante `x04g-magisk-visible-launcher-014` (firmware v0.1.2), autonoma nel repository e nel vault.

Gli esperimenti 011, 012 e 013 restano documentati in `AI/`, ma i relativi artefatti binari, manifest operativi e report specifici vengono rimossi.

## Allowlist repository

- `Firmware stock/` completo, incluso `dump_mico_x04g.tar.xz`.
- `Nuovo firmware/Fase 0 - Reverse engineering/` senza le copie binarie estratte `partitions/` e `logical-partitions/`; restano strumenti e report di analisi.
- `Nuovo firmware/Fase 1 - Modifica firmware/candidates/x04g-magisk-visible-launcher-014/` completo e autonomo.
- Manifest correnti:
  - `20260913-x04g-magisk-visible-launcher-flash-014.json`;
  - `20260914-x04g-physical-rollback-014.json`.
- Manifest di acquisizione fisica:
  - `20260912-x04g-physical-backup-001.json`;
  - `20260912-x04g-full-flash-backup-004.json`;
  - `20260912-x04g-boot-regions-backup-005.json`;
  - `20260912-x04g-preloader-backup-006.json`.
- Evidenze 014, toolchain, vault e backup fisico riuscito/analizzato (gate 004-009).
- Toolchain offline, chiavi AVB pubbliche, sorgenti launcher, mockup, documentazione e strumenti generici di build/validazione/rollback.

## Allowlist vault LUKS

- `/mnt/miiclock-vault/backups/` completo e non modificato.
- `/mnt/miiclock-vault/keys/` completo e non modificato.
- `/mnt/miiclock-vault/candidates/x04g-magisk-visible-launcher-014/` completo, inclusa la seconda build indipendente del boot.
- File di inizializzazione del vault e directory tecniche del filesystem.

## Delete-list repository

- Candidate obsoleti:
  - `candidates/x04g-logo-padding-sentinel-011/`;
  - `candidates/x04g-visible-launcher-012/`;
  - `candidates/x04g-open-root-adb-013/`.
- Copie binarie stock già ricavabili dall'archivio compresso:
  - `Fase 0 - Reverse engineering/partitions/`;
  - `Fase 0 - Reverse engineering/logical-partitions/`.
- Output intermedi sostituiti dalla 014:
  - `Fase 1 - Modifica firmware/artifacts/`.
- Report specifici di esperimenti ormai conclusi/falliti:
  - `reports/device-gate/brom-preflight/`;
  - `reports/device-gate/logo-sentinel/`;
  - `reports/device-gate/open-root-adb/`;
  - `reports/device-gate/visible-launcher/`;
  - `reports/device-gate/physical-backup/20260912-002/`;
  - `reports/device-gate/physical-backup/20260912-003/`;
  - `reports/device-gate/physical-backup/20260913-logo-sentinel-011/`;
  - `reports/device-gate/physical-backup/20260913-project-avb-chain-010/`.
- Manifest preflight/falliti e release obsolete: BROM 001-004, physical backup 002-003, logo 011, firmware 012 e 013.
- Tool one-shot o specifici degli artefatti eliminati:
  - `build_system_open_root_adb.sh`;
  - `create_logo_padding_sentinel.py` e relativo test;
  - `prepare_magisk_candidate_014.py` e relativo test;
  - `validate_open_root_adb_candidate.py`;
  - `validate_visible_candidate.py`.
- Output duplicati o rigenerabili del launcher (`dist/`, `.gradle/`, `build/`, `app/build/`) e report roundtrip che puntava a una `super.repacked.bin` eliminata.

## Delete-list vault LUKS

- `/mnt/miiclock-vault/analysis/`;
- `/mnt/miiclock-vault/candidates/x04g-logo-padding-sentinel-011/`;
- `/mnt/miiclock-vault/candidates/x04g-visible-launcher-012/`;
- `/mnt/miiclock-vault/candidates/x04g-project-avb-v1-unmodified-010/`;
- `/mnt/miiclock-vault/candidates/x04g-magisk-visible-launcher-014-build-a/`;
- `/mnt/miiclock-vault/candidates/x04g-magisk-visible-launcher-014-build-b/`;
- `/mnt/miiclock-vault/candidates/x04g-magisk-visible-launcher-014-staging/`.

## Gate eseguiti prima di cancellare

- La 014 contiene direttamente APK, product, super sparse, boot, recovery, dtbo e tutti i vbmeta necessari.
- Tutti i file della 014 verificano contro `SHA256SUMS`.
- `validate_magisk_candidate.py` restituisce `MAGISK_CANDIDATE_014_OK` usando il backup fisico e la seconda build indipendente nel vault.
- Il manifest 014 verifica 4/4 artefatti; il rollback fisico verifica 6/6 artefatti.
- La directory finale 014 nel vault possiede un proprio `VAULT_SHA256SUMS` verificato.
- La suite degli strumenti conservati supera 24/24 test automatici.

## Struttura Git richiesta

- `Base_version`: baseline immutabile v0.1.2.
- `develop`: integrazione delle future feature.
- `main`: linea stabile.
- Tag annotato `v0.1.2-working-base` sulla stessa baseline.

I tre branch devono puntare allo stesso nuovo root commit, privo della cronologia contenente i grandi artefatti cancellati. Prima della pubblicazione viene mantenuto un riferimento locale temporaneo al vecchio commit; viene rimosso soltanto dopo verifica del remoto.

## Stato

Preparazione e validazione della 014 autonoma: **PASS**.

- Repository di lavoro: 2204984171 byte e 251 file, esclusa `.git`; un solo candidato firmware presente e nessuna cache/build/checkout esterna ignorata.
- Vault dopo la pulizia: 7933202566 byte occupati; recuperati 4985381963 byte.
- Parti protette: backup 6609696714 byte, chiavi 4509 byte, baseline 014 1323501246 byte; dimensioni invariate durante la delete-list.
- Gate dopo la pulizia: candidato 014 **PASS**, rollback fisico 6/6 **PASS**, vault smontato e mapper chiuso.
- Pubblicazione: **PASS**. `main`, `Base_version` e `develop` puntano allo stesso root commit senza genitori; il tag annotato `v0.1.2-working-base` punta alla stessa baseline.
