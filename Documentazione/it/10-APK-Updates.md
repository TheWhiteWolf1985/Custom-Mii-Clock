# Aggiornamento del solo launcher

**Italiano** · [English](../en/10-APK-Updates.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

La RC APK è **0.26.5 / versionCode 58**, package `local.miiclock.launcher`. È byte-identica alla copia integrata nel firmware 032: un Clock già su quella coppia non ha bisogno di reinstallarla per ottenere la stessa versione.

## Disponibilità e compatibilità

La [release launcher](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) è una prerelease. Il controllo integrato basato su `releases/latest` non la propone automaticamente. È disponibile il download manuale dell'APK firmato con `ota-manifest.json`, `ota-manifest.sig` e `SHA256SUMS` della stessa release. La UI non fornisce una funzione di importazione manuale del manifest.

Il range firmato ammette firmware build 027–032. Questo è un requisito di compatibilità dell'APK corrente, non un invito a scaricare firmware diversi. Le correzioni native bootstrap/HOME richiedono 032 e non si ottengono installando il solo APK.

Un aggiornamento APK agisce su `/data/app` e non modifica `product`, `super`, `boot`, AVB o le altre partizioni. Non richiede il factory reset previsto per la prima installazione firmware. Un APK non prepara un Clock stock né concede nuovi privilegi firmware.

## Controlli e consenso

Il percorso OTA verifica firma RSA del manifest, schema, package, certificato Android ufficiale, SHA-256, dimensione, versionCode crescente, range firmware, spazio e stato della transazione. Controlli aggiornamenti al massimo ogni 24 ore; download e installazione richiedono consenso esplicito quando un aggiornamento compatibile è offerto.

Gli APK manuali vanno verificati contro il manifest autentico. Un nome file corretto non basta. Non aggirare controlli, firmare nuovamente l'APK o usare manifest di un'altra release. GitHub trasporta i file: la fiducia viene dalle firme e dai controlli locali.

## Verifica dopo installazione e rollback

Il motore conserva una copia verificata dell'APK precedente. Il health check richiede package/versione/firma corretti, MainActivity prevista, ruolo HOME, PID stabile per 30 secondi, journal coerente e assenza di safe mode, crash o ANR bloccanti. Il watchdog privilegiato controlla il recupero con esiti legati alla transazione; gli errori non vengono trasformati in successi.

Il rollback punta alla copia APK precedente verificata; la copia integrata in `product` resta fallback tramite rimozione dell'aggiornamento dell'app di sistema. La prima migrazione da un watchdog legacy dipende inizialmente da quello già installato: il recupero non è retroattivamente garantito e il suo collaudo fisico resta aperto. Tenere disponibile il backup e il percorso di [recupero](09-Recovery.md).
