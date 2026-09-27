# Worklog

## 2026-09-09 — Bootstrap knowledge base

- Letto `xiaomi-mi-smart-clock-x04g-passaggio-consegne.md`.
- Compilati documenti di progetto, runbook, inventario, decisioni, rischi, glossario e task operativi.
- Nessuna immagine firmware, host Xubuntu o dispositivo è stata modificata o verificata in questa sessione.
- Validazione YAML automatica non eseguita: il modulo Python `yaml` non è installato; struttura controllata manualmente.

## 2026-09-13 — Gate 3 logo sentinella e restore

- Preparata da `logo` fisica una sentinella con un solo byte modificato nel padding finale; originale e candidata versionate con SHA-256.
- Pubblicati manifest fail-closed per scrittura e restore, con start quota 40%, stop 5% e autorizzazione runtime.
- Corretto prima della scrittura il probe fastboot bloccante aggiungendo timeout e test automatico; suite finale 19/19 PASS.
- Flashata soltanto `logo` sentinella: fastboot OKAY, boot Android completo e hash sentinella confermato dalla partizione via ADB root.
- Ripristinata immediatamente la `logo` del backup fisico: fastboot OKAY, secondo boot completo e hash originale confermato.
- `device_provisioned` e `user_setup_complete` sono rimasti `1`; webcam finale sulla schermata stock `Configurazione` normale.
- Nessun erase, wipe, bypass AVB o flash di altre partizioni; stato finale del Clock uguale alla baseline per `logo`.
- L'utente ha chiarito che il prossimo candidato deve produrre un cambiamento visibile, non soltanto una sentinella tecnica.

## 2026-09-13 — Recupero Magisk e candidato 014

- Congelata la release 013 come fallita: Android avviato, ma interfaccia USB Android/ADB non enumerata; fastboot rimasto recuperabile.
- Superata la strategia di ADB autonomo in `system`: Magisk e SELinux `Permissive` diventano baseline permanente approvata.
- Selezionato esclusivamente il boot fisico gia funzionante da 16 MiB e SHA-256 `112b8952c6a2ae833e468fd40bff654210f588192a0c589ef1f017fcc8d6a756`.
- Implementati builder fail-closed, doppia build deterministica, inventario ramdisk Magisk, confronto payload, firma AVB progetto e validatore completo del candidato 014.
- Il nuovo manifest autorizza solo `boot`, `vbmeta_system`, `super` e `vbmeta`; `super` e le immagini AVB diverse dal boot vengono riusate byte-identiche dalla 012.
- Soglia operativa aggiornata su richiesta: stop e minimo di partenza al 5% residuo.
- Doppia build reale eseguita nel vault: entrambe le immagini e la copia candidata hanno SHA-256 `080584f845456a9db3d473430f28ce105aacde16d17e162f764b934532ba9a6b`.
- Il payload boot e rimasto byte-identico fino a 8439808; tutte le differenze iniziano dopo tale confine e appartengono a padding/footer AVB.
- Validatore offline 014 PASS: Magisk inventariato, `system` stock e `product` 012 verificati dalla `super`, sparse roundtrip, APK esatto, 013 esclusa e chain AVB completa.
- Candidato e manifest trasferiti dalla VM al repository tramite archivio SHA-256 `5f8d44c183ca74cf4a4a347a3eb1e0b24e3aabe4dc2b1f3484169a92c73257ef`; nessuna chiave privata inclusa.
- Candidato finale pubblicato nel commit `599f4ff`; validatore manifest con `--require-pushed` PASS 4/4 e `origin/main` identico a HEAD.
- Rollback fisico rivalidato PASS 6/6 e candidato finale rivalidato sulla VM con report SHA-256 `7caafcd456a2836153b55a4850ecbeafd2963fdee5b91946b683fe34d29dc2ce`.
- Preflight fastboot 014 PASS: cinque enumerazioni stabili, tre probe `mico_x04g`, quattro dimensioni conformi, bootloader unlocked/non-secure e frame webcam; zero scritture.

## 2026-09-14 — Flash fisico e accettazione tecnica 014

- Ricontrollata la quota prima del gate: 41% residuo nella finestra di cinque ore e 79% settimanale, sopra la soglia di stop del 5%.
- Flashati nell'ordine autorizzato `boot` Magisk 014, `vbmeta_system` 012, `super` sparse 012 e `vbmeta` root 012; tutti i comandi hanno restituito `OKAY` e `vbmeta` e rimasta ultima.
- Non sono state scritte altre partizioni e non sono stati usati erase, format, wipe o flag di bypass AVB.
- Primo boot PASS: Android completo, USB `adb` su `musb-hdrc`, ADB immediato e root, Magisk attivo, SELinux `Permissive`, AVB `orange`/verity `enforcing`, provisioning conservato e nessun listener 5555.
- Una chiave ADB host mai autorizzata prima ha ottenuto immediatamente stato `device` e shell `uid=0(root)`; ADB TCP e rimasto disabilitato.
- Launcher v0.1.0 PASS via webcam: schermata visibile, pagina Applicazioni con swipe, apertura Impostazioni Android, ritorno al launcher e ritorno alla UI stock.
- Secondo reboot PASS in circa 69 secondi; tutti i controlli USB/ADB root/Magisk/AVB sono rimasti validi e il launcher si e riaperto con `Status: ok`.
- Importati report, log fastboot e fotografie; `99-SHA256SUMS.txt` verifica l'intero set di evidenze fisiche.
- Chiuso il vault tramite wrapper, verificato il mapper LUKS inattivo e rimosso l'archivio temporaneo; Android e ADB sono rimasti operativi.
- Ripetuto su richiesta il percorso UI via ADB con cinque nuove fotografie: launcher, Applicazioni, Settings Android, UI stock e launcher riaperto; tutte le activity e i frame sono coerenti.

## 2026-09-14 — Consolidamento baseline e pulizia

- Resa autonoma la 014: APK, product, super sparse, recovery, dtbo e vbmeta sono ora contenuti direttamente nel candidato, senza dipendenze dalla 012.
- Aggiornati validatore e manifest affinche usino soltanto la directory 014 e il rollback fisico.
- Ripetuti nella VM i gate offline completi: candidato 014 PASS, manifest flash 4/4 PASS e rollback fisico 6/6 PASS.
- Creata nel vault una copia autonoma della 014 con seconda build indipendente del boot e checksum completo.
- Rimossi dal vault soltanto analysis e candidate intermedi, recuperando 4985381963 byte; backup, chiavi e baseline 014 sono rimasti invariati per dimensione e checksum.
- Rimossi dal repository candidate 011/012/013, copie stock estratte, output intermedi e tooling one-shot; stock compresso, sorgenti, toolchain, evidenze 014 e know-how in `AI/` sono preservati.
- Stato finale vault: backup 6609696714 byte, chiavi 4509 byte, baseline 014 1323501246 byte; rollback e candidato rivalidati dopo la pulizia, poi vault smontato e mapper chiuso.
- Riscritta con autorizzazione esplicita la storia Git su un root commit pulito e pubblicati atomicamente `main`, `Base_version`, `develop` e il tag `v0.1.2-working-base`, tutti sulla stessa baseline.

## 2026-09-14 — Launcher v0.2.0 Home interattiva

- Creato `codex/home-screen-v0.2.0` da `develop`, preservando i file Mockup; i rami baseline e il firmware 014 non sono stati modificati.
- Implementati Home Canvas 800x480, componenti riutilizzabili, wallpaper separato, stato Wi-Fi, fallback meteo, dock, sette schermate stabili, carosello e gesture.
- Aggiunta configurazione JSON v1 atomica con Home, ordine/abilitazione schermate e timeout predefinito di 60 secondi.
- Due build release offline e due firme indipendenti sono byte-identiche; APK firmato SHA-256 `85473f7f32c1698f47008fcbf7cf7d2fd916e51ef18634c00753e4dc4be76610`.
- Installato esclusivamente l'aggiornamento APK via ADB: versionCode 2 da `/data/app`, firma invariata e ruolo HOME stock preservato.
- Verificate sul Clock navigazione, long press, Quick Settings interno, Settings Android, fallback YouTube/Chrome, Home Assistant placeholder e timeout 45/65 secondi.
- Riavvio finale PASS con ADB root, SELinux `Permissive`, ADB TCP disabilitato e launcher 0.2.0 persistente.
- Evidenze digitali, webcam e logcat raccolti nella release; merge in `develop` rinviato all'accettazione estetica dell'utente.

## 2026-09-17 — Launcher v0.7.2 Home e Impostazioni

- Creato `codex/home-settings-v0.7.0` dalla linea launcher con Home, Calendario, Meteo e Sveglie; rami stabili e firmware 014 invariati.
- Home aggiornata con orologio e spaziatura caratteri +20%, dock e icone +20%; tap Impostazioni collegato al menu interno.
- Implementate Data e Ora con selettori reali, verifica dell'esito e policy Magisk limitata all'UID Android rilevato; implementata Informazioni con valori runtime per device, Android, firmware, root e ADB.
- Le candidate 0.7.0 e 0.7.1 hanno permesso di individuare e correggere quoting SQL, contenuti tagliati dalla geometria 800×480 e glifo Indietro non renderizzato; la release finale è 0.7.2/code 12.
- Due build e due firme offline della 0.7.2 sono byte-identiche; APK firmato SHA-256 `edcf8341a49c7e58214a10c6f05152ce6516b2631a294010502e1e576728a126` e certificato persistente invariato.
- Installazione ADB PASS; Home, menu, voci disabilitate, Data/Ora, Informazioni e pulsante Indietro verificati con screenshot e webcam.
- Sulla 0.7.2 è stata applicata l'ora corrente con `auto_time=0`, poi ripristinata la modalità automatica con `auto_time=1`; root launcher misurato come autorizzato.
- Riavvio finale PASS: boot completo, ADB `device` e root, code 12, hash APK e policy Magisk persistenti; nessun crash/ANR nei log mirati.
- Evidenze e report: `Nuovo firmware/Fase 2 - Custom launcher/MiiClockLauncher/releases/0.7.2/evidence/`; merge sospeso fino all'accettazione estetica dell'utente.
- Consolidamento: rimosse dal repository le release diagnostiche 0.7.0/0.7.1, dalla VM tutte le build/archive/evidence intermedie e dal vault sei copie di firma ridondanti 0.7.0-0.7.2. Preservati export 0.7.2, rollback 0.6.1, backup fisico, chiavi e firmware 014; vault verificato chiuso.

## 2026-09-17 — Launcher v0.7.4 Impostazioni verticali

- Ricostruite `Data e Ora` e `Informazioni` come liste verticali in stile Settings Android, senza ricerca; Data e Ora contiene soltanto le quattro voci richieste.
- La candidate 0.7.3 ha evidenziato una sovrapposizione della capsula stock sulla prima tile; la 0.7.4 aggiunge il margine protetto e supera screenshot e webcam.
- Picker data/ora aperti; toggle automatico verificato `1→0→1`; fuso applicato `Europe/Samara` e ripristinato `Europe/Rome` dalla UI.
- Le dieci tile Informazioni mostrano valori runtime reali per hardware, Android, firmware, launcher, root e ADB/USB.
- 0.7.4/code 14: doppia build e doppia firma byte-identiche; APK SHA-256 `28d746ebd4f819b612dc117d202ad4655fc764109dc4908d20a1f97645ce7292`.
- Riavvio finale PASS in circa 64 secondi: USB `adb`, adbd running, ADB root, policy Magisk, versione, hash, `auto_time=1` e `Europe/Rome` persistenti.
- Nessun fastboot, flash o intervento su firmware/partizioni; launcher riportato sulla Home e vault chiuso.
- Il 2026-09-18 l'utente ha approvato la 0.7.4; `codex/home-settings-v0.7.0` è stato integrato in `develop` con merge commit `8bc1427`, test host PASS e APK canonico invariato. `main` e `Base_version` non sono stati modificati.

## 2026-09-18 — Launcher v0.8.0 Calendario autonomo

- Creato `codex/calendar-local-v0.8.0` da `develop`; firmware 014, `main` e `Base_version` invariati.
- Collegata la vista Calendario al CalendarProvider AOSP con lettura asincrona, cache mensile, ContentObserver e supporto eventi temporizzati, all-day, notturni e multi-giorno.
- Creato il calendario dedicato `MiiClock` di tipo `LOCAL`, senza Google, rete o dipendenze dal backend privato Assistant.
- Implementato CRUD completo in Impostazioni > Calendario, mantenendo consultiva la vista carosello e isolando le scritture al solo calendario MiiClock.
- Il gate fisico ha corretto due difetti prima dell'accettazione: editor adattato alla finestra reale 800x408 e UPDATE/DELETE spostati dagli URI item all'URI collezione richiesto dal provider X04G.
- Test host PASS: Home 56, Calendar 58, Weather e Alarms; lint release PASS; doppia build e doppia firma byte-identiche.
- APK finale 0.8.0/code 15 SHA-256 `fdcdefdbba2c99097dfe6123bcd550d8c9dcbd63ebe826729aab0725950ba322`, certificato persistente invariato.
- Installazione ADB PASS; CREATE, UPDATE, DELETE, resa nella schermata Calendario e persistenza dopo reboot verificati con screenshot, webcam e query provider.
- Stato finale dispositivo: ADB root attivo, permessi calendario concessi, calendario locale presente e vuoto, nessun flash o intervento su partizioni.

## 2026-09-18 — Launcher v0.9.0 Impostazioni Calendario e Lingua

- Integrata la 0.8.0 in `develop` con merge commit `630aa55`; creato il ramo
  `codex/settings-calendar-language-v0.9.0`, lasciando invariati firmware 014,
  `main` e `Base_version`.
- Aggiunto Impostazioni > Calendario con sole opzioni non legate agli eventi:
  calendari visibili, vista, primo giorno, numeri settimana, fuso e informazioni.
- Primo giorno e numeri settimana sono persistenti e modificano realmente la
  griglia mensile; le query provider restano indipendenti dalla resa scelta.
- Aggiunto il selettore persistente Italiano, English, Deutsch, Español e
  Français; la traduzione completa dei testi resta rinviata.
- Rimosse le icone microfono/Wi-Fi del vecchio header, il relativo monitor e i
  permessi di rete; rimosso anche il margine della capsula dalle Impostazioni.
- La modalità AppOps `deny` è stata scartata perché provocava
  `BadTokenException` nel processo Assistant; la release usa `ignore`, che
  sopprime la grafica mantenendo `AssistantCoreService` attivo e stabile.
- Test host PASS: Home 54, Calendar 63, Weather, Alarms e policy manifest; lint
  release PASS; due build e due firme indipendenti byte-identiche.
- APK finale 0.9.0/code 16 SHA-256
  `938016804294cc455866a859ef6dbf48691df203212c26240a1bf5cac2f8c663`.
- Installazione ADB e riavvio PASS: ADB root, hash, preferenze, AppOp `ignore` e
  processo Assistant persistenti; nessun crash nel gate finale.
- Screenshot digitali e webcam confermano Home senza le due icone, menu
  Calendario e selettore Lingua. Stato finale ripristinato a Italiano, primo
  giorno di sistema e numeri settimana OFF; nessun flash o modifica partizioni.

## 2026-09-25 — Flash fisico e accettazione firmware 015

- Completato da remoto l'ingresso nel bootloader reale tramite Android ADB,
  recovery ADB e `adb shell reboot bootloader`; tre probe `getvar product` e
  le dimensioni delle partizioni hanno superato il preflight.
- Flashati soltanto `vbmeta_system`, `super` sparse e `vbmeta` root, con
  `vbmeta` per ultima; tutti gli invii e le scritture hanno restituito `OKAY`.
- Primo e secondo boot PASS: ADB USB `device`, shell `uid=0(root)`, adbd attivo,
  controller `musb-hdrc`, SELinux `Permissive`, AVB `orange`, nessuna porta
  5555 e nessun erase, format, wipe o rollback.
- Verificati il marker firmware `0.1.3 (015)`, l'APK 0.11.4 esatto in `product`,
  il resolver HOME MiiClock e il ritorno automatico in foreground dopo il
  takeover iniziale di MediaShell.
- Screenshot e webcam mostrano la Home MiiClock; monitoraggio finale PASS 30/30
  per cinque minuti. Il Clock e rimasto sul firmware 015.
- Evidenze redatte e checksum: `Nuovo firmware/Fase 1 - Modifica firmware/reports/device-gate/default-home/20260924-015/`.
- Vault LUKS chiuso dopo la verifica del rollback fisico; backup e chiavi non
  sono stati copiati in Git.

## 2026-09-27 — Launcher v0.15.0 e chiusura Sveglie

- Integrate in `develop` le Impostazioni rapide 0.15.0 già installate e
  verificate fisicamente sul Clock: luminosità, volume, Wi-Fi, microfono e
  conferma di riavvio.
- Conservata la nota che il riavvio effettivo dal pannello sarà eseguito nel
  collaudo unificato della V1; nel gate 0.15.0 è stato scelto `Annulla`.
- Chiuso il punto funzionale Sveglie dopo le prove tecniche 0.6.1 e
  l'accettazione dell'utente sul comportamento corrente.
- CRUD, trigger, suono, Snooze e Stop sono considerati accettati; ricorrenza,
  ripianificazione, persistenza e arresto automatico restano nella regressione
  unificata della V1, non come sviluppo funzionale ancora aperto.

## 2026-09-27 — Launcher v0.16.0 Memoria

- Creato `codex/memory-settings-v0.16.0` da `develop` e aggiunta la pagina
  Android-like `Impostazioni > Memoria` con sei tile verticali.
- RAM totale/disponibile, PSS MiiClock, pressione `lowMemory`, soglia low-memory,
  stato low-RAM e heap limit derivano da API Android reali.
- Il PSS Assistant usa Magisk soltanto per leggere il `TOTAL PSS` Android dei
  processi `com.google.assistant.core`, inclusa la memoria grafica.
- Test host, lint, due build e due firme indipendenti byte-identiche: PASS;
  certificato persistente invariato e vault richiuso.
- Installazione 0.16.0/code 29 PASS; APK installato e candidato hanno SHA-256
  `bff04cbfc45206a22fcfc7b98ac58ef7dda5d234fd266e87947baa13a038ddb1`.
- Due campioni fisici mostrano 980 MiB totali, PSS MiiClock 24,2 MiB, PSS
  Assistant 26,0–26,4 MiB, pressione normale, soglia 144 MiB e heap 128 MiB.
- Screenshot, webcam, scorrimento e controllo crash/ANR PASS. Nessun flash o
  rollback; la 0.16.0 resta installata ed è stata accettata dall'utente.

## 2026-09-27 — Launcher v0.17.0 OTA firmato

- Creato `codex/ota-updates-v0.17.0` da `develop`; il Clock resta sulla 0.16.0.
- Implementato client GitHub Releases fail-closed, manifest RSA firmato,
  controllo package/certificato/hash/versione/firmware e blocco downgrade.
- Aggiunti processo `:updater`, job giornaliero, download atomico, stato
  persistente, backup APK, health check, rollback e boot guard temporaneo.
- Chiave privata creata soltanto nel vault LUKS; pubblica e fingerprint
  versionati; vault richiuso.
- Suite host con 31 controlli OTA e build/lint Android PASS. Audit e piano
  fisico prodotti; nessuna installazione o modifica firmware eseguita.

## 2026-09-27 — Primo OTA reale 0.17.2 → 0.17.3

- Configurato il token GitHub fine-grained nel Credential Manager; revocata la
  vecchia deploy key e rimossi soltanto alias e coppia SSH dedicati.
- Reso pubblico il repository GitHub, contenente soltanto `AI/`, dopo scan
  mirato senza credenziali o chiavi; il PAT non è presente sul Clock.
- La 0.17.1 è stata rilevata e firmata correttamente, ma il download è stato
  bloccato con `APK_SIGNER` prima dell'installazione a causa del PackageManager
  vendor Android 10; launcher 0.17.0 rimasto intatto.
- Implementato il fallback compatibile `GET_SIGNATURES`, mantenendo signer
  unico e fingerprint ufficiale obbligatori; bootstrap 0.17.2/code 32 installato
  via ADB e verificato.
- Costruita 0.17.3/code 33 con due build, due firme APK e due manifest byte-
  identici; release GitHub pubblica riletta anonimamente e firma verificata.
- Test reale PASS: `UPDATE_AVAILABLE`, `DOWNLOADED`, backup rollback 0.17.2,
  installazione watchdog, health check e stato persistente `SUCCESSFUL`.
- Dopo riavvio: APK installato SHA-256
  `5b56f2b53f7ac72473824f64c2333419bed9e59b2eec1bcff0d9ac0fd09b38f8`,
  HOME MiiClock, ADB root, USB `adb`, Wi-Fi e rollback integri; zero crash/ANR.
- Nessun fastboot, flash, wipe o intervento sulle partizioni; vault LUKS chiuso.
