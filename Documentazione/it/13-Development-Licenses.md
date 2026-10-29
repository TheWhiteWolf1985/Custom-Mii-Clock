# Sviluppo, distribuzione e licenze

**Italiano** · [English](../en/13-Development-Licenses.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

## Ruoli dei sistemi

Forgejo privato conserva la sorgente canonica, pipeline, materiale di firma e audit. GitHub pubblica documentazione IT/EN, chiavi pubbliche, metadati descrittivi e asset delle tre release coordinate. Non è un mirror dei sorgenti privati e non esegue build pubbliche del firmware. La release installer include un archivio sorgenti separato per riproducibilità e licenze.

Le firme del manifest, il certificato APK e AVB governano i controlli; GitHub è un trasporto, non la radice di fiducia. Le chiavi private non sono distribuite e non devono entrare in repository, firmware, APK, log o report pubblici.

## Contratti tecnici correnti

Launcher Java per Android 10/API 29 e canvas 800 × 480; componenti separati per Home, servizi locali, WebView persistente, MQTT, MediaSession e overlay. Il firmware integra APK, privilegi necessari e bootstrap; la build controlla geometria LP, ext4, sparse e catena AVB. L'APK canonico, la copia firmware e la distribuzione separata devono essere identici.

Installer con GUI non privilegiata e worker root autorizzato da Polkit, socket locale con verifica dei peer, azioni nominate e journal. Non offre shell, offset, loader o partizioni arbitrarie. Il runtime Python/Qt e i tool sono privati dell'applicazione; l'installer non sostituisce libc, kernel o desktop del sistema.

Le release richiedono test software, doppie build riproducibili, verifica di firme/hash e accettazione esplicita del perimetro fisico. La RC corrente mantiene il limite `hardware_qualified=false`. Modificare documentazione non ricostruisce i binari e non rivalida un percorso fisico.

## Contributi

Aprire una issue prima di modifiche a firmware, OTA, MQTT, Assistant o privilegi. Segnalare modello, versione e risultato riproducibile con dati redatti. Preservare API, versioni di schema, compatibilità e traduzioni complete; valutare memoria/CPU sul profilo low-RAM. Una modifica ai binari richiede nuovi controlli e una nuova pubblicazione coerente, non sostituzione silenziosa di un asset firmato.

## Licenze

Codice originale MiiClock: Apache-2.0. Documentazione e immagini originali: CC BY 4.0, attribuzione `MiiClock project — TheWhiteWolf1985 and contributors — CC BY 4.0.` Indicare le modifiche e non suggerire approvazione del riutilizzo.

Componenti stock Xiaomi/Google, Android/AOSP, dipendenze dell'installer, dati MET Norway e ricerca località Open-Meteo/GeoNames mantengono licenze e condizioni proprie. Le licenze MiiClock non concedono diritti sui componenti di terzi. Leggere [Notice](../../NOTICE.md), la licenza software e gli avvisi distribuiti con l'installer. Gli avvisi non sono una certificazione legale del firmware derivato.
