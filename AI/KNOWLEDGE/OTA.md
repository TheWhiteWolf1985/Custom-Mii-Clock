# OTA del launcher MiiClock

## Scopo e confine

Il sottosistema OTA aggiorna esclusivamente `local.miiclock.launcher` come APK
in `/data/app`. Non scrive `product`, `super`, `boot`, `vbmeta`, recovery o
altre partizioni. La copia firmata integrata in `/product` rimane immutata come
fallback estremo.

Provider e transport sono le GitHub Releases pubbliche di
`TheWhiteWolf1985/Custom-Mii-Clock`. GitHub non è una radice di fiducia: ogni
decisione installabile deriva dal manifest MiiClock firmato e dai controlli
locali sull'APK.

## Architettura

- `OtaUpdatesActivity`: sola UI in `Impostazioni > Aggiornamenti`; non scarica e
  non installa direttamente.
- `OtaScheduler` + `OtaJobService`: lavoro differibile con `JobScheduler` nel
  processo separato `:updater`. Il job periodico richiede rete, è persistente e
  può eseguire al massimo un controllo utile ogni 24 ore.
- `OtaReleaseClient` + `OtaHttp`: REST GitHub e download HTTPS con dimensioni
  massime, timeout, redirect limitati e host allowlist.
- `OtaManifest`, `StrictJson`, `OtaCrypto`, `OtaPolicy`: parser fail-closed,
  firma RSA/SHA-256, hash, versioni e compatibilità firmware.
- `OtaApkInspector`: package, versionCode/versionName e certificato Android
  letti prima dell'installazione. Sul vendor Android 10 X04G viene richiesto
  anche `GET_SIGNATURES`: se `SigningInfo` non è popolato per un archivio v2
  non installato, viene usata la vista legacy, continuando a richiedere un solo
  signer e il fingerprint ufficiale fissato.
- `OtaStateStore`: journal JSON schema 1 scritto con `AtomicFile`.
- `OtaUpdateManager`: download, staging, backup, installazione, health check e
  rollback. Il launcher UI può essere terminato senza perdere la transazione.
- watchdog root: resta indipendente dai processi APK durante `pm install`; un
  guard temporaneo Magisk in `/data/adb/service.d` copre anche il riavvio o la
  perdita di alimentazione durante la transazione e si autoelimina al termine.

## Radici di fiducia e chiavi

Le due firme hanno scopi distinti:

1. firma Android APK `miiclock-launcher-v1`, fingerprint SHA-256
   `7340f6343323e0474c4b6afc0e5891aa9e7f5d5176606be82b6124a39631c029`;
2. firma del manifest `miiclock-ota-manifest-v1`, RSA 3072 con
   `SHA256withRSA`, fingerprint SHA-256 della chiave pubblica DER
   `8a386dc9cb0a4982e422141f490ceafea9304fcf61a2356b391dd901d6704b5a`.

La privata del manifest esiste soltanto nel vault LUKS:

`/mnt/miiclock-vault/keys/ota/miiclock-ota-manifest-v1-private.pem`

È root-only `0600`, non viene copiata nel repository né sul Clock. La pubblica
è versionata in
`MiiClockLauncher/keys/miiclock-ota-manifest-v1-public.pem` ed è incorporata
nell'APK come DER Base64 con fingerprint verificato a runtime. Il vault deve
essere montato soltanto per la firma e richiuso immediatamente.

Una futura rotazione non sovrascrive la v1: richiede un nuovo key ID, una
release del launcher che conosca entrambe le pubbliche e solo dopo la
pubblicazione di manifest firmati dalla nuova chiave.

## Formato manifest schema 1

La firma detached copre i byte esatti UTF-8 di `ota-manifest.json`.
Campi ignoti, duplicati, mancanti o di tipo errato causano il rifiuto.

```json
{
  "schema_version": 1,
  "versionName": "0.17.0",
  "versionCode": 30,
  "apk_asset": "miiclock-launcher-0.17.0.apk",
  "apk_size": 123456,
  "apk_sha256": "64-caratteri-esadecimali-minuscoli",
  "package": "local.miiclock.launcher",
  "firmware_min": 15,
  "firmware_max": 15,
  "changelog": "Descrizione sintetica"
}
```

`firmware_min` e `firmware_max` sono inclusivi e confrontati con il numero tra
parentesi nel marker runtime, per esempio `0.1.3 (015)` diventa `15`. Il
fallback compilato per la linea corrente è 15.

## Asset definitivi della GitHub Release

- `miiclock-launcher-X.Y.Z.apk`
- `ota-manifest.json`
- `ota-manifest.sig`

Non sono ammessi path, nomi alternativi o duplicati. Lo script
`tools/prepare_ota_release.py` legge identità e certificato dell'APK, calcola
dimensione/hash, crea JSON deterministico, firma soltanto con la privata
canonica nel vault, verifica la firma e produce `SHA256SUMS`. La release deve
essere non-draft e non-prerelease perché il client usa l'endpoint GitHub
`releases/latest`.

## Regole di accettazione

Una release è scaricabile/installabile soltanto se tutte sono vere:

- firma detached valida con la pubblica MiiClock;
- schema supportato e manifest strettamente valido;
- asset APK presente una sola volta e dimensione GitHub uguale a quella
  firmata;
- firmware corrente compreso nell'intervallo firmato;
- versionCode strettamente superiore a quello installato;
- nome asset canonico, dimensione e SHA-256 esatti;
- package `local.miiclock.launcher`;
- versionName/versionCode interni uguali al manifest;
- un solo signer Android corrente e fingerprint ufficiale MiiClock.

Un errore lascia l'APK corrente intatto. Nessun flag di downgrade viene usato
per l'update normale; `-d` esiste esclusivamente nel percorso rollback verso la
copia verificata salvata prima della transazione.

## Download, installazione e rollback

Il download usa `candidate.apk.part`; dimensione e SHA-256 vengono calcolati
durante lo stream. Solo dopo firma manifest, identità APK, certificato e hash
validi il file è promosso atomicamente a `candidate.apk`. Prima del download e
prima dell'installazione viene verificato lo spazio per candidato, temporanei,
rollback e margine di sicurezza.

Prima di `pm install -r` viene copiata l'APK realmente attiva, poi riletta come
archive e verificata per package, certificato, versionCode e SHA-256. Il
watchdog root installa da `/data/local/tmp`, avvia MiiClock e richiede:

- versionCode target;
- processo launcher vivo;
- resolver HOME MiiClock;
- Activity MiiClock in primo piano.

Il controllo applicativo aggiunge certificato, avviabilità e assenza di crash
registrati dopo l'inizio della transazione. Se il gate fallisce viene eseguito
`pm install -r -d` esclusivamente sulla copia rollback verificata. Il boot guard
temporaneo ripete la decisione dopo reboot; su successo o rollback cancella se
stesso. L'APK in `/product` non viene mai alterato e resta recuperabile tramite
`pm uninstall-system-updates` in un intervento manuale estremo.

## Macchina a stati persistente

Sequenza normale:

`IDLE → CHECKING → UPDATE_AVAILABLE → DOWNLOADING → DOWNLOADED → INSTALLING → PENDING_VERIFICATION → SUCCESSFUL`

Sequenza di errore post-installazione:

`INSTALLING/PENDING_VERIFICATION → ROLLBACK_REQUIRED → ROLLING_BACK → ROLLED_BACK`

`ERROR` contiene sempre stato comprensibile e codice diagnostico minimo. Dopo
reboot, `DOWNLOADING` elimina il `.part`; `INSTALLING` verifica quale versione
sia attiva; `PENDING_VERIFICATION` riprende il health check; uno stato rollback
incompleto riprova soltanto con la copia verificata.

## Threat model

Mitigato:

- GitHub/CDN compromesso o risposta manomessa: firma manifest e hash APK;
- sostituzione APK: package, certificato Android e identità firmata;
- downgrade/replay: versionCode strettamente crescente;
- release per firmware errato: intervallo firmato;
- download parziale/corrotto: file temporaneo, lunghezza e SHA-256;
- morte launcher/updater: processo separato, journal atomico e receiver;
- reboot durante update: journal più boot guard root temporaneo;
- crash nuova versione: health check e rollback automatico.

Rischio residuo esplicito:

- un attaccante con root/ADB fisico può sostituire codice, stato e radici di
  fiducia; l'attuale firmware offre deliberatamente ADB USB root;
- compromissione della privata OTA o della privata Android richiede revoca,
  rotazione e nuova baseline;
- indisponibilità GitHub impedisce il controllo ma non il funzionamento del
  launcher installato;
- `/data` o filesystem gravemente danneggiato richiede il fallback `/product`
  o il runbook di recupero, non un tentativo OTA improvvisato.

## Consenso e profilo Low-RAM

Il check può essere automatico, al massimo ogni 24 ore e solo con rete. Non
esiste polling di rete o servizio permanente. Download e installazione hanno
due conferme utente distinte. La UI osserva solo il journal locale mentre è
visibile. Nessun aggiornamento viene installato automaticamente.

## Collaudo fisico 2026-09-27

Il primo tentativo 0.17.0 → 0.17.1 è stato bloccato correttamente prima
dell'installazione con `APK_SIGNER`: il PackageManager vendor non popolava
`SigningInfo` per l'APK v2 scaricato. La correzione è stata installata come
bootstrap 0.17.2 e il test reale successivo 0.17.2 → 0.17.3 ha superato:

- rilevamento release pubblica e manifest firmato;
- download atomico e verifica di dimensione, SHA-256, package, versione e
  certificato Android;
- backup rollback dell'APK 0.17.2 con hash verificato;
- `pm install` indipendente dal processo launcher, watchdog e health check;
- stato persistente `SUCCESSFUL`, HOME MiiClock e APK installato identico
  all'asset GitHub;
- riavvio completo con versione 33, ADB root, Wi-Fi, HOME e stato OTA integri;
- assenza di crash/ANR e rimozione di candidato, guardia e boot guard.

Il report e le immagini sono in `releases/0.17.3/evidence/`.
