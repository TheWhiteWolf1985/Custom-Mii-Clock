---
title: Ambienti, toolchain e operatività del progetto
aliases:
  - Toolchain MiiClock
  - Ambienti di build
  - Operatività X04G
tags:
  - miiclock
  - toolchain
  - xubuntu
  - forgejo
  - vault
  - ci
status: canonico
versione_documentazione: 1.0.0-RC1
last_reviewed: 2026-10-08
tipo: operativita
---

# Ambienti, toolchain e operatività del progetto

> [!abstract]
> MiiClock è costruito con una separazione intenzionale fra workstation Windows, VM Xubuntu collegata al device, vault cifrato e runner Forgejo. Questa separazione rende la build riproducibile senza mettere chiavi private o backup fisici nel repository.

## Topologia degli ambienti

| Ambiente | Ruolo | Dati che può contenere |
| --- | --- | --- |
| Workstation Windows | repository, documentazione, review, artefatti versionati | sorgenti, policy, report redatti, firmware derivato |
| VM Xubuntu | toolchain device, ADB/Fastboot, costruzioni e test fisici | tool riproducibili, output temporanei controllati |
| Vault LUKS | materiale riservato | backup fisico, chiavi private AVB, rollback, chiave firma launcher |
| CT Forgejo runner | CI e build manuali firmate | checkout effimero, secret materializzato solo in RAM |
| Forgejo homelab | sorgente Git canonica | codice, documentazione, manifest e artifact CI autorizzati |
| GitHub | distribuzione pubblica di release APK/firmware | solo asset pubblici approvati e firmati |

Indirizzi, login, token e chiavi non fanno parte della documentazione di rilascio. Sono gestiti nel runbook locale e nelle configurazioni protette dell'infrastruttura.

## Workstation e repository

Il repository è mantenuto localmente e versionato. Le aree principali sono:

```text
Firmware stock/     materiale stock e riferimento community
Nuovo firmware/     builder, launcher, candidati e policy
AI/                 runbook, audit, decisioni, prove tecniche
Documentazione/     wiki canonica per Obsidian e RAG
Mockup/             specifiche e riferimenti visivi
```

I file grandi intenzionali sono soggetti a Git LFS o alla policy del repository. Non si cancellano artefatti o lock Git attivi durante una build/stage: prima si verifica processo e `index.lock`.

## VM Xubuntu e tool device

La VM Xubuntu è l'ambiente fisico di riferimento perché riceve il passthrough USB diretto del Clock. Il protocollo Fastboot deve essere eseguito con `sudo fastboot`; ADB viene usato con il toolchain installato e il device viene sempre verificato prima di una scrittura.

Strumenti principali:

- Android platform-tools: `adb`, `fastboot`;
- MTKClient per backup, GPT e recupero BROM di ultima istanza;
- `avbtool` compatibile AVB 1.0;
- strumenti ext4/e2fsck, sparse image e confronto hash;
- Java/Gradle e Android SDK per il launcher;
- `fswebcam` per verifiche fisiche dello schermo quando disponibile.

La VM non deve diventare un deposito di release obsolete: output transitivi e screenshot temporanei vengono rimossi dopo che gli artefatti canonici sono nel repository o nel vault. Il vault viene montato solo per backup, firma, preflight o rollback e chiuso al termine.

## Runner Forgejo

Il runner è una CT Debian dedicata, con Java 17, Android SDK/API 36, build-tools, Git LFS, Python e OpenSSL. È limitato alla propria CT: non deve operare su altre VM/CT senza esplicita autorizzazione.

### Workflow CI

`launcher-ci` è eseguito sui branch previsti e controlla checkout, toolchain, build host e due build unsigned riproducibili. Non usa chiavi di firma.

### Workflow release

`launcher-release` è manuale e richiede versione e consenso di firma. Produce:

- APK release firmato;
- bundle OTA (APK, manifest e firma detached);
- quando versione launcher e configurazione firmware coincidono, bundle firmware completo;
- doppie build confrontate byte per byte;
- report di verifica e checksum.

I secret vengono creati in `/dev/shm` con permessi `0600` e rimossi sempre, anche in caso di errore. La pipeline non pubblica automaticamente su GitHub.

## Criteri di riproducibilità

Una build non è considerata pronta solo perché Gradle termina con successo. Sono richiesti:

1. versioni toolchain fissate e registrate;
2. due output indipendenti byte-identici quando l'artefatto è deterministico;
3. firma APK v2 e fingerprint ufficiale;
4. hash/size/package/compatibilità dell'APK;
5. validator firmware, ext4 e AVB;
6. sparse round-trip e differenza limitata a `product`;
7. manifest, checksum e report redatti committati prima di un flash.

Le esecuzioni CI già note come verdi restano prove storiche; per una release nuova si esegue sempre una pipeline nuova e si collega il relativo report.

## Vault e materiale sensibile

Il vault LUKS conserva backup fisico per-partizione, full-flash, chiavi private AVB, certificato/firma launcher e copie rollback. La salute del vault viene verificata con apertura read-only e `e2fsck -fn`; non si monta in scrittura se non serve.

Nel repository possono esistere chiavi pubbliche, fingerprint, hash, policy e report privi di dati personali. Non possono esistere chiavi private, password Wi-Fi, credenziali MQTT/Home Assistant, token GitHub/Forgejo, seriali, MAC o `userdata`.

## Runbook operativo

I comandi dettagliati e dipendenti dal contesto sono in [[Documentazione/13-Fonti-e-Prove-RC1|Runbook]]. Per la sequenza deliberata di flash/rollback si usa [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Flash, recovery e quality gate]], non un comando copiato da un vecchio log.

## Collegamenti

[[Documentazione/03-Architettura-Firmware-e-Build|Build firmware]] · [[Documentazione/08-OTA-e-Distribuzione-APK|Pipeline OTA]] · [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Procedura fisica]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Protezione dei segreti]].
