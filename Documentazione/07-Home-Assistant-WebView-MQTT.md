---
title: Integrazione Home Assistant, WebView e MQTT
aliases:
  - Home Assistant MiiClock
  - MQTT MiiClock
  - WebView Home Assistant
tags:
  - miiclock
  - home-assistant
  - mqtt
  - webview
  - keyboard
  - privacy
status: canonico
versione_documentazione: 1.0.0-RC1
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: integrazione
---

# Integrazione Home Assistant, WebView e MQTT

> [!abstract]
> MiiClock può visualizzare una dashboard Home Assistant scelta dall'utente e integrarsi con Home Assistant tramite MQTT Discovery. Le due strade sono complementari: WebView è la UI remota, MQTT espone stato e comandi locali con una policy di sicurezza esplicita.

## Dashboard Home Assistant via WebView

### Obiettivo

La voce Home Assistant apre una dashboard specifica senza dipendere da Fully Kiosk o da licenze proprietarie. La WebView gira nel processo isolato `:homeassistant`, riducendo l'impatto di crash e memoria sulla UI del launcher.

### Configurazione utente

L'utente apre Impostazioni → Home Assistant e inserisce l'URL della propria dashboard. Il flusso di autenticazione viene eseguito dalla pagina Home Assistant nella WebView; MiiClock non salva username, password, token o cookie nella propria configurazione, nei log o nel repository.

Per l'uso normale:

1. collegare il Clock alla rete Wi-Fi;
2. configurare l'URL Home Assistant nel menu MiiClock;
3. aprire la vista Home Assistant;
4. eseguire il login nella pagina, usando la [[Documentazione/07-Home-Assistant-WebView-MQTT#tastiera-touch|tastiera touch]] quando necessaria;
5. completata la sessione, la dashboard è riapribile fino a logout o pulizia dei dati WebView.

### Policy WebView

- accetta solo URL `http`/`https` validi;
- HTTP è ammesso soltanto per loopback, rete privata LAN o hostname `.local`; HTTPS è preferibile ovunque disponibile;
- l'origine resta bloccata dopo la configurazione; redirect e navigazioni non autorizzati sono rifiutati;
- nessun bridge JavaScript nativo esposto alla dashboard;
- nessun accesso a file, content provider, mixed content o impostazioni Web permissive;
- cookie e sessione appartengono al profilo WebView Android e possono essere cancellati dall'utente.

## Tastiera touch

MiiClock include un Input Method Editor proprio per permettere digitazione diretta sul touch, in particolare nei campi WebView e configurazione. Non conserva cronologia, suggerimenti o predizioni. Il bootstrap la rende disponibile dopo provisioning, senza sostituire in modo aggressivo un'eventuale IME di terze parti scelta dall'utente.

## MQTT Discovery

### Ruolo

Il servizio MQTT pubblica configurazioni Discovery e stati in modo che Home Assistant possa creare automaticamente il dispositivo MiiClock e le sue entità. Broker, credenziali e nome device vengono configurati localmente dall'utente; non sono parte dell'APK, del firmware, dei manifest di release o della wiki.

### Dati esposti

Il modello include telemetria e comandi esplicitamente approvati per:

- disponibilità del Clock e informazioni firmware/launcher;
- stato locale del display, luminosità e volume;
- stato e CRUD delle sveglie MiiClock;
- stato e controlli sicuri della sessione Media compatibile;
- meteo e stato applicativo compatibili con l'integrazione;
- cambio dello sfondo tramite URL remoto validato.

L'entità background accetta solo un URL strettamente validato: niente credenziali incluse, loopback, redirect o download illimitati; dimensione massima 8 MiB, normalizzazione e sostituzione atomica. Un download errato non cambia lo sfondo attivo.

### Comandi volutamente esclusi

Per ridurre l'impatto di un broker compromesso o di messaggi replay, MQTT non controlla Wi-Fi, microfono, ADB, root, AVB, OTA o calendario. Il riavvio non è un singolo comando: richiede handshake `PREPARE` + `CONFIRM`, con scadenza e protezioni di timestamp/replay. I messaggi retained e le azioni distruttive sono trattati con policy restrittiva.

## Operatività e diagnostica

In assenza di broker o rete, launcher, sveglie e UI locale continuano a funzionare. Il servizio usa riconnessione con backoff e stato persistente; non deve creare loop di connessione su un device low-RAM. Diagnostica riporta stato MQTT, ultimo errore sintetico e configurazione non sensibile.

Il test di accettazione Home Assistant prevede Discovery effettiva, lettura stati, comandi consentiti, rifiuto dei comandi non consentiti, reboot handshake e download background fallito/superato, senza esporre credenziali nei report.

## Collegamenti

[[Documentazione/04-Launcher-e-Interfaccia|Launcher]] · [[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie|Meteo e sveglie]] · [[Documentazione/06-Media-e-Assistente-MiiPulse|Media]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Threat model]] · [[Documentazione/12-Implementazioni-Future|Evoluzioni Home Assistant]].
