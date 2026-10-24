# Politica di rilascio

## Ruoli dei sistemi

- **Forgejo privato:** sorgente canonica, build firmate, audit completi e
  materiali riservati.
- **GitHub pubblico:** documentazione, chiavi pubbliche, tag e asset di Release
  verificati; non contiene mirror dei sorgenti privati, script operativi o
  GitHub Actions. I sorgenti dell'installer e delle dipendenze redistribuite
  sono un asset separato della relativa release, per trasparenza e licenze.
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

- Stato corrente per tester: firmware0.1.20(032), launcher0.26.5/58 e installer1.5.2.
  La prerelease `launcher-v0.26.5` pubblica APK/manifest/firma/checksum senza
  promuovere il canale OTA stabile. Il firmware032 non è stato ricostruito.
- Nessun push automatico Forgejo → GitHub.
- Nessuna chiave privata, backup o credenziale su GitHub.
- Nessun firmware asset prima del gate fisico previsto per quella release.
- Eccezione esplicita2026-10-21: installer1.5.2 distribuito per tester senza
  nuovo collaudo fisico, rinviato dal proprietario perché non dispone di altri
  X04G stock. Non è una qualifica hardware: `hardware_qualified=false` resta.
  Firmware032 riusa il bundle già costruito, verificato offline e oggetto del
  buon esito riferito sul terzo Clock con il precedente installer.
- Le release storiche rimangono disponibili e sono indicate come supersedute,
  salvo un motivo di sicurezza documentato per rimuoverle.
- Una modifica al manifest o a un asset dopo la verifica richiede nuova
  validazione, non un semplice ricaricamento silenzioso.
