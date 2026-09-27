# Checklist — primo OTA reale 0.17.0 → 0.17.1

## Candidato

- [x] Versione 0.17.1/code 31, package invariato.
- [x] Suite host e 31 test OTA PASS.
- [x] Build e firma doppie byte-identiche.
- [x] Certificato APK ufficiale e firma manifest verificati.
- [x] Asset canonici e rollback 0.17.0 identificati per hash.
- [x] Vault richiuso.
- [x] Commit e push Forgejo completati (`7bd0955`).
- [x] GitHub Release pubblica pubblicata e riletta.
- [x] Hash degli asset riletti uguali agli originali e firma manifest `Verified OK`.
- [x] Trasporto anonimo disponibile al Clock: API `latest` e tre asset scaricati
  senza credenziali, con dimensioni e SHA-256 conformi.

## Clock

- [x] Bootstrap 0.17.0/code 30 installato con `Success`.
- [x] APK installato identico al candidato 0.17.0.
- [x] Firmware 015, HOME, ADB root e USB `adb` verificati.
- [x] UI Aggiornamenti e gestione `RELEASE_MISSING` verificate.
- [ ] 0.17.1 rilevata e changelog mostrato.
- [ ] Download verificato e promosso atomicamente.
- [ ] Installazione OTA e health check riusciti.
- [ ] Reboot, persistenza, screenshot/webcam e log finali PASS.

Nessuna operazione fastboot o scrittura di partizioni è ammessa.

## Pubblicazione

Il repository GitHub contiene esclusivamente la cartella documentale `AI/`; dopo
un controllo locale mirato che non ha rilevato token, password o chiavi private,
è stato reso pubblico con autorizzazione esplicita. Il Clock continua a fidarsi
soltanto della firma MiiClock e non contiene credenziali GitHub.
