# Audit preinstallazione — OTA launcher 0.17.0

## Perimetro

- [x] Aggiornamento limitato a `local.miiclock.launcher` in `/data/app`.
- [x] Nessun codice di flash o scrittura partizioni introdotto.
- [x] `product`, `super`, `boot`, `vbmeta` e copia `/product` non modificati.
- [x] Download e installazione richiedono consensi distinti.

## Trust e supply chain

- [x] GitHub trattato solo come transport.
- [x] Manifest schema 1 firmato RSA-3072/SHA-256 sui byte esatti.
- [x] Sul Clock entra soltanto la pubblica, con fingerprint runtime.
- [x] Privata creata nel vault LUKS, mode `0600`; vault richiuso.
- [x] Package, versionName, versionCode, singolo signer e certificato Android verificati.
- [x] Downgrade e reinstallazione stesso code bloccati nel percorso update.
- [x] Compatibilità firmware inclusiva letta dal marker runtime.
- [x] Parser rifiuta campi ignoti/duplicati, tipi errati, nomi asset non canonici e limiti fuori intervallo.

## Robustezza

- [x] Download `.part`, limiti, fsync, SHA-256 e promozione atomica.
- [x] Controllo spazio prima di download e installazione.
- [x] Journal con `AtomicFile` e stati espliciti.
- [x] Worker in processo `:updater`, separato dalla UI.
- [x] Check periodico `JobScheduler` con rete e intervallo 24 ore.
- [x] Copia rollback riletta e verificata prima della scrittura.
- [x] Watchdog root indipendente durante package replacement.
- [x] Boot guard Magisk temporaneo per interruzione/riavvio; auto-rimozione.
- [x] Health check: package, versione, certificato, HOME, avvio, processo, foreground e crash journal.
- [x] Rollback automatico riservato alla copia precedente verificata.
- [x] Fallback `/product` documentato e lasciato intatto.

## Errori coperti

- [x] assenza rete/GitHub irraggiungibile/HTTP e release assente;
- [x] manifest o release malformati, firma errata, asset assente/duplicato;
- [x] hash/dimensione errati e download troncato;
- [x] package, versione, certificato o firmware incompatibili;
- [x] spazio insufficiente e staging/installazione falliti;
- [x] morte processo, crash post-update e riavvio durante la transazione;
- [x] rollback assente, alterato o con versione errata: fail-closed con fallback `/product` dichiarato.

## Verifiche software

- [x] Suite host precedente completa.
- [x] 31 test OTA: parsing, firma, package/cert, versioni, downgrade, firmware e transizioni.
- [x] Build Android release: PASS.
- [x] Lint: zero errori e nessun nuovo warning OTA.
- [x] Due build pulite byte-identiche e doppia firma APK nel vault.
- [x] Tre asset OTA 0.17.0 firmati e verificati dal publisher.
- [x] APK/manifest/firma e report canonici versionati nel commit.

## Gate fisico ancora vietato

- [x] Candidato committato e pushato sul ramo.
- [x] Audit finale senza caselle software aperte.
- [ ] Release GitHub caricata e asset riletti/hashati.
- [ ] Consenso esplicito dell'utente all'installazione APK 0.17.0.

Fino al completamento di queste caselle non installare sul Clock. Il presente
task non autorizza fastboot, flash firmware o scritture di partizioni.
