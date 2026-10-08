# MiiClock per Xiaomi Mi Smart Clock X04G

> **Stato:** `1.0.0-RC1` · documentazione e metadati di pre-release.
> **Compatibilità:** solo Xiaomi Mi Smart Clock **X04G** con product Fastboot `mico_x04g`.

MiiClock è un firmware derivato dal software stock X04G con un launcher
autonomo per Home, calendario locale, meteo, sveglie, Home Assistant, MQTT,
controllo MediaSession e overlay vocale MiiPulse.

## Prima di scaricare

- Non installare su LX04 o su altri smart display Xiaomi.
- Leggere prima [Installazione X04G](INSTALL_X04G.md),
  [Verifica della release](VERIFY_RELEASE.md) e [Recovery X04G](RECOVERY_X04G.md).
- Il firmware è un'operazione Fastboot manuale; l'OTA aggiorna invece soltanto
  l'APK del launcher.
- La release RC1 è destinata a test controllati: segnalare regressioni seguendo
  [Supporto](SUPPORT.md).

> **Gate firmware:** il bundle firmware 028 ha superato le verifiche offline,
> ma non ha ancora completato il collaudo fisico sul dispositivo. Non viene
> pubblicato come asset finché questo gate non risulta superato.

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

- [Wiki del progetto](Documentazione/00-Indice-Wiki.md)
- [Architettura firmware](Documentazione/03-Architettura-Firmware-e-Build.md)
- [Launcher e interfaccia](Documentazione/04-Launcher-e-Interfaccia.md)
- [Home Assistant, WebView e MQTT](Documentazione/07-Home-Assistant-WebView-MQTT.md)
- [OTA e distribuzione APK](Documentazione/08-OTA-e-Distribuzione-APK.md)
- [Decisioni, rischi e limiti](Documentazione/11-Decisioni-Rischi-e-Limiti.md)

## Repository e release

Forgejo è la sorgente tecnica privata e canonica. Questo repository GitHub
pubblica soltanto documentazione, chiavi pubbliche e asset verificati di
Release: non contiene sorgenti, script di build o GitHub Actions. GitHub non è
la radice di fiducia dell'OTA. Per la politica completa vedere
[RELEASES.md](RELEASES.md).

## Licenze e attribuzioni

- Codice e script originali MiiClock: [Apache-2.0](LICENSE).
- Documentazione e immagini originali MiiClock: [CC BY 4.0](LICENSE-DOCUMENTATION.md).
- Componenti di terzi e firmware derivato: vedere [NOTICE.md](NOTICE.md).

Nessuna licenza MiiClock può concedere diritti sui componenti Xiaomi, Google o
su altri componenti di terzi inclusi nel firmware derivato.
