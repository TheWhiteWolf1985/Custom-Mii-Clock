# Release candidate corrente — MiiClock X04G

**Solo Xiaomi Mi Smart Clock X04G (`mico_x04g`). Pre-release per tester.**
Non LX04 e non una versione stabile universalmente qualificata.

| Componente | Versione | Download ufficiale |
| --- | --- | --- |
| Installer Linux amd64 | **1.5.2** | [installer-v1.5.2](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2) |
| Firmware | **0.1.20 (032)** | [firmware-v0.1.20](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/firmware-v0.1.20) |
| Launcher integrato e APK separato | **0.26.5 / versionCode 58** | [launcher-v0.26.5](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) |

Sono tre release coordinate, non tre firmware alternativi. L'installer è vincolato
alla coppia032/0.26.5. L'APK separato e quello incorporato sono byte-identici.
Non mescolare questi file con RC1/RC2 o altri manifest.

## Cosa scaricare

### Prima installazione pulita da firmware stock

1. Leggere la [guida installer](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/15-MiiClock-Installer-Linux),
   i requisiti Linux, i rischi e la procedura di backup personale.
2. Scaricare `miiclock-installer-1.5.2-amd64.deb` con manifest/firma e checksum
   della release installer. La chiave pubblica ufficiale è in `keys/`.
3. Il firmware viene scaricato dal tag032 dall'installer. Per importarlo offline
   scaricare insieme `miiclock-x04g-firmware-0.1.20.zip`, `firmware-manifest.json`
   e `firmware-manifest.sig` dalla release firmware.
4. Usare un PC Linux **fisico** compatibile. La GUI guida backup, sblocco e flash;
   non servono i sorgenti delle dipendenze per la normale installazione.
5. La prima installazione pulita richiede **reset manuale in Recovery dopo il flash**:
   cancella account, configurazione e dati. Mai reset preliminare o riuso del backup
   di un altro Clock. Nessun reset automatico nascosto.

Il `.deb` aggiorna lo stesso programma: non disinstallare preventivamente una
versione precedente; chiudere la GUI e attendere la fine delle operazioni attive.
La compilazione e i test sono separati dall'installazione dell'utente.

### Aggiornamento del solo launcher

La release APK contiene `miiclock-launcher-0.26.5.apk`, `ota-manifest.json`,
`ota-manifest.sig` e `SHA256SUMS`. Non flasha il firmware e non richiede reset.
Se il firmware032 contiene già0.26.5/58, il launcher è già a questa versione.

Il manifest firmato ammette firmware027–032, ma i fix nativi bootstrap/HOME
richiedono032 e non sono introdotti dal solo APK. La prima migrazione OTA da
versioni con watchdog legacy non ha ancora collaudo fisico completo.

Questa APK è una **prerelease**: il controllo integrato che usa `releases/latest`
non la proporrà automaticamente. Il download manuale è disponibile ai tester;
non inventare una funzione di importazione manifest nella UI. Vedere
[OTA e distribuzione](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/08-OTA-e-Distribuzione-APK).

## Chiarimento sul README nello ZIP032

Lo ZIP firmware firmato è stato conservato immutato. Il suo README interno è una
nota storica di preparazione: i riferimenti a installer vincolatoRC1, divieto
assoluto di factory reset e bootloader già predisposto alle chiavi del progetto
**non descrivono la procedura pubblica corrente di prima installazione**.

Per032 seguire la guida installer1.5.2 e le note della release: backup personale,
sblocco verificato, flash completo autorizzato e **reset manuale solo dopo il flash**
nel percorso pulito da stock. Non modificare chiavi AVB, non ribloccare il bootloader,
non usare erase/format arbitrari. Un semplice aggiornamento APK non richiede reset.
I requisiti di recupero e i controlli dell'installer non sono sostituiti da questa nota.

La nota chiarisce il testo storico senza sostituire silenziosamente lo ZIP o
invalidare la firma e i checksum già pubblicati. La pipeline corrente contiene
già il README corretto per i bundle futuri.

## Verifiche e limiti

- Firma RSA originale di tutti e tre i manifest; package, firma Android ufficiale,
  SHA-256, dimensioni, immagini e compatibilità verificabili prima dell'installazione.
- APK SHA-256 `3b03ed8eb03972b589a032388f3e233f6e66eddff73688b4d8880f17b2655c5d`.
- Il nuovo installer ha test software Linux/Qt, ma il collaudo fisico completo
  della1.5.2 è rinviato ai tester: `hardware_qualified=false`, non un PASS hardware.
- È stato riferito un selettore HOME transitorio sul terzo Clock: se necessario
  scegliere **MiiClock → Sempre/Always**. Non rifare reset per questa sola schermata.
- Bootloader sbloccato, ADB USB root e SELinux Permissive sono compromessi espliciti.
  ADB TCP disabilitato. Non esporre porte/debug o credenziali in rete.
- Nessun software garantisce rischio zero. Verifica software e prove storiche
  non qualificano automaticamente ogni esemplare o tutti i percorsi di recupero.

[Verifica firme e checksum](VERIFY_RELEASE.md) · [Supporto](SUPPORT.md) · [Licenze](NOTICE.md)

## English summary

This coordinated **tester prerelease** is for **Xiaomi X04G only**: Linux Installer
1.5.2, firmware0.1.20/build032 and launcher0.26.5/code58. Use the three release
links above and the [English quick start](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/download/installer-v1.5.2/QUICK-START-EN.md).
Installer sources are optional for installation, but available for reproducibility.

The README inside the immutable firmware032 ZIP is historical. The current
clean-stock installation requires a verified personal backup, an unlocked bootloader
and a **manual factory reset after the complete flash**, which deletes user data.
Do not reset before flashing, relock the bootloader or bypass AVB verification.
An APK-only update does not require a firmware flash or a factory reset.
The APK prerelease is not automatically offered by the stable OTA latest endpoint.
The new installer is software-tested, not newly hardware-qualified; legacy first-OTA
migration and recovery still require community testing. See the release notes.
