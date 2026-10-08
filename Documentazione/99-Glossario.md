---
title: Glossario MiiClock
aliases:
  - Glossario X04G
tags:
  - miiclock
  - glossary
  - rag
  - x04g
status: canonico
versione_documentazione: 1.0.0-RC1
last_reviewed: 2026-10-08
tipo: riferimento
---

# Glossario MiiClock

> [!abstract]
> Vocabolario controllato della wiki. Usare questi termini in modo coerente migliora sia la lettura in Obsidian sia il recupero semantico nel RAG.

| Termine | Significato nel progetto |
| --- | --- |
| **ADB** | Android Debug Bridge. Nel firmware MiiClock è disponibile via USB con root; ADB TCP non è abilitato. |
| **AppOp** | Autorizzazione operativa Android oltre i normali permessi. È usata, per esempio, per `SYSTEM_ALERT_WINDOW`. |
| **AVB** | Android Verified Boot: catena di firma/verifica delle immagini Android. Non viene bypassata. |
| **BROM** | Modalità Boot ROM MediaTek, impiegata come recupero di ultima istanza con MTKClient. |
| **Cast state** | Stato della configurazione locale Google/Cast usato per non attivare MiiClock prima di Setup Wizard. |
| **CI** | Continuous Integration. Nel progetto esegue build e validazioni; non pubblica automaticamente release. |
| **ext4** | Filesystem usato da `product`; i blocchi liberi sono un gate di build. |
| **Fastboot** | Protocollo usato per il flash controllato. Per X04G si entra in modo affidabile da Recovery → Reboot to bootloader. |
| **Firmware** | Insieme di immagini Android firmate, in particolare `super` e metadati AVB. |
| **Google Launcher** | `com.google.assistant.launcher`, UI stock rimossa dal firmware MiiClock per recuperare spazio. |
| **Home Assistant** | Piattaforma domotica integrata tramite WebView controllata e MQTT Discovery. |
| **IME** | Input Method Editor. MiiClock include una tastiera touch per i campi WebView e configurazione. |
| **LUKS vault** | Volume cifrato che conserva backup fisico, chiavi private AVB e materiale di rollback. |
| **MediaShell** | Componente Google stock per superfici media/Cast, preservato nel firmware. |
| **MediaSession** | API Android per leggere e controllare una sessione multimediale attiva. |
| **MiiPulse** | Overlay vocale MiiClock guidato da eventi Binder dell'Assistant Core; non registra audio o trascrizioni. |
| **MQTT Discovery** | Pubblicazione di configurazioni MQTT che permette a Home Assistant di creare automaticamente entità. |
| **OTA** | Aggiornamento Over The Air dell'APK, mai delle partizioni firmware in questa linea. |
| **PSS** | Proportional Set Size, misura della memoria attribuita a un processo Android. |
| **RAG** | Retrieval-Augmented Generation: il sistema che indicizzerà questi documenti per rispondere con contesto. |
| **RC1** | Release Candidate 1: baseline candidata al test distribuito, non ancora garanzia universale. |
| **super** | Partizione dinamica che contiene `system`, `vendor` e `product`. |
| **X04G / mico_x04g** | Modello e product identifier fastboot supportati dal progetto. |

Navigazione: [[Documentazione/00-Indice-Wiki|Indice wiki]] · [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Flash e recovery]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Decisioni e limiti]].
