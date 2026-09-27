# AI_TASKS

## Gate 1A — Repack `super` invariato

- Status: DONE
- Evidence: `Nuovo firmware/Fase 1 - Modifica firmware/reports/gate_1_roundtrip.json`.
- Result: output byte-identico allo stock, SHA-256 `271127b4d3a901b0a3c8ad6f08de6c915a39a0da7ad4f5d4ddf05548f8f3aca7`.

## Gate 1B — Trasporto ADB/fastboot nativo

- Status: DONE
- Evidence: report redatti in `reports/device-gate/`.
- Result: X04G Android 10, USB diretto Xubuntu, fastboot non-A/B e partizioni dinamiche confermati.

## Gate 1C — Toolchain riproducibile

- Status: DONE
- Evidence: `reports/toolchain/20260912-xubuntu-toolchain.json`, `toolchain/toolchain.lock.json` e test in `tools/tests/`.
- Result: sorgenti e dipendenze offline hashati; MTKClient e avbtool eseguibili; vbmeta stock letto come AVB 1.0.

## Gate 2 — Backup fisico cifrato

- Status: DONE
- Storage: contenitore-file LUKS2 da 16 GiB sul disco VM esistente, autorizzato dall'utente e verificato chiuso.
- Scope: LUKS, full flash, boot1, boot2, preloader e dump per-partizione, solo lettura.
- Acceptance: hash completi nel vault; manifest e report in Git; GPT primaria/secondaria e dimensioni validate; partizioni tecniche estratte e classificate.

## Gate 3 — Prova recovery `logo`

- Status: DONE
- Prerequisites: Gate 2 completo, `logo` originale fisica verificata, sentinella nel padding provata offline, due manifest pronti e pushati.
- Authorization: frase utente esatta `flash ora` prima della prima scrittura.
- Acceptance: boot+ADB con sentinella, restore immediato, secondo boot+ADB stock.
- Prepared: candidato 011 con un solo byte nel padding; verifica hash on-device via ADB root; transizioni ADB/fastboot e timeout boot fail-closed.
- Result: sentinella scritta e verificata on-device, boot completo, originale fisica ripristinata e riverificata, secondo boot completo, provisioning invariato e schermata stock normale.

## Gate 4 — Catena AVB del progetto

- Status: DONE
- Scope: topologia derivata dagli artefatti fisici, chiave privata solo LUKS, chiave pubblica/fingerprint in Git, nessun bypass.
- Acceptance: doppia build byte-identica; algoritmi, descriptor, rollback index, flags, dimensioni, chiavi attese e catena completa validati offline.

## Gate 5 — Primo candidato visibile

- Status: READY_FOR_VISIBLE_REQUIREMENT_SELECTION
- Scope: una modifica minima ma osservabile richiesta dal PRD, con rigenerazione layout-preserving di `super` e della catena AVB.
- Acceptance: effetto visibile sul display, diff limitato, rollback pronto, manifest/tag immutabili pushati; flash solo con nuova conferma esplicita.
- Requirement update: il solo file sentinella non eseguibile non soddisfa l'aspettativa utente per il prossimo flash; non usarlo come unico risultato del candidato.
