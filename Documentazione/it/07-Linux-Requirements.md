# Requisiti e installazione del programma Linux

**Italiano** · [English](../en/07-Linux-Requirements.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

## Requisiti effettivi

- PC **fisico Linux x86_64** con glibc ≥ 2.39.
- systemd ≥ 249 attivo come manager di sistema, Polkit/pkexec e D-Bus di sistema; desktop con agente di autenticazione e account amministratore.
- Strumenti di sistema previsti disponibili e non modificabili da utenti non privilegiati; librerie Qt/XCB, ABI native e `libfuse.so.2` compatibili.
- USB diretta, cavo dati affidabile, stessa porta durante la sessione e alimentazione stabile.
- Filesystem POSIX persistente, ext4 consigliato; non FAT/exFAT per la cartella personale.
- Almeno **32 GiB liberi** se cartella personale e staging sono sullo stesso disco; altrimenti **20 GiB sul sistema e 12 GiB sul disco backup**.

Il pacchetto fornito è `.deb`, per Debian/Ubuntu compatibili; Ubuntu 24.04 LTS richiede dipendenze aggiornate. La verifica usa capacità reali, non una whitelist del nome della distribuzione. Questo non rende ogni distro collaudata o il DEB adatto a Fedora/Arch. ARM, macOS, Windows, WSL, VM, container e runtime musl/Alpine non sono il percorso USB supportato.

## Installare e avviare

1. Scaricare il DEB e i materiali di verifica dalla [release installer](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2).
2. [Verificare firma e checksum](12-Verification.md).
3. Aprire il DEB con il gestore pacchetti e installare. Le librerie desktop del sistema possono richiedere Internet.
4. Avviare MiiClock Installer dal menu applicazioni come utente normale. Scegliere IT, EN, FR, DE o ES in alto a destra.
5. Eseguire il controllo PC; nessun requisito rosso può essere ignorato.

Python, ambiente privato, Qt e strumenti MTKClient/ADB/Fastboot/AVB necessari sono inclusi. Non occorre installare Python/pip globalmente né compilare dipendenze. Il firmware non è nel DEB.

La GUI non va avviata come root: Polkit autorizza il worker privilegiato quando richiesto. Non aprire altri ADB/Fastboot/MTKClient durante la sessione. Le cinque sezioni restano navigabili; solo le azioni dipendono dai prerequisiti.

## Aggiornare il programma

Chiudere la GUI e attendere il completamento delle operazioni attive. Aprire il nuovo DEB e scegliere Installa/Aggiorna, senza disinstallazione preventiva. Sessioni e backup personali vengono conservati. L'aggiornamento del programma non invia comandi al Clock.

Le verifiche software coprono installazione/GUI in Xubuntu 26.04 e un rootfs Ubuntu 24.04.4 isolato. Non certificano USB, Polkit desktop e flash su ogni sistema. [Stato della RC](01-Release.md).
