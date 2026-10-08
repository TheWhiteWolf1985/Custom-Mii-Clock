# Politica di rilascio

## Ruoli dei sistemi

- **Forgejo privato:** sorgente canonica, build firmate, audit completi e
  materiali riservati.
- **GitHub pubblico:** documentazione, chiavi pubbliche, tag e asset di Release
  verificati; non contiene sorgenti, script o GitHub Actions.
- **Clock:** verifica in locale la firma OTA e non considera GitHub una radice
  di fiducia.

## Procedura obbligatoria

1. Congelare versione, versionCode, firmware supportati e changelog.
2. Eseguire la pipeline Forgejo firmata e le doppie build riproducibili.
3. Verificare checksum, firma APK, manifest OTA, AVB e audit firmware.
4. Costruire e validare l'export pubblico con gli strumenti di preparazione.
5. Creare un tag GitHub corrispondente al commit export approvato.
6. Creare una GitHub Release in stato draft.
7. Caricare manualmente gli asset verificati, confrontare hash e nomi.
8. Pubblicare come pre-release o release stabile in base all'accettazione.
9. Rendere la release immutabile se la configurazione GitHub del repository lo
   consente.

## Regole

- Nessun push automatico Forgejo → GitHub.
- Nessuna chiave privata, backup o credenziale su GitHub.
- Nessun firmware asset prima del gate fisico previsto per quella release.
- Le release storiche rimangono disponibili e sono indicate come supersedute,
  salvo un motivo di sicurezza documentato per rimuoverle.
- Una modifica al manifest o a un asset dopo la verifica richiede nuova
  validazione, non un semplice ricaricamento silenzioso.
