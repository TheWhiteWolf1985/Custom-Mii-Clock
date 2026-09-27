# AI_PROJECT

## Scope

- Obiettivo principale: ricostruire una variante della ROM stock del Xiaomi Mi Smart Clock X04G che esegua stabilmente Fully Kiosk Browser sulla dashboard Home Assistant al boot.
- Ambito incluso: inventario e salvaguardia degli artefatti stock; analisi, unpack e repack offline della `super`; integrazione iniziale di Fully; analisi mirata del launcher Google e del suo shim; flash e verifica solo quando uno step lo autorizza esplicitamente.

## Non-goals

- Non adottare una ROM alternativa: GSI AOSP 10 e LineageOS20_X04G hanno già prodotto bootloop.
- Non fare rooting, unlock o ulteriori aggiramenti runtime.
- Non eliminare indiscriminatamente Google o l'intero `com.google.assistant.core`.
- Non modificare Home Assistant né la dashboard remota.

## Vincoli

- Modifiche minime necessarie; nessuna nuova dipendenza senza richiesta esplicita.
- Il remoto definito è privato nel homelab e gli artefatti di progetto devono essere versionati; non esportare il repository fuori da tale perimetro senza una nuova revisione di segreti e dati.
- Non eseguire factory reset, `erase userdata` o `erase metadata` sulla stock configurata, salvo decisione esplicita di ripartire da zero.
- Non riflashare `boot` o `super` senza backup verificato, hash registrato e obiettivo di test definito.
- Usare Xubuntu/Linux reale per USB, ADB e fastboot; evitare esperimenti con driver Windows/Zadig.
- Preservare `/data` durante la fase di chirurgia offline della stock.

## DoD

- Requisiti soddisfatti con evidenza verificabile.
- Documenti AI aggiornati (`TASKS`, `KNOWLEDGE`, `DECISIONS` quando serve).
- Immagini originali immutabili e artefatti di lavoro identificati con hash.
- Audit finale prodotto senza diff.

## Quality gates

- Build: repack `super` completato e identificato con dimensione e SHA-256; riproducibilità da dimostrare prima del primo flash di test.
- Lint/Format: non applicabile al firmware binario; controllare integrità dei file e script shell prima dell'uso.
- Unit test: non disponibile.
- Integration/E2E: boot della stock modificata, ADB disponibile, `/data` preservata, Fully avviabile e dashboard raggiungibile; il kiosk persistente è l'accettazione finale.

## Sicurezza e Privacy

- Dati sensibili coinvolti: configurazione in `/data` del clock e configurazione LAN della dashboard Home Assistant.
- Gestione secret: il repository remoto privato è il perimetro autorizzato dall'utente per gli artefatti del progetto; tenere i dati sensibili localizzati e documentarne la presenza.
- Regole data handling: conservare originali in `Firmware stock/` e tutte le modifiche in `Nuovo firmware/`; prima di qualsiasi condivisione esterna rivalutare immagini complete, chiavi e contenuti di `/data`.

## Logging e Observability

- Logging standard: catturare output ADB/fastboot e `logcat` post-boot per ogni flash; registrare focus con `dumpsys window`.
- Metriche minime: esito boot, disponibilità ADB root, activity in foreground, apertura dashboard.
- Alerting/tracing: non configurati; conservare i log di un boot fallito fuori dalle immagini originali.
