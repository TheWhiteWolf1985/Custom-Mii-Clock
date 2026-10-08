---
title: Implementazioni future MiiClock
aliases:
  - Roadmap MiiClock
  - Backlog post RC1
tags:
  - miiclock
  - roadmap
  - future
  - backlog
  - post-rc1
status: pianificato
versione_documentazione: 1.0.0-RC1
last_reviewed: 2026-10-08
tipo: roadmap
---

# Implementazioni future MiiClock

> [!abstract]
> Roadmap separata dalla RC1: raccoglie direzioni tecniche che richiedono un progetto, un audit e un gate di accettazione propri prima di essere dichiarate disponibili.

> [!important]
> Questo è l'unico documento di roadmap deliberatamente separato dalla documentazione della RC1. Le voci qui sotto **non sono funzioni promesse o validate** nella release candidata. Ogni implementazione dovrà avere requisiti, branch, audit, test e piano di rollback propri.

## Principio di priorità

Prima di aprire nuove funzioni, completare il ciclo RC1: test distribuiti, raccolta regressioni, triage, stabilità del firmware 028 e procedura di release pubblica. Una feature non deve rientrare in una patch RC solo perché è già stata discussa.

## Priorità alta dopo RC1

### Sincronizzazione calendario opzionale

**Obiettivo:** permettere la sincronizzazione con un'origine esterna senza perdere il calendario locale MiiClock.

**Direzione da valutare:** CalDAV/autenticazione esplicita o integrazione Google tramite API pubbliche e consenso utente. Non leggere né scrivere database privati Assistant/Google.

**Gate richiesti:** modello conflitti, supporto offline, protezione token, cancellazione account, consumo rete, migrazione dei dati locali, test su riavvio e fallback al calendario locale.

Collegamento: [[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie#calendario-miiclock|Calendario locale RC1]].

### Media: maturazione per provider reali

**Obiettivo:** consolidare l'interoperabilità MediaSession con YouTube Music, Spotify e MediaShell senza trasformare MiiClock in un player proprietario.

**Da verificare:** artwork, metadati incompleti, comandi dichiarati/non dichiarati, Cast, più sessioni, resume dopo reboot e gestione quando MediaShell porta in foreground superfici inattese.

**Fuori scope iniziale:** coda, preferiti, selettore output, volume indipendente, download/offline e login in provider.

Collegamento: [[Documentazione/06-Media-e-Assistente-MiiPulse#schermata-media|Schermata Media]].

### Esperienza MiiPulse e accessibilità visiva

**Obiettivo:** evolvere l'aspetto di MiiPulse mantenendo invariato il bridge Assistant e tutte le garanzie di overlay non touchabile.

**Possibili miglioramenti:** profili grafici, contrasto/accessibilità, calibrazione ampiezza, animazioni meno minimali, controllo utente e telemetria locale non sensibile delle prestazioni.

**Vincoli:** niente registrazione audio, trascrizioni, wake lock, polling aggressivo o overlay sopra Setup Wizard/sveglie. Qualunque modifica deve restare a 30 fps massimi e rispettare i budget PSS/CPU.

Collegamento: [[Documentazione/06-Media-e-Assistente-MiiPulse#mii-pulse|MiiPulse RC1]].

## Priorità media

### Gestione background più comoda

**Obiettivo:** semplificare il caricamento di immagini senza indebolire il modello LAN-first.

**Ipotesi:** pagina locale temporanea autenticata, integrazione Samba limitata o import da un supporto controllato. È necessario decidere prima il threat model: Samba può essere comodo, ma aumenta superficie di rete, gestione utenti e problemi di compatibilità.

**Gate richiesti:** dimensioni/formati, normalizzazione, quota disco, cancellazione sicura, fallback allo sfondo stock e assenza di servizio sempre attivo quando non serve.

### Meteo: qualità della scelta località

**Obiettivo:** rendere più semplice impostare località senza costringere l'utente a conoscere latitudine/longitudine.

**Direzione:** migliorare ranking e disambiguazione di città, paese e CAP usando geocoding pubblico, conservando coordinate come dato finale esplicito. Nessun GPS automatico e nessuna posizione fittizia.

### Orologio e UI adattiva controllata

**Obiettivo:** migliorare leggibilità, contrasto e layout con sfondi molto chiari/scuri.

**Vincolo:** la dimensione orologio rimane fissa finché non esiste un sistema di layout completo. Cambiare soltanto il font size creerebbe sovrapposizioni sul display 800 × 480.

### Lingue e revisione contenuti

**Obiettivo:** ampliare i cataloghi JSON oltre Italiano, Inglese, Tedesco, Spagnolo e Francese.

**Gate richiesti:** stessa keyset, validatore i18n, fallback italiano, verifica screenshot per stringhe lunghe e nessuna traduzione di nomi tecnici non localizzabili.

## Priorità infrastrutturale

### Firmware release pipeline pubblica

**Obiettivo:** formalizzare la pubblicazione manuale degli asset firmware e APK a partire dagli artifact Forgejo validati.

**Da mantenere:** GitHub è distribuzione, Forgejo è sorgente; nessun push automatico di release; manifest firmato per OTA; checksum e audit per firmware; firma/chiavi fuori da repository e artifact pubblici.

### Rotazione chiavi e risposta a incidente

**Obiettivo:** definire una procedura per compromissione del certificato APK o della chiave di firma OTA/AVB.

**Gate richiesti:** inventario delle chiavi pubbliche, piano aggiornamento fiducia, blocco downgrade, revoca, nuova baseline e comunicazione compatibilità.

### Test RC automatizzati e feedback utenti

**Obiettivo:** trasformare il test distribuito in regressioni ripetibili senza raccogliere dati personali inutili.

**Direzione:** checklist in-app/esportabile, raccolta log volontaria redatta, riproducibilità del bug e matrice firmware/device. Non usare telemetria cloud nascosta.

## Esplicitamente non pianificato senza nuova decisione

- sostituzione di Android stock con GSI/LineageOS;
- rimozione di Assistant Core, MediaShell o Provision;
- ADB TCP o accesso remoto root non autenticato;
- controllo MQTT di ADB, root, AVB, Wi-Fi o microfono;
- firmware OTA automatizzato senza un nuovo progetto di sicurezza e recovery;
- sincronizzazione calendario ottenuta manipolando dati privati Google;
- eliminazione del vault fisico di rollback.

## Come promuovere una voce a implementazione

1. creare requisito e definire il perimetro nel PRD/audit;
2. indicare branch, migrazioni, permessi, dati sensibili e compatibilità firmware;
3. progettare test host, test fisici, metriche low-RAM e rollback;
4. implementare senza mescolare la feature a una release RC non correlata;
5. aggiornare questa pagina soltanto dopo accettazione e merge.

## Collegamenti

[[Documentazione/00-Indice-Wiki|Indice wiki]] · [[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie|Calendario, meteo e sveglie]] · [[Documentazione/06-Media-e-Assistente-MiiPulse|Media e MiiPulse]] · [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Limiti RC1]].
