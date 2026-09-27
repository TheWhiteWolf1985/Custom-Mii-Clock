# Checklist — OTA reale 0.17.2 → 0.17.3

## Candidato

- [x] Correzione `APK_SIGNER` fail-closed implementata.
- [x] Suite host e 31 test OTA PASS.
- [x] Due build e due firme APK byte-identiche.
- [x] Due manifest OTA firmati byte-identici e verificati.
- [x] Certificato Android ufficiale, package e firmware 15 conformi.
- [x] Vault LUKS richiuso.
- [x] Commit e push Forgejo completati (`9deafa7`).
- [x] Release GitHub pubblicata e riletta senza credenziali.

## Clock

- [x] Bootstrap correttivo 0.17.2/code 32 installato via ADB.
- [x] HOME MiiClock, processo in primo piano, ADB root e USB `adb`.
- [x] 0.17.3 rilevata con firma manifest valida.
- [x] Download atomico promosso dopo verifica APK completa.
- [x] Copia rollback 0.17.2 creata e verificata.
- [x] Installazione OTA, watchdog e health check riusciti.
- [x] Versione 33 e APK installato identico all'asset GitHub.
- [x] Riavvio, persistenza, screenshot, webcam e log finali PASS.

Nessuna installazione ADB diretta della 0.17.3 e nessuna modifica firmware.
