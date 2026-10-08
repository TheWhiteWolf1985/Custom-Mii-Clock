---
title: Indice della wiki MiiClock
aliases:
  - MiiClock Wiki
  - Indice progetto X04G
tags:
  - miiclock
  - x04g
  - wiki
  - rag
  - indice
status: canonico
versione_documentazione: 1.0.0-RC1
release_firmware: "0.1.16 (028)"
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: navigazione
---

# Indice della wiki MiiClock

> [!abstract]
> Questa cartella è la documentazione canonica di rilascio del progetto **MiiClock** per **Xiaomi Mi Smart Clock X04G**. È pensata sia per la lettura in Obsidian sia per un RAG: ogni documento è tematico, ha metadati espliciti, distingue fatti correnti, storico e piano futuro, e rimanda ai documenti collegati.

## Come leggere la wiki

- **Stato corrente** indica ciò che appartiene alla baseline RC1 o alla relativa pipeline verificata.
- **Storico** descrive decisioni e tentativi precedenti: è utile per non ripetere errori, ma non sostituisce la configurazione corrente.
- **Futuro** contiene soltanto lavoro non incluso nella release candidata. È raccolto principalmente in [[Documentazione/12-Implementazioni-Future|Implementazioni future]].
- Gli hash, gli audit dettagliati e le prove eseguibili restano in `AI/`; questa wiki ne riassume il significato operativo senza duplicare segreti, credenziali, seriali o backup personali.

## Mappa della documentazione

### Identità, hardware e firmware

1. [[Documentazione/01-Identita-Release-RC1|Identità della release RC1]] — modello supportato, versioni, artefatti e confini di compatibilità.
2. [[Documentazione/02-Hardware-Stock-e-Reverse-Engineering|Hardware, stock e reverse engineering]] — origine del lavoro, layout del device e conoscenze acquisite dal firmware originale.
3. [[Documentazione/03-Architettura-Firmware-e-Build|Architettura firmware e build]] — immagine `product`, AVB, integrazione del launcher e riproducibilità.
4. [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Flash, recovery e quality gate]] — procedura sicura, rollback e condizioni di stop.

### Launcher e funzioni utente

5. [[Documentazione/04-Launcher-e-Interfaccia|Launcher e interfaccia]] — Home, impostazioni, gesture, localizzazione e diagnostica.
6. [[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie|Calendario, meteo e sveglie]] — servizi locali, fonti dati e limiti di sincronizzazione.
7. [[Documentazione/06-Media-e-Assistente-MiiPulse|Media, Assistant e MiiPulse]] — controllo delle sessioni e overlay vocale.
8. [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant, WebView e MQTT]] — dashboard locale, tastiera e integrazione domotica.
9. [[Documentazione/08-OTA-e-Distribuzione-APK|OTA e distribuzione APK]] — catena di fiducia, aggiornamenti e rollback applicativo.

### Operatività e decisioni

10. [[Documentazione/09-Ambienti-Toolchain-e-Operativita|Ambienti, toolchain e operatività]] — PC, VM, runner, vault e pipeline.
11. [[Documentazione/11-Decisioni-Rischi-e-Limiti|Decisioni, rischi e limiti]] — ADR sintetici, posture di sicurezza e vincoli noti.
12. [[Documentazione/12-Implementazioni-Future|Implementazioni future]] — backlog deliberatamente separato dalla RC1.
13. [[Documentazione/99-Glossario|Glossario]] — termini e acronimi usati nella wiki.

## Fonti tecniche interne

La fonte verificabile di ogni asserzione rimane nel repository. In particolare:

- [[Documentazione/13-Fonti-e-Prove-RC1|Audit di readiness RC1]] per stato, controlli e artefatti candidati;
- [[Documentazione/13-Fonti-e-Prove-RC1|PRD del progetto]] per requisiti e vincoli;
- [[Documentazione/13-Fonti-e-Prove-RC1|Runbook]] per comandi e contesto operativo;
- [[Documentazione/13-Fonti-e-Prove-RC1|Decisioni tecniche]] per la motivazione storica;
- [[Documentazione/13-Fonti-e-Prove-RC1|Audit funzionale launcher]] per la copertura delle viste.

## Regole per aggiornare questa wiki

1. Aggiornare prima il documento tematico e poi l'indice se cambiano navigazione o stato.
2. Non inserire password, token, chiavi private, indirizzi personali, seriali, MAC o backup `userdata`.
3. Dichiarare espressamente se un risultato è **host/CI**, **firmware costruito**, **flashato**, oppure **accettato fisicamente**.
4. Per una funzione futura, aggiornare [[Documentazione/12-Implementazioni-Future|Implementazioni future]] invece di farla apparire come parte della RC1.
5. Mantenere i collegamenti in stile Obsidian quando si rinominano file.

## Stato del perimetro

La documentazione descrive solo **X04G / `mico_x04g`**. Non è una guida per LX04, altri smart display Xiaomi, GSI o LineageOS. Le immagini e le procedure sono progettate per preservare dati e provisioning quando il relativo gate lo consente; un factory reset è sempre un'operazione distinta e distruttiva.

Collegamenti principali: [[Documentazione/01-Identita-Release-RC1|Release RC1]] · [[Documentazione/03-Architettura-Firmware-e-Build|Firmware]] · [[Documentazione/04-Launcher-e-Interfaccia|Launcher]] · [[Documentazione/12-Implementazioni-Future|Roadmap]].
