# Versioni, download e stato della RC

**Italiano** · [English](../en/01-Release.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

MiiClock comprende firmware derivato dalla stock X04G, launcher integrato e installer grafico Linux. È una prerelease per test consapevoli sul modello `mico_x04g`; LX04 e altri smart display non sono compatibili.

| Componente | Versione | Release ufficiale |
| --- | --- | --- |
| Installer Linux amd64 | 1.5.2 | [Installer](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2) |
| Firmware | 0.1.20, build 032 | [Firmware](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/firmware-v0.1.20) |
| Launcher integrato e APK separato | 0.26.5, versionCode 58 | [Launcher](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) |

Il package è `local.miiclock.launcher`. L'APK nella release separata e quello incorporato nel firmware sono byte-identici. L'installer è vincolato a questa coppia firmware/launcher.

## Cosa scaricare

Per installare da stock: DEB `miiclock-installer-1.5.2-amd64.deb`, `installer-manifest.json`, `installer-manifest.sig` e `SHA256SUMS` dalla stessa release installer. Le dipendenze applicative private sono incluse; il firmware si scarica separatamente tramite installer.

Per importazione firmware offline: `miiclock-x04g-firmware-0.1.20.zip`, `firmware-manifest.json` e `firmware-manifest.sig`, tutti dalla release firmware. Per verifica manuale è disponibile `miiclock-x04g-firmware-0.1.20-SHA256SUMS.txt`.

Il solo APK serve a un'installazione MiiClock già compatibile. Non prepara un Clock stock e non introduce le modifiche native del firmware. Seguire [aggiornamento APK](10-APK-Updates.md).

## Stato verificato e limiti

- Test software Linux/Qt dell'installer: 550 prove superate; controlli di installazione, aggiornamento e integrità completati.
- Firmware e APK: firme, hash, compatibilità, AVB e corrispondenza dell'APK integrato verificati; 19 asset delle tre release riscaricati e controllati nell'audit del 24 ottobre 2026.
- Il 29 ottobre 2026 il proprietario riferisce funzionamento sui suoi tre Clock. È un riscontro d'uso personale, non una certificazione di ogni esemplare.
- Il nuovo collaudo fisico completo dell'installer 1.5.2 e di tutti i percorsi di recupero resta da eseguire: `hardware_qualified=false`.
- Prima migrazione OTA da watchdog legacy ancora da qualificare. La RC APK non è proposta automaticamente dal canale stabile.

Le istruzioni correnti prevalgono sul README di preparazione conservato nello ZIP firmware firmato: la prima installazione pulita richiede **reset manuale dopo il flash completo**, con perdita dei dati. Gli asset firmati rimangono invariati. Procedura completa: [installazione](08-Installation.md).
