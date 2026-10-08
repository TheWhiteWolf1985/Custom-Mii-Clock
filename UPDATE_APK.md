# Aggiornamento del launcher tramite OTA

L'OTA MiiClock aggiorna esclusivamente l'app `local.miiclock.launcher` in
`/data/app`. Non modifica `product`, `super`, `boot`, AVB o le altre
partizioni firmware.

## Flusso utente

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

Se il nuovo APK non supera il health check, il sistema ripristina l'APK
precedente verificato. La copia MiiClock integrata in `product` resta il
fallback estremo: la rimozione dell'aggiornamento di sistema la rende di nuovo
attiva senza riflashare il firmware.
