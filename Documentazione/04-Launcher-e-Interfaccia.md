---
title: Launcher MiiClock e interfaccia utente
aliases:
  - Interfaccia MiiClock
  - Launcher X04G
tags:
  - miiclock
  - launcher
  - ui
  - home
  - settings
  - i18n
status: canonico
versione_documentazione: 1.0.0-RC1
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: applicazione
---

# Launcher MiiClock e interfaccia utente

> [!abstract]
> MiiClock è il launcher HOME per X04G, implementato in Java su Android 10/API 29 e progettato per un display 800 × 480 low-RAM. Fornisce una UI autonoma senza eliminare i servizi Google che il dispositivo continua a usare.

## Identità applicativa

| Voce | Valore |
| --- | --- |
| Package | `local.miiclock.launcher` |
| Release RC1 | `0.25.2`, `versionCode 51` |
| Linguaggio e API | Java, Android API 29 |
| Posizione firmware | `product/priv-app/MiiClockLauncherV028` |
| Posizione update OTA | `/data/app` |
| Canvas logico | 800 × 480 px |
| Dipendenze | API AOSP; nessuna dipendenza AndroidX obbligatoria |

## Architettura UI

La Home è disegnata su canvas logico e composta da elementi riutilizzabili: `GlobalHeader`, `HomeScreen`, `WeatherSummary`, `AppDock`, `ScreenRegistry` e `GestureController`. L'uso di componenti separati evita che una nuova vista modifichi accidentalmente Home, gesture o header globali.

Le schermate registrate sono:

| ID stabile | Stato RC1 | Scopo |
| --- | --- | --- |
| `home` | attiva | orario, data, meteo e dock principale |
| `weather` | attiva | previsioni MET Norway e impostazioni meteo |
| `calendar` | attiva | calendario locale AOSP |
| `alarms` | attiva | sveglie autonome CRUD |
| `home_assistant` | attiva | dashboard WebView controllata |
| `media` | attiva | controllo della MediaSession corrente |
| `app_drawer` | attiva | applicazioni disponibili e fallback sicuri |

Lo stato delle schermate è configurabile da **Gestione schermate**: ordine, abilitazione, Home predefinita e timeout. È persistito atomicamente in JSON interno; la configurazione viene migrata per versione e non può lasciare come Home una schermata non disponibile.

## Gesture e navigazione

- carosello circolare sinistra/destra, con soglia iniziale di 120 px e snap animato;
- swipe dall'alto nei primi 24 px: [[Documentazione/04-Launcher-e-Interfaccia#pannello-impostazioni-rapide|pannello impostazioni rapide]];
- long press di 500 ms per le azioni contestuali previste;
- swipe dal basso e doppio tap non hanno azioni MiiClock dedicate;
- dopo 60 secondi di inattività su una schermata interna, ritorno alla Home configurata;
- la vista non intercetta input al di fuori delle proprie regioni e non sostituisce gesture indispensabili del sistema.

## Home e dock

La Home combina orario centrale, data italiana/localizzata, riepilogo meteo e sfondo. Le dimensioni dell'orologio sono intenzionalmente fisse: su un pannello 800 × 480 la ridimensionabilità renderebbe instabile la composizione. Il dock porta a Home Assistant, Sveglie, Impostazioni e Calendario; non conserva più collegamenti a YouTube o Chrome.

Lo sfondo può essere caricato dalla rete locale attraverso il flusso Aspetto e viene validato/normalizzato atomicamente. Non sono previsti temi completi: lo sfondo è la personalizzazione visiva della V1. Il formato 12/24 ore appartiene alle impostazioni Data e ora, non ad Aspetto.

## Impostazioni

Le impostazioni usano una struttura Android-like a tile verticali. Le sezioni attive includono:

- **Gestione schermate** — Home predefinita, ordine, attivazione e timeout;
- **Data e ora** — data, ora, fuso orario e toggle automatico;
- **Lingua** — Italiano, Inglese, Tedesco, Spagnolo e Francese;
- **Aspetto** — sfondo e ripristino dei valori dell'app;
- **Calendario**, **Meteo**, **Home Assistant** e **MQTT** — configurazioni specifiche;
- **Aggiornamenti** — stato OTA e azioni esplicite;
- **Memoria**, **Informazioni**, **Diagnostica** e **Recovery** — stato reale e ripristino controllato.

La voce di recovery apre MiiClock, resetta in modo atomico la configurazione o porta a Impostazioni Android; non tenta di richiamare il Google Launcher rimosso.

## Pannello impostazioni rapide

Lo swipe dall'alto mostra un pannello verticale Android-like che si estende verso il basso. Riutilizza le capacità native dove presenti:

- luminosità automatica/manuale;
- volume media, coerente con i tasti fisici;
- Wi-Fi;
- microfono, coerente con il tasto hardware;
- riavvio, sempre con conferma.

Non sono simulazioni visive: i valori provengono dalle API o dai bridge disponibili sul device. Le modifiche ad alto impatto richiedono conferma e nessun controllo genera polling permanente.

## Localizzazione e accessibilità tecnica

Le stringhe non sono hard-coded nelle viste: i cataloghi JSON runtime usano chiave → traduzione, con italiano come fallback e cinque lingue disponibili. Il validatore verifica completezza delle chiavi e fallback. Per aggiungere una lingua si aggiunge un catalogo conforme, senza riscrivere il codice UI.

## Diagnostica e memoria

La diagnostica mostra dati reali, non segnaposto: versione app/firmware, configurazione, stato ADB/root, componenti Google preservati, Assistant/MiiPulse, stato OTA e log sintetici. La pagina Memoria espone RAM totale/disponibile, PSS di MiiClock e Assistant, pressione memoria/LMK e heap limit. È utile sia per RC1 sia per identificare regressioni su un sistema low-RAM.

## Attivazione HOME post-provisioning

MiiClock diventa HOME solo dopo `device_provisioned=1`, `user_setup_complete=1` e stato Cast/Google Home coerente. `HomeActivationJobService` e il bootstrap firmware applicano questa regola per non coprire Setup Wizard dopo un factory reset. Vedere [[Documentazione/03-Architettura-Firmware-e-Build|firmware]] e [[Documentazione/10-Flash-Recovery-e-Quality-Gates|test da zero]].

## Collegamenti

[[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie|Servizi locali]] · [[Documentazione/06-Media-e-Assistente-MiiPulse|Media e Assistant]] · [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant]] · [[Documentazione/08-OTA-e-Distribuzione-APK|OTA]] · [[Documentazione/12-Implementazioni-Future|Evoluzioni UI]].
