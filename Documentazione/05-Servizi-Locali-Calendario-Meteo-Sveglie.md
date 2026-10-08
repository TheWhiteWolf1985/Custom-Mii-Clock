---
title: Servizi locali: calendario, meteo e sveglie
aliases:
  - Funzioni locali MiiClock
  - Calendario Meteo Sveglie
tags:
  - miiclock
  - calendar
  - weather
  - alarms
  - met-norway
  - offline-first
status: canonico
versione_documentazione: 1.0.0-RC1
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: funzionalita
---

# Servizi locali: calendario, meteo e sveglie

> [!abstract]
> Calendario, meteo e sveglie sono funzioni MiiClock indipendenti dalle card del vecchio Google Launcher. Il calendario e le sveglie mantengono dati locali; il meteo usa una fonte pubblica con cache e configurazione esplicita, senza fingere dati dimostrativi.

## Calendario MiiClock

### Modello dati

Il calendario usa il `CalendarProvider` AOSP già presente sul device. MiiClock crea e gestisce il calendario locale **MiiClock**, con account type `LOCAL`. Gli eventi sono quindi leggibili e modificabili attraverso le API Android standard, ma non vengono automaticamente sincronizzati con Google Calendar o un account cloud.

Questo confine è intenzionale:

- mantiene CRUD reale e persistente senza dipendere da componenti Google privati;
- evita di promettere sincronizzazione che non è stata progettata o verificata;
- permette una futura integrazione di sync come progetto separato, senza cambiare la UI locale.

### Funzioni

- visualizzazione calendario e navigazione tra giorni/mesi;
- creazione, modifica e cancellazione di eventi locali;
- apertura della vista Calendario dal dock e dalle impostazioni;
- impostazioni calendario non legate a eventi cloud;
- permessi `READ_CALENDAR` e `WRITE_CALENDAR` richiesti nel contesto della funzione.

Un'eventuale richiesta di autorizzazione Android alla prima esecuzione è normale: non deve essere confusa con Setup Wizard o con una regressione del launcher. Vedere [[Documentazione/04-Launcher-e-Interfaccia|interfaccia e attivazione HOME]].

### Ciò che non fa

La RC1 non sincronizza eventi con Google, CalDAV, Home Assistant o server privati. Non esegue polling continuo, non memorizza token cloud e non inventa un backend parallelo.

## Meteo

### Fonte e privacy

Le previsioni provengono da **MET Norway Locationforecast 2.0**. La ricerca località usa geocoding Open-Meteo per paese/città/CAP; il risultato finale è memorizzato come località e coordinate scelte dall'utente. L'app non usa GPS né richiede permessi di posizione.

La configurazione Meteo include:

- ricerca città/paese/CAP e scelta del risultato;
- inserimento manuale di latitudine e longitudine come fallback;
- unità, aggiornamento e informazione sull'ultimo dato;
- visualizzazione stale quando l'ultimo dato è ancora disponibile ma non aggiornabile.

### Comportamento della UI

La Home mostra un riepilogo prudente. Se non esiste una previsione valida appare l'icona generica con `--°`, mai una temperatura fittizia. La vista Meteo presenta le condizioni correnti e una previsione settimanale a cinque giorni; il mockup orario iniziale è stato deliberatamente corretto secondo le specifiche.

### Vincoli di rete

Il meteo richiede rete per una nuova ricerca o un aggiornamento. In assenza di Internet, mostra cache/stato stale o indisponibile; non blocca Home, sveglie o altri servizi. I dati sono soggetti alle condizioni e all'identificazione applicativa richieste dal provider: la pipeline e il codice devono continuare a rispettarle.

## Sveglie autonome

### Perché sono autonome

Il backend delle sveglie Google sul Clock non offre un contratto pubblico affidabile. MiiClock quindi non tenta di scrivere dati privati dell'Assistant: implementa un motore proprio con persistenza locale, scheduling e UI CRUD.

### Funzioni incluse

- creare, rinominare, modificare, attivare/disattivare e cancellare sveglie;
- giorni di ripetizione e pianificazione affidabile;
- suoneria, snooze e stop;
- persistenza a riavvio tramite ricevitori e servizi previsti;
- integrazione con la schermata Allarmi e con la Home;
- sospensione sicura del MiiPulse quando una sveglia MiiClock sta suonando.

La richiesta `SET_ALARM` serve al modello Android; la logica effettiva di MiiClock conserva e governa le proprie entità. Le sveglie vocali create da Google Assistant restano un dominio distinto: possono coesistere, ma non sono automaticamente sincronizzate con le sveglie MiiClock.

## Stati reali, cache e persistenza

| Funzione | Persistenza | Rete obbligatoria | Sincronizzazione esterna |
| --- | --- | --- | --- |
| Calendario | CalendarProvider locale | no | no |
| Meteo | configurazione e cache interne | solo aggiornamento/ricerca | fonte MET Norway, nessun account |
| Sveglie | storage MiiClock locale | no | no |

## Quality gate funzionali

- test host per formati data/ora, mapping meteo, cache/stale e CRUD;
- controlli touch per calendario, meteo e allarmi;
- test riavvio per calendario e sveglie;
- verifica che un errore di rete meteo non causi crash/retry aggressivi;
- screenshot e, quando previsto dal gate fisico, acquisizione webcam e logcat limitato.

## Collegamenti

[[Documentazione/04-Launcher-e-Interfaccia|Launcher]] · [[Documentazione/06-Media-e-Assistente-MiiPulse|Media e Assistant]] · [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant e MQTT]] · [[Documentazione/12-Implementazioni-Future|Futuro sync calendario]].
