---
title: Media, Google Assistant e overlay MiiPulse
aliases:
  - MiiPulse
  - Media MiiClock
  - Assistant Overlay
tags:
  - miiclock
  - media
  - google-assistant
  - miipulse
  - mediasession
  - overlay
status: canonico
versione_documentazione: 1.0.0-RC1
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: funzionalita
---

# Media, Google Assistant e overlay MiiPulse

> [!abstract]
> MiiClock non sostituisce Google Assistant o un player audio: osserva e controlla in modo autorizzato la MediaSession attiva e aggiunge MiiPulse, un overlay puramente visivo alimentato da eventi dell'Assistant Core. Nessun audio, trascrizione o comando vocale viene intercettato da MiiClock.

## Schermata Media

### Modello scelto

La schermata Media usa `MediaSessionManager` e la capability privilegiata `MEDIA_CONTENT_CONTROL`. Questa scelta è stata preferita a `NotificationListenerService`, risultato poco affidabile su Android 10 low-RAM. Il permesso viene concesso solo alla copia in `product/priv-app`; un semplice APK OTA non può ottenere privilegi nuovi.

### Cosa mostra e controlla

- sessione media attiva, titolo, artista, artwork e stato;
- selezione della sessione quando Android ne espone più di una;
- play, pausa, precedente, successivo e seek solo se la sessione dichiara il comando disponibile;
- avanzamento e stato reale della riproduzione;
- compatibilità generica MediaSession, verificata progressivamente con servizi quali YouTube Music e MediaShell.

La UI non implementa una coda proprietaria, un catalogo musicale, un selettore device o uno slider volume aggiuntivo. Il volume resta coerente con Quick Settings e tasti fisici. Non esiste un player MiiClock autonomo: per riprodurre musica è necessario avviare un provider o chiedere ad Assistant; la vista MiiClock ne espone la sessione quando disponibile.

### Superficie nera MediaShell

Quando una riproduzione comandata da Assistant/YouTube Music apriva una superficie stock nera, `MediaForegroundGuardService` è stato introdotto per nascondere soltanto quella superficie visiva quando la media è attiva. Non modifica la sessione, il Cast o l'audio; l'obiettivo è lasciare visibile MiiClock e la sua vista Media.

## Google Assistant dopo la rimozione del launcher

Il Google Launcher è stato rimosso, ma `com.google.assistant.core` e MediaShell restano nel firmware. Assistant continua quindi a gestire hotword, conversazione, timer/sveglie vocali, riproduzione e Google Home. MiiClock diventa HOME solo dopo provisioning; non è un sostituto del motore Assistant.

La compatibilità è valutata per versione e hash dell'Assistant Core conosciuto. Se la versione non è quella attesa, le integrazioni private fail-closed: Assistant continua a funzionare normalmente, ma MiiClock non tenta overlay o decoding non sicuri.

## MiiPulse

### Obiettivo

MiiPulse rende visibile una conversazione Assistant senza ripristinare l'overlay Google. È un overlay trasparente, non interattivo e non focalizzabile, quindi non ridimensiona né deforma l'Activity sottostante e non intercetta touch, swipe o tastiera.

### Flusso tecnico

```text
AssistantCoreService Binder privato
        ↓ eventi strettamente minimi
AssistantEventBridge
        ↓ coda limitata + decoder protobuf minimale
state machine MiiPulse
        ↓ main thread, massimo 30 fps
AssistantOverlayService / TYPE_APPLICATION_OVERLAY
```

Il bridge usa un contratto interoperabile minimale e ignora campi sconosciuti, payload malformati, troncati o troppo grandi. I dati vocali non vengono memorizzati né interpretati: interessano solo stato conversazionale e livello vocale numerico.

### Stati e sicurezza

Gli stati ammessi sono `IDLE`, `LISTENING`, `START_OF_SPEECH`, `END_OF_SPEECH`, `THINKING`, `EXECUTING`, `TTS_PLAYING` ed `ERROR`. Time-out di sicurezza nascondono l'overlay dopo inattività, disconnessione Binder, schermo spento o suoneria di una sveglia MiiClock. La reconnessione usa backoff, non polling continuo.

Il permesso overlay viene concesso a MiiClock tramite AppOp; Assistant Core resta esplicitamente su `ignore`. Qualunque incompatibilità, launcher Google presente, provisioning incompleto o permesso negato disattiva MiiPulse senza crash.

### Aspetto attuale

La versione consolidata usa un'onda simmetrica ai lati del centro, con 24 barre per lato, gradiente turchese/blu/viola e spazio libero per orologio e contenuto principale. L'intensità risponde al livello vocale reale con normalizzazione su finestra mobile e smoothing. L'overlay scompare progressivamente al ritorno a `IDLE`.

Il renderer è isolato in `AssistantPulseView` e `AssistantPulseRenderer`: è quindi possibile sostituire grafica e animazioni senza riscrivere bridge, decoder o state machine. Le eventuali evoluzioni grafiche sono in [[Documentazione/12-Implementazioni-Future|Implementazioni future]].

## Diagnostica e accettazione

Diagnostica espone compatibilità Assistant Core, stato Binder, AppOp overlay, ultimo stato/età ed errore sintetico senza payload vocali. I test includono chiamate normali, annullamento, microfono disattivato, musica, Home Assistant aperto, nessun falso overlay in inattività, assenza di interferenze touch e limiti PSS/CPU.

## Collegamenti

[[Documentazione/03-Architettura-Firmware-e-Build|Firmware e privilegi]] · [[Documentazione/04-Launcher-e-Interfaccia|Launcher]] · [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Limiti e decisioni]].
