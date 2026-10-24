# MiiClock per Xiaomi Mi Smart Clock X04G

> **Stato:** RC per tester · Installer `1.5.2`, firmware `0.1.20 (032)`, launcher `0.26.5 / 58`;
> documentazione e asset RC1 conservati separatamente.
> **Compatibilità:** solo Xiaomi Mi Smart Clock **X04G** con product Fastboot `mico_x04g`.

MiiClock è un firmware derivato dal software stock X04G con un launcher
autonomo per Home, calendario locale, meteo, sveglie, Home Assistant, MQTT,
controllo MediaSession e overlay vocale MiiPulse.

## Installer Linux e firmware 032 — test comunitario

Disponibili come **pre-release per tester**:

- [MiiClock Installer 1.5.2 per Linux amd64](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2), con GUI IT/EN/FR/DE/ES;
- [firmware 0.1.20 (032), launcher 0.26.5/58 integrato](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/firmware-v0.1.20);
- [APK separato launcher 0.26.5/58 e manifest OTA firmato](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5), prerelease;
- [guida passo passo con installer](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/15-MiiClock-Installer-Linux).

Il nuovo installer è verificato via software; il suo collaudo fisico completo
è rinviato ai tester per esplicita decisione del proprietario, non dichiarato PASS.
Solo X04G, PC Linux fisico compatibile, backup personale verificato. La prima
installazione pulita impone il reset manuale **dopo** il flash e cancella i dati.
Le sezioni RC1 sotto descrivono la vecchia release: non mescolarne gli asset
con032. L'installer usa un tag firmware esplicito, mai latest/RC1 come fallback.

### Aggiornamento del solo APK — RC 0.26.5

L'APK separato è byte-identico a quello incorporato nel firmware032. Il manifest
firmato ammette firmware build27–32; le correzioni native di032 non vengono però
introdotte dal solo APK. La prima migrazione dal watchdog OTA legacy richiede
ancora collaudo fisico dedicato: consultare i limiti nelle note della release.
Il download manuale dell'APK non richiede flash o reset. Questa prerelease non
è proposta da `/releases/latest`: il canale OTA stabile resta distinto e non è
stato promosso. Verificare i quattro asset ufficiali, non mescolare i manifest.

## Prima di scaricare

- Non installare su LX04 o su altri smart display Xiaomi.
- Leggere prima [Installazione X04G](INSTALL_X04G.md),
  [Verifica della release](VERIFY_RELEASE.md) e [Recovery X04G](RECOVERY_X04G.md).
- L'installer Linux guida il flash del firmware032; la guida Fastboot manuale
  RC1 resta distinta. L'OTA aggiorna invece soltanto l'APK del launcher.
- La release RC1 è destinata a test controllati: segnalare regressioni seguendo
  [Supporto](SUPPORT.md).

> **Gate firmware:** il bundle firmware 028 ha superato le verifiche offline,
> il flash controllato e il collaudo su X04G. La Release pubblica resta una
> **pre-release** destinata a tester consapevoli, non una garanzia universale.

## Funzioni principali

- launcher MiiClock e Home post-provisioning;
- calendario locale AOSP con CRUD;
- meteo MET Norway con configurazione località senza GPS;
- sveglie autonome;
- dashboard Home Assistant in WebView isolata e tastiera touch;
- integrazione MQTT Discovery con comandi soggetti a allowlist;
- MediaSession Android e compatibilità con riproduzioni esterne;
- MiiPulse, overlay visivo per Assistant senza registrare audio;
- OTA firmato del solo APK;
- ADB root via USB, AVB attiva e ADB TCP disabilitato.

## Versioni della RC1

| Componente | Versione |
| --- | --- |
| Firmware | `0.1.16 (028)` |
| Launcher | `0.25.2`, `versionCode 51` |
| Package | `local.miiclock.launcher` |
| Range OTA APK | firmware build `27–28` |

L'APK integrato nel firmware e l'APK OTA devono essere byte-identici. I valori
di hash, certificato e compatibilità sono riportati in
[RELEASE-METADATA.json](RELEASE-METADATA.json).

## Documentazione

- [Wiki del progetto](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki)
- [Architettura firmware](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/03-Architettura-Firmware-e-Build)
- [Launcher e interfaccia](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/04-Launcher-e-Interfaccia)
- [Home Assistant, WebView e MQTT](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/07-Home-Assistant-WebView-MQTT)
- [OTA e distribuzione APK](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/08-OTA-e-Distribuzione-APK)
- [Decisioni, rischi e limiti](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/wiki/11-Decisioni-Rischi-e-Limiti)

## Repository e release

Forgejo è la sorgente tecnica privata e canonica. Questo repository GitHub
pubblica soltanto documentazione, chiavi pubbliche e asset verificati di
Release: non contiene mirror dei sorgenti privati, script operativi o GitHub
Actions. La release installer include un archivio sorgenti separato per
trasparenza e licenze dei componenti redistribuiti, non una pipeline pubblica. GitHub non è
la radice di fiducia dell'OTA. Per la politica completa vedere
[RELEASES.md](RELEASES.md).

## Licenze e attribuzioni

- Codice e script originali MiiClock: [Apache-2.0](LICENSE).
- Documentazione e immagini originali MiiClock: [CC BY 4.0](LICENSE-DOCUMENTATION.md).
- Componenti di terzi e firmware derivato: vedere [NOTICE.md](NOTICE.md).

Nessuna licenza MiiClock può concedere diritti sui componenti Xiaomi, Google o
su altri componenti di terzi inclusi nel firmware derivato.
