---
title: OTA e distribuzione dell APK MiiClock
aliases:
  - OTA MiiClock
  - Distribuzione APK
  - Release APK
tags:
  - miiclock
  - ota
  - apk
  - github-releases
  - signing
  - rollback
status: canonico
versione_documentazione: 1.0.0-RC1
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: distribuzione
---

# OTA e distribuzione dell'APK MiiClock

> [!abstract]
> Il sistema OTA aggiorna esclusivamente `local.miiclock.launcher`. GitHub Releases distribuisce gli asset ma non è la radice di fiducia: autenticità e integrità derivano dalla firma del manifest, dalla firma Android dell'APK e dai controlli locali prima dell'installazione.

## Separazione fra firmware e OTA

| Canale | Aggiorna | Richiede fastboot | Rollback principale |
| --- | --- | --- | --- |
| Firmware | `product`/`super` e AVB autorizzati | sì | immagini firmware verificate |
| OTA APK | solo MiiClock in `/data/app` | no | APK precedente verificato o copia `/product` |

Un update OTA non modifica `product`, `super`, `boot`, `vbmeta`, `userdata` o altre partizioni. Le autorizzazioni privilegiate vengono fornite dal firmware già installato: un APK OTA è compatibile solo nel range firmware dichiarato dal manifest.

## Formato di una GitHub Release

Una release distribuita contiene almeno:

```text
miiclock-launcher-X.Y.Z.apk
ota-manifest.json
ota-manifest.sig
```

Il manifest versionato include `schema_version`, `versionName`, `versionCode`, nome asset APK, dimensione, SHA-256, package atteso, intervallo firmware minimo/massimo e changelog. La firma detached del manifest usa la chiave privata conservata fuori dal device e dal repository; il Clock contiene soltanto la chiave pubblica necessaria alla verifica.

## Catena di fiducia

Un pacchetto è valido solo quando tutti questi controlli sono superati:

1. `ota-manifest.sig` verifica con la chiave pubblica MiiClock;
2. il manifest è ben formato e usa schema supportato;
3. package APK = `local.miiclock.launcher`;
4. la firma Android coincide con il certificato ufficiale MiiClock;
5. SHA-256 e dimensione coincidono con il manifest;
6. `versionCode` è superiore alla versione installata;
7. firmware corrente compreso nel range dichiarato;
8. spazio libero e stato locale sono adeguati.

GitHub è quindi un transport/provider. Una release GitHub non firmata correttamente è rifiutata anche se viene dal repository atteso.

## Stato macchina OTA

Lo stato persistente dell'update evita ambiguità dopo reboot o interruzioni. Gli stati sono:

- `idle` / nessun update in corso;
- `downloaded` / file temporaneo completo e validato;
- `installing` / installazione avviata;
- `pending_verification` / APK installato, health check richiesto;
- `successful` / versione e launcher sani;
- `rollback_required` / test post-installazione fallito.

Il download è atomico: prima file temporaneo, poi verifica completa, infine promozione a pacchetto installabile. File parziali, hash errati o firma errata non vengono mai proposti a Package Manager.

## Installazione e rollback

`UpdateManager` è separato dalla logica UI e può completare verifica/rollback anche se l'Activity launcher viene terminata. Prima dell'update conserva una copia verificata dell'APK attuale. L'installazione usa i privilegi root disponibili, ma solo per il package MiiClock.

Il health check successivo verifica package/versione, firma, launcher avviabile, ruolo HOME e assenza di crash/ANR bloccanti. Se fallisce, viene ripristinato l'APK precedente; la copia integrata in `/product` è il fallback estremo tramite rimozione dell'aggiornamento di sistema.

## Esperienza utente

Impostazioni → Aggiornamenti mostra versione installata, ultimo controllo, stato, disponibilità, changelog, avanzamento, errori comprensibili e diagnostica minima. Il controllo può avvenire al massimo una volta ogni 24 ore; download e installazione richiedono sempre conferma esplicita dell'utente nella RC1.

## Pipeline e pubblicazione manuale

Forgejo costruisce, firma e conserva bundle APK/OTA come artefatti. La pubblicazione su GitHub Release è deliberatamente manuale: evita che un errore CI, un tag o un mirror pubblichi automaticamente asset non approvati. Il repository Forgejo è la sorgente canonica del progetto; GitHub è il canale pubblico degli asset.

Per la pipeline, le chiavi vengono materializzate in memoria temporanea con permessi restrittivi e poi eliminate. Nessuna chiave privata deve finire in Git, APK, firmware, log o artifact pubblico. Vedere [[Documentazione/09-Ambienti-Toolchain-e-Operativita|ambienti e CI]].

## Collegamenti

[[Documentazione/01-Identita-Release-RC1|Release RC1]] · [[Documentazione/03-Architettura-Firmware-e-Build|Firmware]] · [[Documentazione/09-Ambienti-Toolchain-e-Operativita|Pipeline]] · [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Quality gate]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Sicurezza]].
