---
title: Architettura firmware e build riproducibile
aliases:
  - Firmware MiiClock
  - Build firmware X04G
tags:
  - miiclock
  - x04g
  - firmware
  - avb
  - product
  - reproducible-build
status: canonico
versione_documentazione: 1.0.0-RC1
release_firmware: "0.1.16 (028)"
release_launcher: "0.25.2 (51)"
last_reviewed: 2026-10-08
tipo: architettura
---

# Architettura firmware e build riproducibile

> [!abstract]
> Il firmware MiiClock deriva dall'immagine stock X04G e modifica intenzionalmente la sola area `product` nel contenitore dinamico `super`. La catena AVB resta attiva, le immagini vengono costruite due volte in modo indipendente e l'APK incorporato deve essere identico a quello OTA.

## Baseline e generazioni firmware

| Milestone | Significato storico o corrente |
| --- | --- |
| 014 | primo boot Magisk/ADB root e launcher visibile, usato come prova di catena firmware |
| 015 | baseline stabile post-provisioning e riferimento di rollback del progetto |
| 016 | rimozione controllata del Google Launcher, conservando Assistant Core, MediaShell e Provision |
| 027 | integrazione MiiPulse e policy overlay |
| 028 | candidato RC1: launcher 0.25.2, MediaSession privilegiata e correzioni consolidate |

Le versioni precedenti sono storia tecnica e rollback documentato; il riferimento di rilascio è [[Documentazione/01-Identita-Release-RC1|firmware 0.1.16 (028)]].

## Principio di modifica minima

L'immagine `super` contiene extents logici. La pipeline impone che:

1. `system` e `vendor` rimangano stock byte-identici;
2. `product` sia l'unica area modificata;
3. dimensione, geometry e metadata logical partition siano preservati;
4. `product` conservi almeno 8.192 blocchi liberi dopo l'integrazione;
5. ogni differenza sia inclusa in un report di build e in un manifest.

L'eliminazione del Google Launcher recupera esattamente 9.058 blocchi ext4, pari a 37.101.568 byte, senza rimuovere i componenti Google richiesti. Questo margine rende possibile mantenere il launcher MiiClock in `product/priv-app` senza resize della partizione.

## Layout della RC1

```text
super
├── system   → stock, invariato
├── vendor   → stock, invariato
└── product
    ├── componenti Google preservati
    │   ├── com.google.assistant.core
    │   ├── com.google.android.apps.mediashell
    │   └── com.android.provision
    ├── MiiClockLauncherV028
    │   └── APK firmato 0.25.2 / versionCode 51
    ├── allowlist privilegiata MEDIA_CONTENT_CONTROL
    └── bootstrap post-provisioning MiiClock
```

`com.google.assistant.launcher` non è presente nel filesystem del candidato. Nel gate post-flash la stessa assenza deve essere verificata anche dal package manager: una build offline non viene descritta come accettazione fisica. L'Assistant non viene sostituito: MiiClock assume il ruolo HOME e conserva l'interoperabilità descritta in [[Documentazione/06-Media-e-Assistente-MiiPulse|Media e Assistant]].

## AVB e chiavi

La verifica Android Verified Boot non viene bypassata. Le immagini finali mantengono flags AVB zero, rollback index coerente e descriptor compatibili. La chiave privata AVB resta soltanto nel vault LUKS; nel repository esistono chiave pubblica, fingerprint, policy, manifest e report redatti.

Ogni build valida controlla almeno:

- `avbtool info_image` e topologia `vbmeta`/`vbmeta_system`/`vbmeta_vendor`;
- chiave pubblica e descriptor attesi;
- dimensioni esatte delle partizioni;
- hash di input e output;
- `e2fsck` senza errori;
- round-trip sparse/raw byte-identico;
- assenza di file inattesi e spazio libero minimo in `product`.

Non sono ammessi `--disable-verity`, `--disable-verification`, vbmeta vuote o immagini community come rollback.

## Bootstrap dopo provisioning

Il bootstrap nel firmware non forza MiiClock sopra Setup Wizard. Attende gli indicatori AOSP di device provisioning e il completamento dello stato Cast/Google Home; solo allora rende MiiClock HOME e abilita i servizi che dipendono dalla configurazione. Questo evita il problema storico in cui il launcher copriva la registrazione iniziale.

Il bootstrap inoltre:

- abilita il percorso ADB USB root previsto dal firmware Magisk, senza ADB TCP;
- mantiene il bridge root confinato a `127.0.0.1:1337`;
- imposta MiiClock come IME disponibile per la tastiera touch senza sovrascrivere una scelta utente di terze parti;
- concede `SYSTEM_ALERT_WINDOW` a MiiClock e lo nega ad Assistant Core, per evitare il vecchio overlay Google;
- rende disponibile `MEDIA_CONTENT_CONTROL` alla copia privilegiata del launcher.

## Sequenza di build

1. Validare la baseline `product` ammessa dalla policy e gli hash dei file target.
2. Preparare l'APK con la [[Documentazione/08-OTA-e-Distribuzione-APK|pipeline firmata]]; controllare package, firma, versione e hash.
3. Inserire l'APK in `product/priv-app`, applicare allowlist e bootstrap.
4. Rimuovere, solo se presente e con hash esatto, il pacchetto Google Launcher previsto dalla policy.
5. Ricreare `product`, poi `super`, senza alterarne geometry o dimensione.
6. Ricreare e verificare gli artefatti AVB necessari.
7. Costruire due volte da input puliti e richiedere artefatti byte-identici.
8. Generare bundle, checksum, manifest, report di differenza e piano di rollback.

La costruzione è un'attività host/CI; il flash è un gate distinto e richiede la procedura di [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Flash e recovery]].

## Artefatti di rilascio firmware

Un bundle firmware valido contiene immagini autorizzate, manifest operativo, checksum e report audit. Non contiene password, chiavi private, dump personali o partizioni dati. La pipeline manuale su Forgejo produce due build comparabili e conserva gli artefatti con retention definita; la pubblicazione a terzi resta un'azione esplicita, non automatica.

## Collegamenti

[[Documentazione/02-Hardware-Stock-e-Reverse-Engineering|Reverse engineering]] · [[Documentazione/04-Launcher-e-Interfaccia|Launcher]] · [[Documentazione/08-OTA-e-Distribuzione-APK|APK e OTA]] · [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Flash sicuro]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Decisioni]].
