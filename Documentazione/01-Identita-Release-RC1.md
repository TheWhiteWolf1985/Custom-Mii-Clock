---
title: Identità della release MiiClock 1.0.0-RC1
aliases:
  - Release RC1
  - MiiClock 1.0.0 RC1
tags:
  - miiclock
  - x04g
  - rc1
  - release
  - firmware
  - launcher
status: canonico
versione_documentazione: 1.0.0-RC1
release_firmware: "0.1.16 (028)"
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: release
---

# Identità della release MiiClock 1.0.0-RC1

> [!abstract]
> MiiClock 1.0.0-RC1 è la release candidata del firmware ricostruito e del launcher personalizzato per il solo Xiaomi Mi Smart Clock X04G. Sostituisce l'interfaccia Google Clock con MiiClock mantenendo i componenti Google necessari per Assistant, Google Home e MediaShell.

## Scheda di rilascio

| Campo | Valore |
| --- | --- |
| Nome pubblico | `MiiClock 1.0.0-RC1` |
| Modello supportato | Xiaomi Mi Smart Clock **X04G** |
| Codice fastboot richiesto | `mico_x04g` |
| Firmware | `0.1.16`, build/label `028` |
| Launcher | `0.25.2`, `versionCode 51` |
| Package launcher | `local.miiclock.launcher` |
| Android base | Android 10, API 29, low-RAM |
| Display target | 800 × 480 px, 240 dpi |
| Architettura | MediaTek MT8167, ARM32 |
| Partizioni normali da aggiornare | `vbmeta_system`, `super`, `vbmeta` per ultima |

L'APK canonico della RC1 ha SHA-256 `a64c76571a9e3aeadd4870a7f60a5829edd14efb3adcae9d75d1e891279c2562`. Il certificato Android atteso ha fingerprint SHA-256 `7340f6343323e0474c4b6afc0e5891aa9e7f5d5176606be82b6124a39631c029`.

## Cosa compone la RC1

- [[Documentazione/03-Architettura-Firmware-e-Build|Firmware 028]] con MiiClock integrato come applicazione privilegiata in `product`, policy AVB valida e immagini riproducibili.
- [[Documentazione/04-Launcher-e-Interfaccia|Launcher 0.25.2]]: Home, gesture, App Drawer, impostazioni, Recovery e diagnostica.
- [[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie|Servizi locali]]: calendario AOSP locale, meteo MET Norway e motore sveglie indipendente.
- [[Documentazione/06-Media-e-Assistente-MiiPulse|Media e Assistant]]: controllo MediaSession e overlay vocale MiiPulse.
- [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant]]: dashboard WebView, tastiera touch, MQTT Discovery e comandi con guardrail.
- [[Documentazione/08-OTA-e-Distribuzione-APK|OTA APK]]: aggiornamenti applicativi firmati, senza aggiornare partizioni firmware.

## Stato e terminologia corretta

La RC1 identifica una **baseline pronta per il collaudo distribuito**, non una garanzia universale per ogni esemplare X04G. Ogni artefatto deve essere classificato da un audit come:

1. **costruito e verificato offline** — checksum, AVB, filesystem, due build e test host superati;
2. **idoneo al flash** — anche rollback, manifest, capacità del device e preflight sono verdi;
3. **flashato** — installato su un esemplare fisico, senza dedurre automaticamente l'accettazione;
4. **accettato fisicamente** — avvio, ADB, UI e funzioni target verificati sul device.

Il riferimento puntuale di questi stati è [[Documentazione/13-Fonti-e-Prove-RC1|l'audit RC1]], non il solo nome della release.

## Compatibilità e non compatibilità

### Inclusa

- X04G con prodotto fastboot `mico_x04g` verificato per tre volte consecutive.
- Firmware derivato dalla baseline e dalle chiavi AVB del progetto.
- Aggiornamento del solo APK tramite OTA quando il firmware rientra nell'intervallo dichiarato dal manifest.

### Esclusa

- LX04 e altri modelli Xiaomi, anche se esteriormente simili.
- Dump della community come immagine da flashare direttamente.
- GSI, LineageOS e modifiche non appartenenti alla catena AVB del progetto.
- Downgrade APK, immagini AVB con verificazione disabilitata e flash che includono `userdata` o `metadata`.

## Relazione fra firmware e APK

L'APK integrato in `product` e l'APK distribuito via OTA devono essere **lo stesso payload firmato byte-identico**. Cambia la modalità di installazione:

- il firmware rende disponibile la copia di base e le autorizzazioni privilegiate;
- OTA aggiorna solo `/data/app`, verificando manifest firmato, certificato Android, hash, dimensione, versione e compatibilità firmware;
- rimuovere un aggiornamento di sistema ripristina la copia in `product` senza riflashare.

Vedere [[Documentazione/03-Architettura-Firmware-e-Build|architettura firmware]] e [[Documentazione/08-OTA-e-Distribuzione-APK|OTA]] per i dettagli.

## Collegamenti

[[Documentazione/00-Indice-Wiki|Indice wiki]] · [[Documentazione/02-Hardware-Stock-e-Reverse-Engineering|Origine stock]] · [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Gate di flash]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Rischi e limiti]].
