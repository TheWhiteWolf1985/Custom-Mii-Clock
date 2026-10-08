---
title: Flash, recovery e quality gate X04G
aliases:
  - Flash sicuro X04G
  - Recovery MiiClock
  - Quality gate firmware
tags:
  - miiclock
  - x04g
  - fastboot
  - recovery
  - rollback
  - quality-gate
status: canonico
versione_documentazione: 1.0.0-RC1
release_firmware: "0.1.16 (028)"
last_reviewed: 2026-10-08
tipo: procedura
---

# Flash, recovery e quality gate X04G

> [!abstract]
> Il flash X04G è consentito soltanto da manifest immutabili, dopo preflight del device e con un rollback fisico già verificato. Fastboot, backup e acceptance fisica sono gate distinti, non passaggi impliciti di una build CI.

> [!warning]
> Il flash modifica uno stato fisico e non è automatico. Questa procedura è un riferimento di sicurezza: ogni release deve avere il proprio manifest immutabile, preflight e rollback hashato. Non eseguire comandi di una release precedente senza verificarne input e hash.

## Principi non negoziabili

- usare Xubuntu con USB passthrough diretto e `sudo fastboot`;
- preservare `userdata`, provisioning Google, Wi-Fi, app e configurazione; niente `erase`, `format` o factory reset nel normale flash;
- non usare `--disable-verity`, `--disable-verification`, vbmeta vuote o immagini community come rollback;
- il rollback deriva dal backup fisico nel vault, non da file casuali o download;
- ogni scrittura fisica richiede un'autorizzazione esplicita separata e un manifest già committato/pushato;
- se un comando Fastboot fallisce, non riavviare e non improvvisare: fermarsi sul manifest e applicare il rollback previsto.

## Preflight obbligatorio

Prima di rendere un firmware flashabile devono risultare tutti verdi:

1. vault LUKS presente, sanità filesystem verificata e rollback fisico hashato;
2. build doppia riproducibile, checksum completi e release manifest immutabile;
3. `e2fsck`, sparse round-trip e validazione AVB passati; flags AVB zero;
4. `system` e `vendor` invariati; modifiche circoscritte a `product`;
5. APK incorporato byte-identico all'APK OTA e firma attesa;
6. spazio libero `product` sopra la soglia e policy di rimozione rispettata;
7. disponibilità modello/quota operativa sufficiente;
8. alimentazione stabile e USB affidabile;
9. commit e push già completati;
10. piano di rollback eseguibile con gli stessi controlli.

## Entrata Fastboot affidabile

1. Aprire Recovery sul Clock.
2. Selezionare **Reboot to bootloader**; il solo menu fastboot di Recovery non è sufficiente.
3. Verificare il collegamento USB.
4. Eseguire tre volte `sudo fastboot getvar product`.
5. Proseguire solo se tutte le risposte indicano `mico_x04g`.

`fastboot devices` o il VID/PID Google `18d1:4ee0` da soli non certificano il protocollo. Un `getvar` che si blocca è una stop condition: ripetere l'entrata tramite Reboot to bootloader, non forzare il flash.

## Ordine di flash

Il manifest seleziona solo le immagini necessarie. Per una release derivata dalla baseline firmware 015, l'ordine ordinario è:

```text
1. vbmeta_system
2. super
3. vbmeta      ← ultima, rende attiva la nuova catena
4. reboot
```

Una procedura di ripristino completo può avere un set più ampio (boot, recovery, dtbo, `vbmeta_vendor`, `vbmeta_system`, `super`, `vbmeta` ultima), ma è un piano dedicato e non va confusa con l'aggiornamento normale.

Non si toccano in un flash ordinario `boot`, `recovery`, `dtbo`, `vbmeta_vendor`, `userdata`, `metadata`, `preloader`, `lk` o `lk2`.

## Verifica post-flash

Il primo boot può richiedere fino a dieci minuti. Si verifica:

- boot Android completo e UI attesa;
- ADB USB `device`, root e assenza ADB TCP;
- AVB nello stato previsto, mai schermata red;
- SELinux/configurazione prevista dal firmware Magisk;
- mount e marker firmware;
- package MiiClock, code path e firma;
- assenza del Google Launcher e presenza hashata dei componenti Google preservati;
- HOME post-provisioning, Recovery, Quick Settings, meteo, calendario, sveglie, Media, MQTT, WebView e MiiPulse secondo il perimetro della release;
- almeno due riavvii per i servizi che devono persistere.

Ogni gate fisico raccoglie screenshot digitale, fotografia webcam quando utile e logcat filtrato; le prove vengono redatte prima del commit conclusivo.

## Rollback

### Se il flash si interrompe prima del reboot

Restare in Fastboot. Non riavviare. Applicare il set esatto di immagini di rollback previste dal manifest, con `vbmeta` per ultima, poi verificare boot completo.

### Se Android non completa il boot o ADB non compare

Attendere al massimo dieci minuti. Rientrare manualmente in Recovery → Reboot to bootloader e applicare il rollback firmware verificato senza wipe. Se anche Fastboot non è disponibile, BROM/MTKClient e il backup LUKS sono l'ultima risorsa, non un primo tentativo.

### Factory reset

Il factory reset cancella dati e provisioning; è un gate distruttivo a sé. Richiede autorizzazione esplicita, verifica completa di Setup Wizard/Google Home e conferma che MiiClock diventi HOME soltanto dopo il completamento. Non è un rimedio per una build non validata.

## Test e livello di evidenza

| Livello | Dimostra | Non dimostra |
| --- | --- | --- |
| Test host | logica, parser, policy, artifact | comportamento del device |
| CI/build | riproducibilità e integrità artefatto | flash riuscito |
| Preflight Fastboot | collegamento e modello esatto | avvio Android |
| Flash + ADB | boot e servizi basilari | esperienza prolungata |
| Soak/uso reale | stabilità nel tempo | compatibilità universale |

## Collegamenti

[[Documentazione/02-Hardware-Stock-e-Reverse-Engineering|Hardware e reverse]] · [[Documentazione/03-Architettura-Firmware-e-Build|Firmware]] · [[Documentazione/09-Ambienti-Toolchain-e-Operativita|Ambienti]] · [[Documentazione/11-Decisioni-Rischi-e-Limiti|Rischi]].
