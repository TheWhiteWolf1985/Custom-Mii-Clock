# AI_CONVENTIONS

## Tracciabilità

- Distinguere sempre `confermato`, `proposto/non verificato` e `fallito`.
- Per ogni immagine registrare percorso assoluto, origine, dimensione, SHA-256, data e ruolo.
- Ogni iterazione firmware modifica un solo obiettivo funzionale e conserva output ADB/fastboot/logcat.

## Naming degli artefatti

- Non sovrascrivere l'originale: usare una copia con suffisso descrittivo, revisione e data.
- Il nome deve identificare base stock, modifica applicata e stato (`work`, `test`, `verified`).

## Verifica e sicurezza

- Prima del flash: backup verificato, hash, criterio di successo, criterio di stop e rollback espliciti.
- Dopo il flash: boot, ADB/root, focus, dashboard, rete/touch e secondo reboot.
- Non fare wipe/reset senza decisione esplicita; non eliminare package Google in blocco.
- APK, immagini, credenziali e contenuti del progetto possono essere versionati soltanto in questo repository locale e nel remoto privato homelab autorizzato; non pubblicarli o sincronizzarli verso servizi esterni.

## Commit style

- Repository Git locale inizializzato sul branch `main`, con remoto privato `origin` previsto.
- Preferire Conventional Commits e un commit per iterazione verificata.
