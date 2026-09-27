# DECISIONS

## ADR 011 — OTA firmato e fail-safe del solo launcher

- Date: 2026-09-27
- Context: lo sviluppo del launcher procede tramite aggiornamenti `/data/app`;
  i flash ripetuti non sono necessari e GitHub deve essere solo transport.
- Decision: usare GitHub Releases di `TheWhiteWolf1985/Custom-Mii-Clock` con
  manifest schema 1 firmato RSA-3072/SHA-256 e seconda verifica della firma
  Android ufficiale. Check automatico al massimo giornaliero; download e
  installazione restano esplicitamente manuali. La transazione usa un worker
  separato, journal atomico, rollback verificato e guard temporaneo Magisk.
- Consequences: la privata OTA resta nel vault LUKS; il Clock contiene soltanto
  la pubblica. Il percorso normale non ammette downgrade. Nessuna partizione è
  modificata e `/product` resta il fallback estremo. Ogni nuova release deve
  pubblicare esattamente APK, manifest e firma detached canonici.

## ADR 001 — Conservare la stock e modificarla offline

- Date: 2026-09-09 (decisione riportata dal passaggio di consegne)
- Context: GSI AOSP 10 e LineageOS20_X04G sono state flashate ma hanno prodotto bootloop; la stock recuperata è funzionante, con ADB root e Fully già installato.
- Decision: ricostruire una variante modificata della stock intervenendo offline sulla `super`; iniziare dall'integrazione di Fully, poi intervenire soltanto sul meccanismo launcher necessario.
- Alternatives: ROM alternativa/GSI; hook init nel boot; configurazioni runtime di Fully/HOME.
- Consequences: serve una catena riproducibile di unpack/repack e copie immutabili delle immagini; non trattare GSI/Lineage o workaround runtime già falliti come baseline.

## ADR 002 — Preservare i dati e limitare l'intervento Google

- Date: 2026-09-09 (decisione riportata dal passaggio di consegne)
- Context: dopo il recovery stock la configurazione del clock è stata rifatta; `/data` contiene configurazione utile. `com.google.assistant.core` è protetto e potrebbe contenere servizi essenziali.
- Decision: non fare reset/wipe durante il lavoro sulla stock e non rimuovere l'intero package Google core; analizzare e neutralizzare selettivamente launcher/shim solo dopo una stock+Fully avviabile.
- Alternatives: wipe e ripartenza da zero; disabilitazione/rimozione completa Google.
- Consequences: ogni flash richiede rollback mirato e verifica che `/data` resti valida; la neutralizzazione va preceduta da analisi di manifest, intent, receiver e dipendenze.

## ADR 003 — Usare Linux reale per l'USB del dispositivo

- Date: 2026-09-09 (decisione riportata dal passaggio di consegne)
- Context: fastboot Windows ha avuto problemi driver e Zadig ha interferito con ADB; Xubuntu ha eseguito flash e ADB in modo affidabile.
- Decision: preparare e usare Xubuntu/Linux reale per ADB, fastboot e operazioni firmware sul dispositivo.
- Consequences: i file citati su Windows/WSL/NAS sono materiale storico o di trasferimento, non un ambiente di flash preferito.

## ADR 004 — Backup e chiave privata soltanto su LUKS dedicato

- Date: 2026-09-12
- Context: backup fisico e chiave AVB privata contengono dati o materiale che non deve entrare in Git.
- Decision: usare un disco virtuale dedicato da 16 GiB cifrato LUKS, montato solo durante backup, firma, preflash e rollback.
- Consequences: in Git entrano solo hash, report redatti, chiave pubblica e policy. Il requisito del disco separato è stato successivamente adattato da ADR 006 per indisponibilità di spazio hypervisor.

## ADR 005 — Toolchain host congelata e fail-closed

- Date: 2026-09-12
- Decision: MTKClient `v2.1.4.1` e AOSP avbtool `1.2.0` sono conservati con dipendenze offline e hash. Ogni operazione fisica usa argv validati senza shell e rifiuta wipe, erase, unlock e bypass AVB.
- Consequences: un manifest di scrittura richiede rollback pronto, quota sufficiente, commit/push e conferma runtime `flash ora`; `vbmeta` root è ultima.

## ADR 006 — Usare un contenitore LUKS sul disco VM esistente

- Date: 2026-09-12
- Context: non è possibile aggiungere un altro drive virtuale alla VM per mancanza di spazio disponibile sull'host.
- Decision: creare un container sparse LUKS2 da 16 GiB sul filesystem della VM, con chiave root-only fuori dal repository e mount temporaneo `nosuid,nodev,noexec`.
- Consequences: backup e chiave AVB privata restano fuori da Git e il vault è normalmente chiuso; container e chiave sullo stesso disco non difendono da compromissione root, rischio accettato nel trusted homelab.

## ADR 007 — Integrare ADB USB aperto e root nel firmware

- Date: 2026-09-13
- Context: il candidato 012 con boot pulito ha rimosso il ponte ADB/root fornito dal precedente boot fisico Magisk e il device e tornato `unauthorized`. Il cliente richiede accesso immediato da qualunque PC e shell root.
- Decision: integrare nel `system` del progetto `ro.secure=0`, `ro.adb.secure=0`, `ro.debuggable=1`, trasporto USB ADB persistente e una regola init dedicata. Non usare Magisk o script di terzi nel firmware definitivo.
- Consequences: qualunque host con accesso fisico USB ottiene controllo root senza conferma sul dispositivo; rischio accettato esplicitamente come requisito cliente. ADB TCP, netcat e rootshell di rete restano vietati, SELinux deve essere `Enforcing` e AVB non viene bypassata.

## ADR 008 — Rendere permanente il boot Magisk fisico

- Date: 2026-09-13
- Context: il candidato 013 ha avviato Android ma la modifica autonoma di `system` ha eliminato la funzione USB Android/ADB. Il boot fisico Magisk, SHA-256 `112b8952c6a2ae833e468fd40bff654210f588192a0c589ef1f017fcc8d6a756`, aveva gia fornito ADB USB immediato, shell root e controller `musb-hdrc` funzionante.
- Decision: ADR 007 e superata. La linea firmware usa permanentemente quel payload Magisk, rifirmato con la chiave AVB del progetto senza decomprimere o ricostruire il ramdisk. SELinux `Permissive` e accettato; ADB TCP resta vietato; AVB resta attiva con flags zero.
- Consequences: la release 014 riusa `super`, `product`, `vbmeta_system` e `vbmeta` esatti della 012 e non usa alcun artefatto `system`/`super` della 013. Ogni release successiva deve conservare il boot Magisk 014 come baseline finche una decisione esplicita e verificata non lo sostituisce.

## ADR 009 — Aggiornare il Calendario come APK non-HOME

- Date: 2026-09-15
- Context: firmware 014 già funzionante e launcher 0.2.0 installato come aggiornamento `/data/app`; le nuove viste non richiedono la ricostruzione di `super`.
- Decision: sviluppare e verificare il Calendario nel ramo `codex/calendar-screen-v0.3.0`, distribuendo la release completa 0.3.1 con `adb install --no-streaming -r`, stesso package/certificato, senza acquisire ancora il ruolo HOME. La prima 0.3.0, con selettore mese/anno provvisorio, rimane soltanto un punto di rollback versionato.
- Consequences: il backup/firmware 014 e la copia launcher 0.1.0 in `/product` restano invariati; ADB root e aggiornamento persistono ai reboot. Nessun provider eventi è collegato e l'Agenda mostra errore esplicito anziché dati demo. L'overlay dell'Assistant stock non va alterato come effetto collaterale di questa vista.

## ADR 010 — Calendario autonomo su CalendarProvider AOSP

- Date: 2026-09-18
- Context: sul clock è presente `com.android.providers.calendar` con un solo
  calendario `LOCAL` vuoto; non sono installati Google Calendar, Google Calendar
  Sync Adapter, GMS o GSF e i package Assistant non espongono un backend
  calendario pubblico. La schermata Calendario 0.3.1 era quindi priva di dati.
- Decision: usare un calendario dedicato `MiiClock` con account type `LOCAL` come
  sorgente canonica offline. La vista nel carosello resta consultiva; il CRUD è
  separato nelle Impostazioni del launcher. Letture e scritture passano soltanto
  da `CalendarContract`, mai da database o token privati dell'Assistant.
- Consequences: il launcher richiede `READ_CALENDAR` e `WRITE_CALENDAR`, carica
  `Instances` fuori dal thread UI, mantiene cache/stato stale e osserva le
  modifiche del provider. Non esiste sincronizzazione cloud nella 0.8.0; Google
  o CalDAV potranno essere connettori opzionali futuri senza sostituire il
  calendario locale.
