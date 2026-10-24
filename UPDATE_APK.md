# Aggiornamento del launcher tramite OTA

L'OTA MiiClock aggiorna esclusivamente l'app `local.miiclock.launcher` in
`/data/app`. Non modifica `product`, `super`, `boot`, AVB o le altre
partizioni firmware.

## Flusso utente

**RC corrente0.26.5:** è una prerelease per tester, non il latest stabile.
Il controllo sotto descritto usa `releases/latest` e quindi non la propone
automaticamente. La release separata offre il download manuale dell'APK firmato.
Per versione/range e limiti leggere [Release candidate corrente](RELEASE-CANDIDATE.md).

1. Aprire **Impostazioni → Aggiornamenti** su MiiClock.
2. Eseguire il controllo aggiornamenti.
3. Leggere versione, compatibilità firmware e changelog.
4. Confermare esplicitamente download e installazione.
5. Attendere health check e riavvio del launcher quando richiesto.

Il controllo può essere eseguito automaticamente al massimo una volta ogni 24
ore. Download e installazione non sono automatici nella RC1.

## Verifiche di sicurezza

Il Clock rifiuta un update se una delle seguenti condizioni non è verificata:

- firma del manifest OTA;
- package `local.miiclock.launcher`;
- certificato Android MiiClock;
- dimensione e SHA-256 dell'APK;
- `versionCode` maggiore della versione installata;
- compatibilità con il firmware attivo;
- spazio disponibile e stato update coerente.

GitHub distribuisce gli asset ma non è la radice di fiducia. Il manifest firmato
e il certificato APK sono le verifiche decisive.

## Rollback APK

La prima OTA partita da una versione con watchdog legacy usa inizialmente il
vecchio watchdog: il nuovo motore non può garantire retroattivamente il recupero
se non riesce ad avviarsi. Questa migrazione resta un test fisico dedicato, non
una garanzia universale. I fix nativi del bootstrap richiedono firmware032.

Se il nuovo APK non supera il health check, il sistema ripristina l'APK
precedente verificato. La copia MiiClock integrata in `product` resta il
fallback estremo: la rimozione dell'aggiornamento di sistema la rende di nuovo
attiva senza riflashare il firmware.
