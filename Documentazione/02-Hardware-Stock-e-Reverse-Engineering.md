---
title: Hardware, firmware stock e reverse engineering X04G
aliases:
  - Reverse engineering X04G
  - Firmware stock MiiClock
tags:
  - miiclock
  - x04g
  - hardware
  - firmware-stock
  - reverse-engineering
  - mediatek
status: canonico
versione_documentazione: 1.0.0-RC1
last_reviewed: 2026-10-08
tipo: architettura
---

# Hardware, firmware stock e reverse engineering X04G

> [!abstract]
> Il progetto non nasce da una ROM generica: ricostruisce in modo controllato il firmware stock del Xiaomi Mi Smart Clock X04G, mantenendo la catena di boot e sostituendo con misura l'interfaccia utente. Il backup fisico dell'esemplare resta la sorgente di rollback, mentre il dump pubblico è solo materiale di studio.

## Hardware e sistema di base

| Area | Conoscenza consolidata |
| --- | --- |
| SoC | MediaTek MT8167 / famiglia MT8516-MT8362 |
| CPU | ARM32 |
| Sistema | Android 10, API 29, configurazione low-RAM |
| Pannello | 800 × 480 px a 240 dpi |
| Storage | eMMC con boot region, GPT e flash utente |
| Partizioni dinamiche | `super` con `system`, `vendor`, `product` |
| Slot | non A/B, `slot-count=0` |
| Prodotto fastboot | `mico_x04g` |

L'hardware limitato rende importanti dimensione APK, PSS a riposo, assenza di servizi pesanti e disciplina nella dimensione di `product`. Questa è la ragione per cui l'architettura privilegia Java e API AOSP disponibili invece di dipendenze esterne pesanti. Vedere [[Documentazione/04-Launcher-e-Interfaccia|Launcher e interfaccia]].

## Materiale stock e sorgenti di verità

### Backup fisico del device

Il backup fisico acquisito con MTKClient è la fonte primaria per rollback e confronto. Vive in un volume LUKS esterno al repository e include full flash, regioni boot e immagini delle partizioni. Non viene mai versionato insieme a `userdata`, identificativi del device, credenziali o chiavi private.

### Dump della community

Il file `Firmware stock/dump_mico_x04g.tar.xz` è conservato per studio; il suo hash SHA-256 registrato è `9ffd2a4b1b66d7f1ae0496b5e7e2f4db3e1d14cad74d03802f1df30d64dde214`. Non va considerato un'immagine stock pronta da scrivere: provenienza, dati personali e corrispondenza con l'esemplare fisico devono essere verificate prima di ogni utilizzo.

### Cosa resta invariato

La ricostruzione RC1 non sostituisce indiscriminatamente Android. Rimangono stock e verificati `system`, `vendor`, `boot`, `recovery`, `dtbo`, `vbmeta_vendor` e i componenti Google indispensabili. La modifica concentrata è l'extent `product` in `super`, insieme ai metadati AVB coerenti. I dettagli sono in [[Documentazione/03-Architettura-Firmware-e-Build|Architettura firmware]].

## Partizioni e ruolo operativo

| Partizione | Ruolo | Regola nel progetto |
| --- | --- | --- |
| `boot` | kernel, ramdisk, Magisk e avvio ADB root | non modificare nei flash applicativi ordinari |
| `recovery` | recovery Android e percorso fastboot affidabile | invariata salvo rollback completo autorizzato |
| `dtbo` | overlay device tree | invariata |
| `vbmeta*` | verifica AVB delle immagini | mai svuotare o disabilitare |
| `super` | contenitore logico di `system`, `vendor`, `product` | costruito in modo riproducibile |
| `product` | app privilegiate e configurazione MiiClock | area di integrazione principale |
| `userdata`, `metadata` | dati, provisioning e stato utente | mai scrivere durante un normale firmware flash |
| `preloader`, `lk`, `lk2` | bootloader MediaTek | esclusi dal flusso normale |

## Scoperte operative decisive

### Fastboot non è sempre realmente pronto

L'ID USB `18d1:4ee0` e l'output di `fastboot devices` non bastano. Il menu Recovery può mostrare fastboot mentre il protocollo resta bloccato. La sequenza affidabile è Recovery → **Reboot to bootloader**, quindi tre verifiche di `sudo fastboot getvar product` che devono restituire `mico_x04g`. Questa condizione è un gate, non un dettaglio diagnostico. Vedere [[Documentazione/10-Flash-Recovery-e-Quality-Gates|procedura di flash]].

### BROM e MTKClient

L'ingresso BROM/Preloader dipende dalla sequenza fisica e dal passthrough USB; viene gestito manualmente e impiegato per backup o recupero di ultima istanza. MTKClient ha permesso di leggere GPT, dump completi, boot region e preloader, ma non è la normale via di aggiornamento del firmware.

### GSI e ROM generiche non sono una scorciatoia

I tentativi storici con GSI/Lineage hanno generato bootloop e incompatibilità. La scelta consolidata è modificare il firmware stock ricostruito, conservando driver, vendor e catena AVB dell'X04G.

## Componenti Google: cosa è stato analizzato

Il reverse engineering ha separato l'interfaccia privata Google dai servizi necessari:

- `com.google.assistant.launcher` era il launcher e le card stock; è stato rimosso da `product` nella linea firmware successiva alla 015 per recuperare spazio.
- `com.google.assistant.core` resta necessario per Assistant vocale e collegamento Google Home.
- `com.google.android.apps.mediashell` resta necessario per superfici e integrazioni media/Cast stock.
- `com.android.provision` resta necessario per Setup Wizard e provisioning dopo un factory reset.

La rimozione è limitata a file hashati e soggetti a policy fail-closed; non è un semplice `rm` generico. Vedere [[Documentazione/03-Architettura-Firmware-e-Build|build firmware]] e [[Documentazione/11-Decisioni-Rischi-e-Limiti|decisioni]].

## Collegamenti

[[Documentazione/01-Identita-Release-RC1|Identità RC1]] · [[Documentazione/03-Architettura-Firmware-e-Build|Architettura firmware]] · [[Documentazione/09-Ambienti-Toolchain-e-Operativita|Toolchain]] · [[Documentazione/10-Flash-Recovery-e-Quality-Gates|Recovery]].
