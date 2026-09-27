# Checklist funzionalità di base v1

1. **Gestione completa delle schermate — COMPLETATA in 0.14.1**

   `Impostazioni > Gestione schermate` raccoglie tutta la configurazione V1:
   - attivazione e disattivazione delle schermate tramite interruttore;
   - riordino con controlli su/giù;
   - scelta della schermata Home tra quelle attive;
   - timeout di ritorno automatico da 15 secondi a 10 minuti;
   - Home sempre attiva e fallback sicuro se la Home selezionata viene nascosta;
   - Media esclusa dalla V1 e non riattivabile dalla configurazione;
   - migrazione atomica del JSON v1 alla versione 2.

   Il long press sull’area libera della Home apre la stessa pagina e il launcher
   ricarica ordine, visibilità, Home e timeout quando torna in primo piano.
   Installazione, migrazione, controlli, ritorno temporizzato e carosello a sei
   schermate sono stati verificati fisicamente sul Clock.

2. **Pannello Impostazioni rapide — COMPLETATO in 0.15.0**

   La tendina Android-like segue il dito dal bordo superiore, si estende verso
   il basso e fa snap in apertura o chiusura. Controlla realmente:
   - luminosità manuale con curva stock e automatica tramite sensore Android;
   - volume multimediale sincronizzato con i tasti fisici;
   - attivazione e disattivazione Wi-Fi, con ripristino del launcher in primo piano;
   - attivazione e disattivazione microfono sincronizzata con il tasto fisico;
   - riavvio tramite Magisk soltanto dopo conferma esplicita.

   Build, permessi, installazione, stati Android, gesture progressiva, webcam,
   controlli fisici e ripristino degli stati iniziali sono stati verificati sul
   Clock. Il collaudo del riavvio si è fermato alla conferma e ha scelto
   `Annulla`, senza riavviare il dispositivo.

3. **Modalità notte e gestione display**

   Bisogna integrare e verificare:
   - dimming automatico;
   - eventuale fascia oraria;
   - luminosità minima configurabile;
   - ripristino corretto della luminosità;
   - compatibilità con il comportamento stock del sensore/display;
   - mantenimento della schermata corrente durante il dimming.

4. **Meteo con dati reali — COMPLETATO in 0.12.0**

   Il backend autonomo usa MET Norway Locationforecast 2.0 ed è stato verificato
   fisicamente sul Clock:
   - località configurata manualmente senza permessi GPS;
   - download HTTPS dei dati reali;
   - cache locale atomica e conservazione degli ultimi dati;
   - stato stale/offline esplicito;
   - aggiornamento automatico configurabile e refresh manuale;
   - previsione composta da cinque giorni esatti;
   - unità temperatura e vento configurabili;
   - prova rete assente/ripristinata superata senza perdita della cache;
   - Home e schermata Meteo alimentate dallo stesso snapshot reale.

   La probabilità pioggia resta `--` quando il payload del provider non la
   espone; nessun valore viene inventato. Report e immagini sono in
   `MiiClockLauncher/releases/0.12.0/`.

5. **Schermata Media — RINVIATA OLTRE LA V1 (launcher 0.14.1)**

   La pagina provvisoria è esclusa dal carosello e dalla configurazione V1. Il
   Clock usa Android 10/API 29 con `ro.config.low_ram=true`: su questa
   configurazione un `NotificationListenerService` non costituisce un accesso
   affidabile alle sessioni multimediali.

   Le specifiche e il mockup restano versionati per una futura implementazione,
   che richiederà:
   - launcher integrato nel firmware sotto `/product/priv-app`;
   - allowlist del permesso privilegiato `MEDIA_CONTENT_CONTROL`;
   - controllo generico delle `MediaSession` Android;
   - adattamenti verificati per YouTube, Spotify e MediaShell stock;
   - prevalenza delle specifiche testuali sul mockup: niente coda, preferiti,
     selettore dispositivo o slider del volume.

   Fino alla riattivazione esplicita non vengono introdotti permessi, listener,
   bridge root o servizi persistenti per Media.

6. **Traduzioni complete**

   Il selettore contiene già Italiano, Inglese, Tedesco, Spagnolo e Francese, ma gran parte delle stringhe è ancora scritta direttamente in italiano. Bisogna:
   - spostare tutti i testi nelle risorse localizzate;
   - applicare immediatamente la lingua selezionata;
   - tradurre Home, Calendario, Meteo, Sveglie, App Drawer e Impostazioni;
   - verificare testi lunghi e layout 800×480;
   - mantenere la scelta dopo il riavvio.

7. **Completamento e certificazione delle Sveglie — COMPLETATA E ACCETTATA**

   Il motore autonomo, il CRUD e l'interfaccia sono stati provati sul Clock.
   Le evidenze tecniche della 0.6.1 coprono trigger temporale, suono, Snooze,
   Stop, persistenza e assenza di crash; le successive prove d'uso hanno
   confermato creazione, modifica, rinomina ed eliminazione delle sveglie.
   Il comportamento corrente è stato accettato dall'utente il 2026-09-27.

   Il punto funzionale è quindi chiuso. Sveglie ricorrenti, ordinamento,
   ripianificazione dopo riavvio, persistenza e arresto automatico rimangono
   inclusi nella regressione unificata della V1 al punto 11, senza bloccare
   ulteriormente lo sviluppo delle funzioni mancanti.

8. **Aspetto — COMPLETATO (launcher 0.14.0)**

   Implementato e verificato fisicamente sul Clock secondo lo scopo ridotto
   stabilito per la v1:
   - `Impostazioni > Aspetto` attivo con tile verticali;
   - caricamento da browser LAN di JPEG, PNG e WebP fino a 8 MiB;
   - endpoint monouso protetto da token e limitato a cinque minuti;
   - normalizzazione dell'immagine a 800×480 con orientamento, `centerCrop`,
     JPEG qualità 90 e sostituzione atomica;
   - persistenza dello sfondo personalizzato negli aggiornamenti APK;
   - ripristino confermato dello sfondo predefinito;
   - formato 12/24 ore collocato in `Data e Ora`, persistente e applicato a
     Home e intestazione globale;
   - riavvio completo superato con APK, HOME, ADB root e formato 24 ore intatti.

   Per la v1 la dimensione dell'orologio resta fissa e non vengono introdotti
   temi o personalizzazioni di data, meteo e layout. Lo sfondo dell'utente vive
   in `/data`, quindi non consuma lo spazio residuo della partizione `product`.
   Evidenze e report sono in `MiiClockLauncher/releases/0.14.0/evidence/`.

9. **Diagnostica e recupero automatico — COMPLETATO (launcher 0.13.0)**

   Implementato e verificato fisicamente sul Clock:
   - versione e validità della configurazione JSON;
   - versione launcher/firmware, stato HOME e primo piano reali;
   - ultimo errore persistente, sintetico e con scritture duplicate limitate;
   - stato di MET Norway, cache meteo e motore sveglie;
   - riavvio controllato del solo task MiiClock;
   - disponibilità e apertura del Clock originale, con verifica della HOME stock;
   - watchdog: tre crash non gestiti in cinque minuti attivano il recupero;
   - UI minima di recupero indipendente da `LauncherView`, con avvio MiiClock,
     ripristino atomico della configurazione, diagnostica e Clock originale;
   - ritorno alla Home MiiClock provato anche dopo aver aperto il Clock stock;
   - cold start, HOME predefinita, ADB root e assenza di crash/ANR verificati.

   Evidenze e report sono in `releases/0.13.0/evidence/DEVICE_ACCEPTANCE.md`.

10. **Memoria — COMPLETATA (launcher 0.16.0)**

    - Accettazione utente registrata il 2026-09-27.

    Implementata e verificata fisicamente sul Clock in `Impostazioni > Memoria`:
    - RAM totale e disponibile reali;
    - PSS MiiClock tramite API Android;
    - `TOTAL PSS` aggregato di Assistant, inclusa la memoria grafica;
    - pressione `lowMemory`, soglia di sistema e stato low-RAM;
    - limite heap del processo;
    - aggiornamento ogni cinque secondi e scorrimento verticale Android-like.

    Due campioni, screenshot e webcam hanno confermato valori coerenti, layout
    800×480 e assenza di crash/ANR. Evidenze in
    `MiiClockLauncher/releases/0.16.0/evidence/DEVICE_ACCEPTANCE.md`.

11. **Collaudo completo della v1**

    Dopo le implementazioni bisognerà eseguire un gate unico comprendente:
    - più riavvii e almeno uno spegnimento completo;
    - navigazione continua tra tutte le schermate;
    - timeout e ritorno alla Home configurata;
    - App Drawer e avvio applicazioni;
    - Calendario locale CRUD;
    - Sveglie complete;
    - Meteo online/offline;
    - Quick Settings;
    - cambio lingua;
    - dimming notturno;
    - perdita e ritorno della rete;
    - monitoraggio prolungato di RAM, crash e foreground;
    - screenshot e webcam di ogni vista;
    - verifica ADB root e USB;
    - rollback applicativo e firmware.

    Il factory reset andrà provato soltanto con autorizzazione esplicita, perché cancella i dati correnti.

12. **Integrazione nella prossima release firmware**

    Durante lo sviluppo continueremo a installare l’APK in `/data/app`, evitando flash ripetuti. Alla fine dovremo:
    - rendere l’APK sufficientemente compatto oppure riprogettare lo spazio di `product`;
    - eseguire due build riproducibili;
    - verificare firma e hash;
    - ricostruire `product`, `super` e la catena AVB;
    - creare il manifest di flash e rollback;
    - flashare e ripetere il collaudo con la copia realmente integrata nel firmware.

13. **OTA launcher firmato — IMPLEMENTATO E VALIDATO FISICAMENTE**

    Il launcher 0.17.3 include manifest RSA firmato, validazione completa APK,
    download atomico, journal persistente, processo updater separato, backup,
    health check e rollback automatico. Il percorso reale 0.17.2 → 0.17.3 ha
    superato download da GitHub, firma/hash/package/certificato, installazione
    root, watchdog, HOME, riavvio e persistenza senza flash. Audit, piano e
    risultato sono in `OTA_AUDIT_017.md`,
    `../EXECUTION/OTA_PHYSICAL_TEST_PLAN_017.md` e
    `OTA_LIVE_TEST_0173.md`.

Non considero mancanti per la v1 Home Assistant, sincronizzazione Google del Calendario, store applicazioni e installer APK grafico. Home, App Drawer, Calendario locale, Data/Ora, Informazioni, firmware HOME 015 e accesso ADB root sono invece già realizzati e fisicamente verificati.
