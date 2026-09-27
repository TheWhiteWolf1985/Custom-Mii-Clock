# Product Requirements Document

## Custom ROM / Custom Launcher — Xiaomi Mi Smart Clock

**Versione documento:** 0.1  
**Stato:** Draft iniziale  
**Target:** Xiaomi Mi Smart Clock  
**Tipologia progetto:** Firmware / Custom Android UI / Custom Launcher

---

# 1. Obiettivo del progetto

Realizzare una versione modificata del software dello Xiaomi Mi Smart Clock che mantenga il processo originale di configurazione e registrazione Google, ma sostituisca l'esperienza utente successiva con una shell/launcher personalizzato.

Il nuovo sistema dovrà trasformare il dispositivo da smart clock con interfaccia sostanzialmente fissa a smart display configurabile, mantenendo quanto più possibile compatibilità con hardware, servizi Google e componenti originali.

L'obiettivo non è creare una ROM Android completamente nuova da zero, ma modificare il firmware originale in maniera controllata e reversibile.

---

# 2. Principi di progetto

Il progetto dovrà rispettare i seguenti principi:

- mantenere il setup iniziale originale Xiaomi/Google;
- non interferire con il provisioning Google;
- utilizzare il firmware originale come base;
- sostituire il comportamento del launcher successivamente al completamento del setup;
- mantenere una possibilità di fallback verso il launcher/Clock originale;
- evitare dipendenze cloud aggiuntive;
- salvare localmente le configurazioni;
- rendere l'interfaccia utilizzabile principalmente tramite touch e gesture;
- mantenere il progetto sufficientemente modulare da poter aggiungere nuove schermate in seguito.

---

# 3. Comportamento del launcher

Il launcher originale Xiaomi non sarà l'interfaccia principale del sistema.

Dovrà essere mantenuto principalmente come:

- fallback;
- ambiente di recovery;
- possibile schermata Clock richiamabile dal launcher personalizzato.

Il nuovo launcher dovrà prendere il controllo dell'esperienza utente dopo il completamento del provisioning iniziale.

Il sistema dovrà impedire al launcher originale di riprendere automaticamente il controllo dello schermo durante il normale utilizzo del launcher personalizzato.

---

# 4. Primo avvio e provisioning

Dopo un factory reset o un nuovo flash:

1. il dispositivo dovrà avviarsi normalmente;
2. il setup Xiaomi/Google originale dovrà essere mostrato senza modifiche;
3. l'utente dovrà poter completare normalmente registrazione, Wi-Fi e configurazione Google;
4. il launcher personalizzato non dovrà interferire con il Setup Wizard;
5. una volta completato il provisioning, il launcher personalizzato potrà diventare l'interfaccia principale.

Il comportamento originale del setup Google deve essere preservato integralmente.

---

# 5. Navigazione principale

La navigazione dovrà essere basata principalmente su gesture touch.

## 5.1 Swipe orizzontale

Lo swipe verso sinistra o destra dovrà permettere di passare tra le schermate abilitate.

Esempio:

Clock → Dashboard → Applicazioni → schermata futura

L'ordine delle schermate dovrà essere configurabile dall'utente.

## 5.2 Swipe dall'alto

Uno swipe dalla parte superiore dello schermo dovrà aprire il pannello rapido.

## 5.3 Interazione

Il tap continuerà a essere utilizzato per l'interazione con pulsanti, widget, impostazioni e applicazioni.

---

# 6. Schermata Home

Il sistema dovrà introdurre il concetto di **schermata Home**, analogamente a un launcher Android tradizionale.

L'utente dovrà poter scegliere quale schermata venga considerata Home.

La schermata Clock originale sarà quella predefinita inizialmente.

Una successiva configurazione dell'utente potrà cambiare la Home.

---

# 7. Timeout di inattività

Dopo un periodo configurabile di inattività, il sistema dovrà tornare automaticamente alla schermata configurata come Home.

Il timeout dovrà essere configurabile.

Il comportamento non dovrà essere legato obbligatoriamente alla schermata Clock: sarà la scelta dell'utente della Home a determinarne la destinazione.

---

# 8. Sistema multi-schermata

Il launcher dovrà supportare un insieme dinamico di schermate.

Per ogni schermata l'utente dovrà poter:

- abilitarla;
- disabilitarla;
- cambiarne l'ordine;
- eventualmente impostarla come Home.

La struttura dovrà essere progettata in modo da poter aggiungere nuovi tipi di schermata senza modificare profondamente il core del launcher.

---

# 9. Gestione schermate

Dovrà essere disponibile una pagina specifica di configurazione delle schermate.

La pagina dovrà mostrare almeno:

- nome schermata;
- stato attiva/disattiva;
- posizione;
- controllo per il riordinamento;
- indicazione della schermata Home.

La configurazione dovrà essere persistente anche dopo il riavvio.

---

# 10. Applicazioni Android

Il firmware dovrà consentire l'installazione di APK Android compatibili con il dispositivo.

Le applicazioni installate dovranno essere disponibili tramite un **App Drawer** dedicato.

Il launcher non dovrà necessariamente trasformare ogni applicazione in una schermata del carosello principale.

L'App Drawer dovrà almeno:

- mostrare le applicazioni installate utilizzabili dall'utente;
- mostrare icona e nome;
- permetterne l'avvio tramite tap.

L'installazione APK potrà inizialmente essere gestita tramite ADB o strumenti Android esistenti.

Per requisito esplicito del cliente, il firmware dovra esporre ADB tramite USB immediatamente a qualunque PC, senza autorizzazione RSA, e `adb shell` dovra ottenere `uid=0(root)`. Dopo il fallimento del candidato 013, Magisk e il boot fisico gia funzionante diventano una dipendenza permanente della linea firmware: non si ricostruisce piu questo accesso modificando `system`. Non dovra essere abilitato ADB TCP; SELinux `Permissive` e accettato come configurazione definitiva e la catena AVB dovra rimanere attiva con flags zero. Il rischio di controllo root da parte di qualunque host con accesso fisico USB e accettato come requisito di prodotto.

Una GUI per l'installazione degli APK non è requisito obbligatorio della v1.

---

# 11. Impostazioni personalizzate

Il launcher dovrà includere una schermata di impostazioni semplificate.

Dovranno essere valutati almeno i seguenti controlli:

- Wi-Fi;
- luminosità;
- volume;
- configurazione schermate;
- schermata Home;
- timeout inattività;
- modalità notte;
- informazioni dispositivo;
- riavvio;
- accesso alle impostazioni Android complete.

L'interfaccia personalizzata non dovrà sostituire completamente le Settings Android originali.

Le pagine interne seguono una lista verticale in stile Settings Android,
senza barra di ricerca. `Data e Ora` contiene esclusivamente regolazione data,
regolazione ora, regolazione fuso orario e ora automatica con toggle. La pagina
`Informazioni` presenta in tile verticali i dati reali di dispositivo, Android,
firmware, launcher, root e ADB/USB.

---

# 12. Accesso alle impostazioni Android

L'utente dovrà poter aprire le impostazioni Android originali attualmente nascoste nell'esperienza stock.

L'accesso potrà essere effettuato tramite apposito comando nella pagina delle impostazioni personalizzate.

Qualora alcune sezioni delle Settings originali siano incompatibili con il dispositivo, queste non dovranno compromettere la stabilità del launcher.

---

# 13. Pannello rapido

Lo swipe dall'alto dovrà aprire un pannello rapido.

La prima specifica del pannello comprende:

- luminosità;
- volume;
- stato/accesso Wi-Fi;
- Home;
- Settings;
- riavvio.

Dovrà essere prevista strutturalmente la possibilità di aggiungere ulteriori pulsanti in futuro.

Il controllo Home Assistant previsto concettualmente verrà mantenuto fuori dalla v1 finché l'integrazione HA non verrà attivata.

---

# 14. Modalità notte

La modalità notte dovrà utilizzare principalmente il dimming automatico del display.

Il passaggio alla modalità notte:

- non dovrà necessariamente cambiare schermata;
- dovrà mantenere la schermata attualmente visualizzata;
- dovrà diminuire la luminosità secondo il comportamento configurato o disponibile nel firmware.

Quando tecnicamente possibile, dovranno essere riutilizzati i meccanismi originali Xiaomi/Android per il dimming invece di duplicarne il comportamento.

---

# 15. Persistenza della configurazione

Le impostazioni del launcher dovranno essere salvate localmente in un file JSON leggibile.

Il file dovrà contenere almeno:

- schermate disponibili;
- schermate abilitate;
- ordine delle schermate;
- schermata Home;
- timeout inattività;
- configurazione modalità notte;
- preferenze UI;
- eventuali impostazioni future Home Assistant.

Il formato dovrà essere:

- leggibile;
- documentato;
- versionato;
- tollerante all'aggiunta futura di nuove proprietà.

Esempio concettuale:

```json
{
  "config_version": 1,
  "home_screen": "clock",
  "idle_timeout": 60,
  "screens": [
    {
      "id": "clock",
      "enabled": true,
      "order": 0
    },
    {
      "id": "apps",
      "enabled": true,
      "order": 1
    }
  ]
}
```

Il percorso definitivo del file sarà deciso durante l'analisi del firmware e dei permessi disponibili.

---

# 16. Recovery e fallback

La modifica non dovrà lasciare il dispositivo inutilizzabile nel caso in cui il nuovo launcher vada in crash o non riesca ad avviarsi.

Dovrà essere implementato un meccanismo di fallback verso:

- Clock originale;
- launcher originale;
- oppure ambiente diagnostico.

Il meccanismo dovrà permettere almeno di:

- recuperare l'accesso al dispositivo;
- modificare o cancellare una configurazione corrotta;
- diagnosticare il problema;
- riavviare il launcher personalizzato.

La scelta tecnica definitiva del watchdog/fallback verrà effettuata dopo l'analisi del firmware originale.

---

# 17. Diagnostica

Il progetto dovrà prevedere una modalità diagnostica minima.

Dovranno essere disponibili almeno informazioni relative a:

- versione launcher;
- versione configurazione;
- versione firmware;
- stato launcher;
- ultima causa di errore nota;
- accessibilità del launcher originale;
- informazioni basilari sul dispositivo.

Il logging dovrà evitare scritture eccessive sulla memoria flash.

---

# 18. Home Assistant

Home Assistant è una funzionalità prevista dal progetto, ma **non farà parte della prima versione funzionale**.

L'architettura dovrà comunque evitare scelte che ne rendano difficile l'integrazione futura.

La futura implementazione prevista utilizzerà preferibilmente:

- URL locale configurabile;
- WebView integrata;
- login gestito dalla WebView;
- visualizzazione fullscreen/kiosk;
- gestione di perdita della connessione;
- reload manuale o automatico.

La futura schermata Home Assistant dovrà poter essere gestita esattamente come le altre schermate:

- abilitabile;
- disabilitabile;
- riordinabile;
- eventualmente impostabile come Home.

---

# 19. Architettura concettuale

L'architettura attesa è:

```text
Firmware Xiaomi originale
        │
        ├── Android
        ├── servizi Google
        ├── Setup Wizard
        ├── componenti hardware
        └── launcher / Clock originale
                    │
                    ▼
           Custom Launcher
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      Clock      Dashboard    App Drawer
        │
        └───────────┬───────────┘
                    ▼
              Quick Settings
```

Home Assistant verrà aggiunto successivamente come ulteriore modulo/schermata.

---

# 20. Scope v1

La prima versione dovrà concentrarsi sulle fondamenta del sistema.

## Incluso

- boot corretto;
- setup Google originale;
- custom launcher;
- navigazione swipe;
- sistema multi-schermata;
- Clock come Home predefinita;
- scelta Home;
- ritorno alla Home dopo inattività;
- configurazione schermate;
- riordinamento schermate;
- App Drawer;
- avvio APK installati;
- impostazioni personalizzate;
- accesso Settings Android;
- pannello rapido;
- luminosità;
- volume;
- Wi-Fi;
- reboot;
- dimming/modalità notte;
- configurazione JSON locale;
- recovery/fallback;
- diagnostica minima.

## Escluso dalla v1

- Home Assistant integrato;
- dashboard HA definitiva;
- gestione avanzata WebView;
- installer APK grafico;
- aggiornamento OTA del custom launcher;
- store applicazioni;
- sincronizzazione cloud;
- account aggiuntivi;
- sistema plugin pubblico.

---

# 21. Fasi di sviluppo

## Fase 0 — Reverse engineering

Prima di modificare il firmware dovranno essere identificati:

- SoC;
- versione Android;
- API level;
- layout delle partizioni;
- boot image;
- filesystem;
- launcher originale;
- Clock originale;
- SystemUI;
- Settings;
- framework;
- servizi Xiaomi;
- servizi Google;
- Setup Wizard;
- eventuali firme APK;
- SELinux;
- Verified Boot;
- modalità di aggiornamento OTA;
- possibilità di flash e recovery.

Il materiale inizialmente disponibile è solamente il firmware originale / OTA.

## Fase 1 — Firmware modificabile

Obiettivo:

- estrarre firmware;
- identificare partizioni;
- ricostruire immagini;
- riuscire a flashare una modifica minima;
- verificare il boot;
- definire procedura affidabile di recovery.

Nessuna modifica importante alla UX deve essere implementata prima di raggiungere questo risultato.

## Fase 2 — Custom launcher minimo

Implementare:

- avvio custom launcher;
- schermata test;
- navigazione;
- fallback launcher originale.

## Fase 3 — Sistema schermate

Implementare:

- registry schermate;
- abilitazione;
- disabilitazione;
- riordinamento;
- scelta Home;
- persistenza JSON.

## Fase 4 — Funzioni Android

Implementare:

- App Drawer;
- apertura Settings;
- quick settings;
- luminosità;
- volume;
- Wi-Fi;
- reboot.

## Fase 5 — Stabilità

Implementare e verificare:

- timeout;
- dimming;
- crash recovery;
- watchdog;
- diagnostica;
- comportamento dopo reboot;
- comportamento dopo factory reset.

## Fase futura — Home Assistant

Implementare il modulo HA dopo il consolidamento della piattaforma.

---

# 22. Requisiti non funzionali

Il custom launcher dovrà:

- avviarsi in maniera affidabile;
- non impedire il boot del dispositivo;
- non rompere il provisioning Google;
- mantenere un consumo RAM compatibile con l'hardware;
- evitare servizi in background non necessari;
- evitare scritture flash frequenti;
- essere utilizzabile interamente tramite touchscreen;
- funzionare anche senza connessione Internet;
- non richiedere server esterni;
- non aggiungere telemetria;
- mantenere configurazione e dati localmente.

---

# 23. Criteri di accettazione v1

La v1 potrà essere considerata completata quando:

1. il dispositivo completa normalmente il setup originale Google;
2. il custom launcher viene avviato dopo il provisioning;
3. sono presenti almeno due schermate navigabili tramite swipe;
4. è possibile scegliere e riordinare le schermate;
5. è possibile impostare una schermata Home;
6. dopo inattività il launcher torna alla Home configurata;
7. l'App Drawer mostra e avvia APK compatibili installati;
8. è possibile accedere alle Settings Android;
9. il pannello rapido controlla almeno luminosità e volume;
10. le impostazioni sopravvivono al riavvio;
11. la configurazione viene salvata in JSON;
12. il dispositivo mantiene il comportamento di dimming previsto;
13. il crash del custom launcher non rende inutilizzabile il dispositivo;
14. esiste un percorso verificato per tornare al launcher originale;
15. esiste una procedura verificata per ripristinare il firmware originale.

---

# 24. Materiale disponibile

Attualmente il punto di partenza disponibile è:

**firmware originale / OTA dello Xiaomi Mi Smart Clock.**

L'estrazione e classificazione dei componenti del firmware costituisce quindi il primo vero task tecnico del progetto.

---

# 25. Decisioni ancora dipendenti dal reverse engineering

Non vengono definite anticipatamente perché dipendono dalla struttura effettiva del firmware:

- tecnologia con cui sviluppare il custom launcher;
- modifica o sostituzione degli APK originali;
- gestione firma APK;
- necessità di modificare `system`, `product` o altre partizioni;
- modalità di disattivazione del launcher originale;
- posizione del file JSON;
- accesso programmatico a luminosità e Wi-Fi;
- metodo di reboot;
- gestione privilegi;
- comportamento SELinux;
- eventuali modifiche al boot image;
- tecnica di watchdog;
- meccanismo di fallback;
- modalità di integrazione del Clock originale nel nuovo launcher.

Questi elementi dovranno essere determinati da Codex attraverso l'analisi del firmware e documentati prima di implementare modifiche invasive.

---

# 26. Regola operativa per lo sviluppo

Prima di modificare il firmware, Codex dovrà produrre un rapporto iniziale contenente:

1. struttura del firmware;
2. versione Android;
3. SoC e architettura;
4. partizioni identificate;
5. APK/componenti rilevanti;
6. launcher attualmente configurato;
7. meccanismo utilizzato per forzarne l'esecuzione;
8. stato di Verified Boot e firme;
9. possibili strategie di modifica;
10. rischi di brick;
11. procedura di recovery disponibile;
12. proposta tecnica per implementare il custom launcher.

Solo successivamente si procederà alle modifiche effettive.

---

# 27. Visione successiva alla v1

Una volta stabilizzata la piattaforma, potranno essere aggiunti:

- Home Assistant kiosk;
- dashboard personalizzate;
- widget;
- schermate informative;
- meteo;
- calendario;
- foto;
- MQTT;
- schermate web generiche;
- ulteriori gesture;
- automazioni contestuali;
- temi;
- configurazione visuale avanzata.

Il core dovrà quindi essere progettato come piattaforma estendibile, evitando di legare il launcher alle sole funzionalità della v1.
