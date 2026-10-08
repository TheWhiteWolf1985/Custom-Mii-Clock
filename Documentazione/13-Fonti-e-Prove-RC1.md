---
title: Fonti pubbliche e prove della RC1
aliases:
  - Audit pubblico RC1
  - Fonti RC1
tags:
  - miiclock
  - x04g
  - rc1
  - audit
  - public-release
status: pubblico
versione_documentazione: 1.0.0-RC1
release_firmware: "0.1.16 (028)"
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: riferimento
---

# Fonti pubbliche e prove della RC1

> [!abstract]
> Questa pagina sostituisce nell'export pubblico i collegamenti alle note
> operative private. Riassume le prove pubblicabili e chiarisce ciò che deve
> essere ancora verificato prima della pubblicazione di asset firmware.

## Fonte tecnica

La sorgente canonica del progetto, comprese build firmate, audit completi,
backup e materiale di recovery, resta nel Forgejo privato. Il repository
GitHub contiene una copia pubblica selezionata e non deve essere considerato
un archivio completo dell'ambiente di sviluppo.

## Evidenze pubblicabili RC1

- firmware target `0.1.16 (028)` e launcher `0.25.2`/code `51`;
- APK canonico, asset OTA e APK incorporato devono essere byte-identici;
- due build firmware indipendenti devono essere byte-identiche;
- validazione ext4, sparse round-trip e catena AVB con flags zero;
- `system` e `vendor` invariati rispetto alla baseline;
- rimozione controllata del Google Launcher e conservazione di Assistant Core,
  MediaShell e Provision;
- spazio libero finale `product` sopra la soglia progettuale;
- pipeline di rilascio manuale, senza chiavi o pubblicazione automatica su
  GitHub.

## Stato della pubblicazione

Un artifact firmware non viene caricato su GitHub finché non supera il gate
fisico definito nella Release: Fastboot X04G verificato, flash controllato,
boot, ADB USB, UI, servizi e rollback pronti. Il file
`RELEASE-METADATA.json` è la sintesi leggibile da strumenti e utenti.

## Collegamenti

[[Documentazione/00-Indice-Wiki|Indice wiki]] ·
[[Documentazione/01-Identita-Release-RC1|Identità RC1]] ·
[[Documentazione/03-Architettura-Firmware-e-Build|Firmware]] ·
[[Documentazione/10-Flash-Recovery-e-Quality-Gates|Quality gate]].
