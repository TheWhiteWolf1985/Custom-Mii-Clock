# AI_RUNBOOK

## Regola preliminare

Lavorare su Xubuntu/Linux reale. Prima di qualunque modifica o flash, localizzare gli asset, registrarne dimensioni e SHA-256 e creare copie di lavoro. Non eseguire wipe della stock configurata.

## Accesso alla VM Xubuntu per X04G

- Host LAN: `192.168.1.248`; utente: `codex`.
- Autenticazione: chiave SSH dedicata locale `C:\\Users\\Juri\\.ssh\\id_ed25519_miiclock_xubuntu`; la chiave privata e qualsiasi password restano fuori dal repository.
- Connessione non interattiva: `ssh -i C:\\Users\\Juri\\.ssh\\id_ed25519_miiclock_xubuntu -o BatchMode=yes codex@192.168.1.248`.
- Il clock deve essere assegnato con passthrough USB diretto ed esclusivo alla VM; non usare la catena Windows/Zadig/USB-IP.
- Su questa VM, `fastboot devices` enumera il device come utente, ma le operazioni di protocollo richiedono `sudo fastboot ...`.

## Inventario iniziale (Xubuntu)

```bash
find /home /root -type f \( \
  -name "super_stock_repacked.img" -o \
  -name "super.img" -o \
  -name "new-boot-fixed.img" -o \
  -name "boot.img" -o \
  -name "Fully-Kiosk-Browser.apk" \
\) 2>/dev/null
```

Per ogni file selezionato, registrare percorso assoluto, `stat`, `sha256sum` e ruolo presunto senza modificare l'originale.

## Diagnostica dispositivo già nota

```bash
adb shell id
adb shell getprop sys.boot_completed
adb shell dumpsys window | grep -E "mCurrentFocus|mFocusedApp"
adb shell cmd package resolve-activity --brief android.intent.action.MAIN --category android.intent.category.HOME
adb logcat -d -b all
adb shell dmesg
```

## Gate fastboot verificato su Xubuntu

Il report `Nuovo firmware/Fase 1 - Modifica firmware/reports/device-gate/20260912-132829-fastboot-xubuntu.txt` documenta il gate read-only riuscito. Il bootloader risponde a `sudo fastboot getvar`; è `unlocked: yes`, `secure: no`, usa partizioni dinamiche (`super`) e non ha slot A/B (`slot-count: 0`; `current-slot` non supportato). Prima di qualsiasi flash resta obbligatoria la validazione separata della procedura di recovery.

## Toolchain Xubuntu congelata

- Root operativo attuale: `/opt/miiclock-toolchain`.
- MTKClient `v2.1.4.1`, commit `5a863eece86fcaa97cb8325cf747e0aae3c307e4`.
- AOSP `avbtool 1.2.0`, tag `android-12.0.0_r34`, commit `1f12696affb6f715964dd5495073d61b1fde76fd`.
- AOSP `fec`, tag `android-10.0.0_r47`, commit `1be00e563e8d4e65f94dc85ed68b893010d2070d`; binario installato `/opt/miiclock-toolchain/bin/fec`.
- Lock, sorgenti e dipendenze offline: `Nuovo firmware/Fase 1 - Modifica firmware/toolchain/`.
- `sudo` non interattivo è configurato per l'utente dedicato `codex`; le password restano fuori da file, report e Git.
- I report di comandi devono passare da `tools/run_and_redact.py`; i manifest fisici da `tools/validate_operation_manifest.py`.

## Ingresso BROM e letture MTKClient

- Il metodo verificato per X04G è: clock scollegato, avviare dalla console VM il comando MTKClient della lettura prevista, quindi ricollegare tenendo premuto `VOL+`.
- L'operatore esegue manualmente il comando dalla console per osservare handshake e avanzamento; Codex non avvia il passaggio BROM.
- Non eseguire prima `mtk printgpt`: quando termina lascia il device nel Download Agent `0e8d:2001`, che un nuovo processo MTKClient non riesce a riutilizzare.
- Dopo una lettura riuscita e con `ModemManager` inattivo, tentare la lettura successiva riutilizzando il DA `0e8d:2001`; scollegare e ripetere l'ingresso BROM soltanto se MTKClient lo richiede.
- `preloader` non compare nella GPT: acquisirlo con `mtk dumppreloader --filename <percorso>` durante un nuovo ingresso BROM, non con `mtk r preloader`.
- Il dump fisico del preloader e riuscito il 2026-09-12: 169132 byte, SHA-256 `84f621317857a28f5566919c3e0770004ecba4f1583a12477b129a2fee3aa530`, intestazione MediaTek `FILE_INFO` e marker X04G/MT8167. Il log grezzo con identificativi resta esclusivamente nel vault.
- Per il full-flash usare direttamente `sudo /opt/miiclock-toolchain/bin/mtk rf /mnt/miiclock-vault/backups/x04g-physical-20260912-001/full-flash.bin` con il clock inizialmente scollegato.
- `ModemManager.service` deve restare inattivo e disabilitato: durante il primo tentativo ha coinciso con `USBError(16, Resource busy)` alla riconnessione del DA ad alta velocità.
- Un exit code zero non basta: l'output deve esistere, avere la dimensione esatta dichiarata dal manifest ed essere privo di errori nel report post-operazione.
- Il boot fisico acquisito il 2026-09-12 contiene l'overlay dello script storico ADB/root (`init.adb.rc`, Magisk e rootshell) e non supera la verifica del proprio hash descriptor AVB stock. Deve essere conservato come rollback dell'attuale stato funzionante, non presentato come boot stock pulito.
- Il `boot.bin` community ha la stessa chiave, descriptor e payload AVB attesi e supera `avbtool verify_image`; inoltre gli altri componenti tecnici confrontati sono byte-identici alla copia fisica. Puo essere usato soltanto come base stock candidata della ricostruzione, da rifirmare nella catena del progetto, mai come rollback al posto del backup fisico.

Per `avbtool` il tag AOSP Android 10 non viene usato: la relativa versione richiede Python 2. La revisione AOSP selezionata gira con Python 3 e ha letto correttamente il `vbmeta` stock di riferimento come AVB minimo 1.0, algoritmo `SHA256_RSA2048`, rollback index 0 e flags 0.

Il tool `fec` proviene invece dai sorgenti AOSP Android 10 congelati nel repository. Il port host conserva encode/decode/print-size e rimuove soltanto due modalita diagnostiche non richieste da `avbtool`. Il gate 008 ha rigenerato con roots=2 i dati, le hashtree e il FEC di product/system/vendor byte-identici allo stock; non usare `--do_not_generate_fec`.

La verifica offline completa deve includere `vbmeta --follow_chain_partitions`, i vbmeta secondari, boot, recovery, dtbo e le immagini logiche estratte dalla super fisica. I warning sulle chiavi attese non passate non sostituiscono la verifica: per la catena di progetto fornire esplicitamente ogni chiave pubblica prevista e richiedere exit code zero.

## Quota operativa Codex

- Ricontrollare la finestra di 5 ore prima di ogni gate e immediatamente prima di una scrittura.
- Per decisione esplicita dell'utente del 2026-09-12, continuare fino al 5% residuo e non iniziare nuove operazioni fisiche sotto tale soglia.
- Al 5% fermarsi al primo checkpoint sicuro e attendere il reset o una nuova decisione esplicita.
- Non consumare crediti di reset senza una conferma esplicita dell'utente per il singolo credito.

## Volume cifrato obbligatorio

Poiché non è disponibile spazio hypervisor per un secondo disco virtuale, l'utente ha autorizzato il 2026-09-12 un contenitore-file LUKS2 sul disco esistente. Il backup fisico e la chiave AVB privata risiedono soltanto nel vault montato in `/mnt/miiclock-vault`.

- Container: `/var/lib/miiclock-vault/miiclock-vault.luks`, sparse 16 GiB, mode `0600`.
- Header backup e chiave di unlock: root-only, fuori dal repository; il valore della chiave non appare nei report.
- Mapper: `/dev/mapper/miiclock_vault`; label filesystem: `MIICLOCK_VAULT`.
- Mount obbligatorio: `nosuid,nodev,noexec`; apertura e chiusura tramite `tools/vault_mount.sh` e `tools/vault_unmount.sh`.
- Il wrapper richiede almeno 2 GiB liberi per aprire il vault; ogni gate deve inoltre verificare separatamente lo spazio richiesto dai propri output prima di crearli.
- Il vault deve risultare chiuso quando non è in uso.

La chiave e il contenitore risiedono sullo stesso disco: questa soluzione fornisce separazione logica e impedisce commit accidentali, ma non protegge da una compromissione root della VM. Il server homelab è considerato trusted per decisione dell'utente.

## Chiave AVB di progetto

- ID attivo: `x04g-project-avb-v1`, RSA 2048, uso `SHA256_RSA2048`.
- Privata: `/mnt/miiclock-vault/keys/avb/x04g-project-avb-v1-private.pem`, root-only mode `0600`; non copiarla fuori dal vault.
- Pubbliche e fingerprint: `Nuovo firmware/Fase 1 - Modifica firmware/keys/`.
- La prima ricostruzione usa la stessa chiave per root e catene, mantenendo rollback index 0 e le location stock 1/2/3/4.
- Non sovrascrivere o rigenerare la chiave v1 se esiste; una rotazione usa un nuovo ID e un gate dedicato.

Il builder riproducibile è `tools/build_project_avb_chain.py`. Deve essere eseguito come root esclusivamente con input nel vault e rifiuta output preesistenti, hash stock inattesi, chiavi pubbliche diverse o chiavi private fuori da `keys/avb`. Il gate storico 010 ha prodotto due volte immagini byte-identiche e ha verificato la root con tutte le `expected_chain_partition` nel formato AVB pubblico; i suoi output intermedi sono stati rimossi dopo il consolidamento della 014.

## Test offline obbligatorio prima del flash

1. Validare entrambe le copie GPT del full-flash fisico, incluse CRC di header e array delle entry, con `tools/extract_gpt_partitions.py`.
2. Estrarre senza mount soltanto le partizioni tecniche selezionate, verificando preventivamente lo SHA-256 del full-flash e producendo un manifest degli output nel vault.
3. Partire dalla copia di lavoro di `super` fisica verificata.
4. Unpack → repack senza cambiamenti tramite gli strumenti effettivamente presenti.
5. Confrontare layout/partizioni, dimensioni e hash; un hash diverso è atteso dopo repack ma deve essere spiegabile.
6. Registrare i comandi esatti e non flashare finché il risultato non è analizzato.

## Flash e recovery

I comandi di flash non sono autorizzati in automatico da questo runbook. Per ogni iterazione devono essere definiti asset, hash, backup, obiettivo e criteri di stop. L'unico rollback autorizzato è il manifest fisico 014 basato sulle copie hashate nel vault; non usare nomi o immagini storiche non inventariate.

Sul X04G l'enumerazione `18d1:4ee0` e l'output di `fastboot devices` non bastano a dimostrare un bootloader operativo. Dalla recovery bisogna selezionare esplicitamente **Reboot to bootloader**: una modalita fastboot raggiunta in altro modo puo enumerarsi ma lasciare `getvar` bloccato sulla prima richiesta USB bulk. Prima di ogni gate richiedere sempre un `getvar product` riuscito.

Ogni scrittura richiede inoltre un manifest `ready`, già committato e pushato, validato con hash degli artefatti e `--require-pushed`. La conferma runtime deve essere la frase esatta `flash ora`. `vbmeta` root è sempre l'ultima partizione scritta nei manifest firmware e rollback.

### Gate storico `logo` 011

- Gli artefatti e i manifest 011 sono stati rimossi dopo il PASS. L'originale resta nel backup fisico come `/mnt/miiclock-vault/backups/x04g-physical-20260912-001/partitions/logo.img`, 8388608 byte, SHA-256 `82ecd4ac0ca574f00b99494fdb782e5e047e273eb9930f6a9a007fe5214aca6d`.
- Sentinella: stesso contenuto salvo offset `0x007fffff`, nel padding dopo il record finale `cert2`, cambiato da `0x00` a `0xa5`; SHA-256 `7e790f5e5e6e2733e5bed35908b4fa0dcad8f88ef7b3a1e13ebe03c860c5a6aa`.
- `tools/ensure_fastboot.py` gestisce sia Android/ADB sia un device gia in fastboot senza scritture persistenti; usa sempre `sudo fastboot` per il protocollo.
- `tools/wait_for_android_boot.py` attende al massimo 600 secondi e non stampa identificativi.
- Dopo ogni flash, leggere in sola lettura `adb shell sha256sum /dev/block/by-name/logo`; l'hash deve coincidere prima con la sentinella e poi con l'originale.
- Verificare inoltre `device_provisioned=1`, `user_setup_complete=1` e un frame webcam leggibile acquisito con `sudo fswebcam`; l'utente SSH `codex` non appartiene al gruppo `video`.
- I due manifest formano un singolo gate: il restore originale segue immediatamente la raccolta prove della sentinella ed e il percorso obbligatorio anche se il primo boot fallisce.
- Se Android non torna, ristabilire fastboot senza wipe e applicare il manifest restore. Non usare MTKClient salvo fallimento documentato del percorso fastboot.

### Candidato visibile 012

- Le sei scritture `boot`, `recovery`, `vbmeta_system`, `vbmeta_vendor`, `super` sparse e `vbmeta` root sono terminate con `OKAY`; `vbmeta` e stata scritta per ultima.
- Android si e avviato senza erase o wipe, ma ADB e risultato `unauthorized`; il gate runtime ha quindi eseguito lo STOP previsto.
- Causa: il boot pulito del progetto ha sostituito il boot fisico storico modificato da Magisk/script ADB-root. Non tentare di recuperare l'accesso copiando chiavi private tra host.
- Gli artefatti 012 sono stati rimossi dopo essere stati incorporati nella 014. Il rollback corrente è `manifests/operations/20260914-x04g-physical-rollback-014.json`, basato sulle immagini fisiche pre-progetto e pronto senza wipe.

### Candidato v0.1.1 open-root ADB 013

- **Stato: FALLITO E CONGELATO. Non riflashare e non riusare `system`, `super` o `vbmeta_system` 013.**
- Obiettivo cliente: ADB USB immediato da un host mai autorizzato e `adb shell` come `uid=0(root)`.
- Implementazione: `system/etc/prop.default` con `ro.secure=0`, `ro.adb.secure=0`, `ro.debuggable=1`, `persist.sys.usb.config=adb`; `system/etc/init/miiclock-adb.rc` mantiene il trasporto e riavvia `adbd` al boot.
- Vincoli: nessun Magisk, netcat, rootshell di rete, ADB TCP, `setenforce`, erase, wipe o bypass AVB. SELinux deve restare `Enforcing`.
- `product` conserva il launcher v0.1.0 non-HOME del candidato 012. `super` mantiene geometry, metadata LP, dimensione totale e tutti i byte fuori dalle extent `product` e `system`.
- Hash `system.img`: `9eb52b8fa358c41d90ab4b01edbe365731368657a3f0e4e9d759ca5fef48b1dd`.
- Hash `super.sparse.img`: `5bc1f3ee05614a1f68b8acdc79147bd43293c7b8620d630d4c4894b697b7bf87`.
- Hash `vbmeta_system.img`: `43c42f9b076c13b6e72e6de48783637ee7d4df867c7b4eafcc500d6a74d71a41`; hash `vbmeta.img`: `0dd23b7dad165122c0759532d3d64fcdde85a84a50e169d3521c9c76e034e9ec`.
- Il manifest e gli artefatti 013 sono stati rimossi. Gli hash riportati qui restano una denylist permanente per i validatori futuri.
- Esito fisico: Android ha iniziato/completato l'avvio, ma l'interfaccia USB Android/ADB non si e enumerata. Fastboot e rimasto il percorso di recupero. I gate offline non avevano quindi dimostrato il comportamento del gadget USB runtime.
- Causa operativa: la modifica autonoma di `system` non ha riprodotto la catena init/ramdisk che nel boot fisico Magisk governava correttamente ADB e USB. La 014 elimina integralmente questa modifica ripristinando il `system` stock della 012.

### Candidato v0.1.2 Magisk e launcher visibile 014

- Boot ammesso: esclusivamente `/mnt/miiclock-vault/backups/x04g-physical-20260912-001/partitions/boot.img`, 16777216 byte, SHA-256 `112b8952c6a2ae833e468fd40bff654210f588192a0c589ef1f017fcc8d6a756`.
- Il builder inventaria `.backup`, `overlay.d`, `init.adb.rc`, `magisk32.xz`, `rootshell.sh` e `stub.xz`, rimuove solo il vecchio footer AVB e firma con la chiave progetto. Kernel, ramdisk e payload precedente al footer devono restare byte-identici.
- Sono obbligatorie due build indipendenti con `boot.img` identiche. Il validatore rifiuta gli hash 013, espande la `super` sparse autonoma della 014, verifica le extent stock `system`/`vendor`, `product` con APK e la catena AVB completa.
- Componenti flashabili, nell'unico ordine ammesso: `candidates/x04g-magisk-visible-launcher-014/avb-chain/boot.img`, `vbmeta_system.img`, `super-build/super.sparse.img` e `vbmeta.img`. Recovery, dtbo, vbmeta_vendor, userdata, metadata, logo, lk e preloader non vengono scritti.
- Policy runtime: Magisk permanente, ADB USB immediato con `uid=0(root)`, SELinux `Permissive`, ADB TCP/5555 disabilitato, AVB attiva con flags zero.
- Fastboot valido: Recovery -> selezione esplicita **Reboot to bootloader**, poi tre `sudo fastboot getvar product` consecutivi con `mico_x04g`.
- Dopo l'eventuale provisioning avviare il risultato visibile con `adb shell am start -W -n local.miiclock.launcher/.MainActivity`; la webcam deve mostrare titolo, clock, data e `firmware gate visibile`.
- Il flash e vietato finche builder/validatore/manifest/hash non sono committati e pushati e il rollback fisico non e stato riletto dal vault. Al primo comando diverso da `OKAY`, non riavviare.
- Esito fisico 2026-09-14: PASS. `boot`, `vbmeta_system`, `super` e `vbmeta` sono state le sole partizioni scritte; tutte hanno restituito `OKAY`, Android ha completato due boot e non e stato eseguito alcun wipe.
- Baseline runtime provata: ADB USB immediato anche con una chiave host mai autorizzata, shell `uid=0(root)`, `magiskd` attivo, `musb-hdrc`, SELinux `Permissive`, AVB `orange` con verity `enforcing`, ADB TCP vuoto e zero listener 5555.
- Evidenza UI: launcher avviato con `adb shell am start -W -n local.miiclock.launcher/.MainActivity`, pagina Applicazioni, Settings e ritorno stock verificati via webcam. Report e immagini sono in `reports/device-gate/magisk-visible-launcher/20260914-014/`.
- Consolidamento 2026-09-14: la 014 e autonoma nel repository e nel vault. `validate_magisk_candidate.py` non accetta piu un percorso 012; il manifest flash verifica 4/4 artefatti locali e il rollback fisico verifica 6/6 artefatti nel vault.

### Release v0.1.3 HOME predefinita 015

- Candidato canonico: `candidates/x04g-default-home-015/`; integra il Launcher
  0.11.4 in `product`, conserva `system` e `vendor` stock e riusa la catena
  tecnica funzionante 014. Le sole partizioni scritte sono state
  `vbmeta_system`, `super` e `vbmeta`, in quest'ordine.
- Esito fisico 2026-09-25: PASS. Ogni flash ha restituito `OKAY`, Android ha
  completato due boot, ADB USB root e Magisk sono rimasti attivi, AVB e
  risultata `orange`, ADB TCP e rimasto disabilitato e non e stato eseguito
  alcun wipe o rollback.
- Il marker runtime e `MIICLOCK_FIRMWARE_VERSION=0.1.3 (015)`; l'APK in
  `/product/app/MiiClockLauncher/MiiClockLauncher.apk` deve avere SHA-256
  `2c91ca2bd73932640c4293fd5f60afbc4c6a82d35e45dc148b5224c76a1b8db5`.
- MiiClock risolve l'intent HOME. MediaShell puo apparire brevemente al boot;
  il correttore one-shot del launcher riprende il foreground dopo la sua
  finestra di avvio. Il gate finale ha superato 30 controlli su 30 in cinque
  minuti.
- Percorso remoto provato senza presenza fisica: da Android eseguire
  `adb reboot recovery`, attendere che la recovery esponga ADB, quindi usare
  `adb shell reboot bootloader`. Il semplice `adb reboot bootloader` dalla
  recovery e stato ignorato su questo firmware. Ogni invocazione ADB in uno
  script ricevuto via stdin deve usare `</dev/null`, altrimenti puo consumare
  le righe successive dello script.
- Anche nel percorso remoto la scrittura resta vietata finche tre
  `sudo fastboot getvar product` non restituiscono `mico_x04g`; la sola
  enumerazione USB non e un gate sufficiente.
- Prove redatte: `reports/device-gate/default-home/20260924-015/`. Il firmware
  015 resta installato; il rollback fisico 014 rimane disponibile nel vault.

## Branch e promozione release

- `Base_version` e immutabile e identifica la baseline v0.1.2 fisicamente verificata.
- Ogni nuova modifica nasce da `develop` su un feature branch dedicato; `Base_version` resta il riferimento immutabile di recupero.
- Una feature accettata viene unita in `develop`; `main` viene aggiornato soltanto quando una nuova baseline e stabile, documentata e fisicamente verificata.
- Non riscrivere `Base_version`; per una nuova baseline creare un nuovo tag e una decisione esplicita.
- Baseline approvata 2026-09-19: firmware v0.1.2/014 e launcher APK 0.10.1
  installato separatamente in `/data/app`. La mappa degli artefatti e delle
  prove è in `Nuovo firmware/CURRENT_RELEASE.md`. La promozione di `main`
  non ricostruisce `super` e non modifica il Clock.

## Comandi/azioni da non ripetere come soluzione

- Non aspettarsi persistenza da `launchOnBoot`, `monkey`, `force-stop`, `set-home-activity`, RoleManager HOME o `/data/adb/service.d`.
- Non provare a disabilitare `com.google.assistant.core`: è risultato protected package.
- Non usare `erase userdata`, `erase metadata`, factory reset o driver Windows/Zadig nell'attuale percorso.

## Aggiornamenti launcher tramite ADB

- Dal firmware 014 il launcher in `/product` puo essere aggiornato senza
  fastboot usando un APK con lo stesso package e lo stesso certificato.
- Prima dell'installazione verificare hash, firma, versionCode, ADB `device`,
  `sys.boot_completed=1`, shell root, display 800x480 e copia base in `/product`.
- Comando approvato per la 0.2.0: `adb install --no-streaming -r <APK>`; il
  risultato deve essere `Success` e il code path attivo deve passare a
  `/data/app`.
- Rollback applicativo: `adb shell pm uninstall-system-updates
  local.miiclock.launcher`; non tocca firmware, dati generali o partizioni e
  ripristina la copia 0.1.0 in `/product`.
- Il launcher resta non-HOME e va avviato con `adb shell am start -W -n
  local.miiclock.launcher/.MainActivity`.
- Sul Clock il bordo inferiore resta gestito dai componenti stock: la prova
  Meteo 0.4.1 mostra il pannello luminosita/volume e `dumpsys window` vede
  l'overlay `com.google.assistant.core` di 800x12 px ancorato `BOTTOM`, tipo
  `APPLICATION_OVERLAY`. Non e stata dimostrata la classe interna del pannello
  ne un'interfaccia pubblica. Non disabilitare Assistant/SystemUI per aggirarlo.
- La webcam dichiara al driver soltanto 640x480: conservare il log del tentativo
  1280x720 e acquisire la prova utile con `-r 640x480 -S 8`.

### Launcher Calendario 0.3.1 — 2026-09-15

- Ramo di lavoro: `codex/calendar-screen-v0.3.0`, derivato dal ramo Home 0.2.0. Il firmware 014 e i rami stabili non sono stati modificati.
- La prima build 0.3.0 ha superato installazione e gesture, ma aveva soltanto un placeholder per il selettore mese/anno. È stata superata dalla 0.3.1, che include un selettore interattivo e il dettaglio datato.
- Release canonica: `Nuovo firmware/Fase 2 - Custom launcher/MiiClockLauncher/releases/0.3.1/`; APK firmato SHA-256 `38fb33a421d1356057ddbb7a67cba37612775cfff8df197fcb206d5f5fe86a3d`, certificato invariato `7340f6343323e0474c4b6afc0e5891aa9e7f5d5176606be82b6124a39631c029`.
- Installazione già eseguita con `sudo /opt/miiclock-toolchain/android-sdk/platform-tools/adb install --no-streaming -r /home/codex/miiclock-calendar-031-release/MiiClockLauncher-0.3.1.apk`; `Success`, versionCode 4, code path `/data/app`.
- Avvio da qualunque host ADB collegato: `adb shell am start -W -n local.miiclock.launcher/.MainActivity`. Il launcher resta non-HOME.
- Sul device il Calendario si apre sempre su oggi, il titolo offre selezione mese/anno, tap giorno fuori mese aggiorna mese e Agenda, secondo tap apre dettaglio consultivo, swipe mese e edge-swipe restano distinti. A 45 s è ancora aperto; oltre 60 s torna alla Home.
- Dopo reboot normale: ADB `device`, `uid=0(root)`, `sys.boot_completed=1`, versionCode 4 e APK in `/data/app` con hash/certificato identici. Evidenze e report: `releases/0.3.1/evidence/DEVICE_ACCEPTANCE.md`.
- Nessun provider eventi ancora collegato: `Calendario non disponibile` è il fallback intenzionale, non un crash. Gli eventi demo del mockup non vanno inseriti come dati reali.
- `com.google.assistant.core` ha un overlay stock `APPLICATION_OVERLAY` ancorato in alto a sinistra, che copre parzialmente il titolo del mese. Non disabilitarlo in questa fase: Assistant/provisioning restano fuori dalla modifica launcher.
- Dopo commit/push del report sono stati rimossi soltanto i build staging 030/031, gli archivi di trasferimento, le copie di screencap/preflight sulla VM e le quattro copie di firma ridondanti in `vault/candidates/miiclock-launcher-030*` e `-031*`. Restano nel repository gli APK/report/evidenze e sulla VM gli export pubblici 030/031 per rollback; full-flash, backup partizioni, chiavi private e firmware 014 non sono stati toccati. Vault richiuso.

### Launcher Meteo 0.4.1 — 2026-09-15

- Ramo: `codex/weather-screen-v0.4.0`, derivato dal ramo Calendario gia verificato per conservare Home/Calendario non ancora promossi; `develop`, `main`, `Base_version` e firmware 014 non sono stati modificati. La 0.4.0 e stata installata e ha provato Home, Meteo e cinque giorni, ma il layout impostazioni si sovrapponeva e le unita erano accoppiate: release diagnostica, non baseline.
- 0.4.1/versionCode6 e la release corretta, APK canonico `Nuovo firmware/Fase 2 - Custom launcher/MiiClockLauncher/releases/0.4.1/MiiClockLauncher-0.4.1.apk`, SHA-256 `ad2b968da22393114880da28f2b05186e569e120bfe6cb297ee5eb46d0eea223`, cert v2 uguale alla base. Due build/firme offline indipendenti sono byte-identiche; vault LUKS chiuso.
- Installazione sulla VM solo tramite `sudo /opt/miiclock-toolchain/android-sdk/platform-tools/adb install --no-streaming -r /home/codex/miiclock-weather-041-release/MiiClockLauncher-0.4.1.apk`: `Success`, `/data/app`, avvio manuale `am start -W` con `Status: ok`. Rollback applicativo stabile 0.3.1 ancora disponibile sulla VM, senza fastboot.
- Vista: card corrente con sei metriche/ultimo update e card di **cinque giorni**; le ore del mockup sono espressamente scartate. Cinque touch target giorno, refresh card, editor localita singola, long press impostazioni senza overlap, unita temperatura/vento separate, intervallo 15/30/60, carosello, header→Calendario, Quick Settings dall'alto e pannello stock dal basso verificati via ADB/screencap/webcam. Timeout65s torna Home.
- Dopo reboot: ADB `device`, root, `sys.boot_completed=1`, `sys.usb.state=adb`, APK `/data/app` con hash e certificato identici; webcam finale 640x480, richiesta1280x720 restituisce comunque 640x480. Report e foto in `releases/0.4.1/evidence/DEVICE_ACCEPTANCE.md`; non unire in `develop` finche l'utente non accetta la fotografia.
- Inventario stock: `com.google.assistant.launcher` contiene `WeatherComplicationView` e cache privata `volley`, `com.google.assistant.core` e attivo, ma non e stata provata un'API Meteo esportata/stabile. L'APK usa un `WeatherRepository` astratto con fallback `--`: nessuna previsione dimostrativa spacciata per reale. Account Google, cache privata e provider esterni non vengono toccati. Dati reali, offline con cache fisica e condizioni severe restano non provati sul device.
- Il Clock oggi mostra 15 dicembre 2021: date dei cinque giorni derivano dall'orologio del dispositivo. Non alterare ora/provisioning come workaround di questa UI. L'overlay Assistant superiore sinistro preesistente resta protetto e parzialmente copre l'header.
- Consolidamento dopo push: log build A/B, lint, APK, manifest di firma/hash, report e foto sono versionati nella 0.4.1. Sono stati eliminati soltanto i build/evidence staging 040/041, i due archivi di trasferimento locali/VM, le copie APK Assistant inventariate, lo script di firma trasferito, l'export VM della 0.4.0 difettosa e le quattro copie di firma ridondanti in `vault/candidates/miiclock-launcher-040*`/`-041*`. Restano la release 0.4.1 nel repository e l'export pubblico VM `/home/codex/miiclock-weather-041-release/`; rollback 0.3.1, backup fisico, firmware stock/014 e chiavi private non sono stati toccati. Vault richiuso.

### Launcher Home e Impostazioni 0.7.2 — 2026-09-17

- Release canonica: `Nuovo firmware/Fase 2 - Custom launcher/MiiClockLauncher/releases/0.7.2/`; APK SHA-256 `edcf8341a49c7e58214a10c6f05152ce6516b2631a294010502e1e576728a126`, versione 0.7.2/code 12 e certificato v2 persistente invariato.
- Installazione autorizzata solo via `adb install --no-streaming -r`; nessun fastboot o intervento su partizioni. Export VM: `/home/codex/miiclock-home-settings-072-release/`.
- La Home usa orologio e spaziatura +20%, dock e icone +20%. L'icona Impostazioni apre il menu interno con Data e Ora, Lingua, Aspetto e Informazioni; Lingua e Aspetto restano disabilitate.
- Data e Ora non possiede `SET_TIME`: usa Magisk tramite una policy limitata all'UID reale di `local.miiclock.launcher`. Configurare/verificare esclusivamente con `configure_launcher_root_policy.sh`; non hardcodare l'UID e non autorizzare altri package.
- La policy Magisk `2` significa allow. Lo script richiede device ADB pronto, shell root, `/sbin/magisk`, UID applicativo valido e rilettura della riga; `remove` cancella soltanto la policy dell'UID corrente del launcher.
- Informazioni deve sempre misurare a runtime root firmware (`/sbin/su -v`), root launcher (`su -c id -u`), servizio ADB e `sys.usb.state`; non sostituire questi probe con stringhe fisse.
- La capsula stock Assistant occupa l'angolo superiore sinistro: gli header delle Activity interne partono dopo tale area. Non disabilitare o modificare Assistant per rimuoverla.
- Gate fisico PASS: scrittura manuale dell'ora e `auto_time=0`, ripristino `auto_time=1`, riavvio, ADB root, hash APK, policy persistente e UI webcam verificati. Report in `releases/0.7.2/evidence/PHYSICAL_TEST_REPORT.md`.
- Rollback applicativo stabile: rimuovere prima la sola policy del launcher, eseguire `pm uninstall-system-updates local.miiclock.launcher`, quindi reinstallare la 0.6.1 se necessario. Non usare `pm clear`, downgrade diretto o wipe.

### Impostazioni verticali 0.7.4 — 2026-09-17

- Release canonica: `Nuovo firmware/Fase 2 - Custom launcher/MiiClockLauncher/releases/0.7.4/`; APK SHA-256 `28d746ebd4f819b612dc117d202ad4655fc764109dc4908d20a1f97645ce7292`, versione 0.7.4/code 14.
- `Data e Ora` deve contenere esclusivamente quattro tile verticali: data, ora, fuso orario e ora automatica con toggle. Non aggiungere ricerca o testi permanenti ulteriori.
- Il fuso è selezionato dall'elenco `ZoneId` e applicato via root con `auto_time_zone=0`, `persist.sys.timezone` e broadcast; validare sempre il valore riletto.
- `Informazioni` contiene dieci tile verticali scorrevoli e continua a usare esclusivamente probe runtime per root e ADB/USB.
- La lista deve iniziare 22 px sotto il contenuto standard perché la capsula stock scende oltre l'header; la 0.7.3 priva del margine è superata e non va installata.
- Gate fisico PASS: editor, toggle reale, cambio/ripristino fuso, screenshot, webcam, valori informativi, reboot, hash e policy. Report: `releases/0.7.4/evidence/PHYSICAL_TEST_REPORT.md`.
- Stato finale: `auto_time=1`, `Europe/Rome`, USB `adb`, adbd running, root/policy attivi, Home in primo piano e vault chiuso.
