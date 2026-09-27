# Rischi tecnici

- **Perdita configurazione** — wipe di `userdata`/`metadata` o factory reset cancella la configurazione stock; mitigazione: vietati salvo decisione esplicita.
- **Bootloop** — GSI e Lineage hanno già dato bootloop; mitigazione: partire da repack stock invariato, una modifica per iterazione, backup/hash verificati.
- **Recovery non disponibile** — file stock citati potrebbero non essere presenti o integri; mitigazione: inventario, `sha256sum`, copie immutabili prima del primo flash.
- **Rimozione servizi essenziali** — `com.google.assistant.core` è protetto; mitigazione: non rimuoverlo e studiare il solo percorso launcher/shim.
- **Falsa persistenza** — workaround runtime già testati non rendono Fully kiosk dopo reboot; mitigazione: non reimplementarli come soluzione primaria.
- **Problemi host USB** — Windows/Zadig ha causato regressioni ADB; mitigazione: usare Xubuntu/Linux reale.
- **Incompatibilità del repack** — un'immagine prodotta non è prova di bootabilità; mitigazione: controlli offline e piano di recovery prima del flash deliberato.
- **Interruzione per quota modello** — una procedura fisica non deve dipendere da token quasi esauriti; mitigazione aggiornata dall'utente: non iniziare e fermarsi in un punto non critico quando il residuo raggiunge il 5%.
- **Esposizione backup/chiavi** — dump e chiave AVB privata non devono entrare in Git; mitigazione: volume LUKS dedicato, pattern `.gitignore` e report redatti.
- **Manifest pericoloso** — un errore di comando può colpire userdata o AVB; mitigazione: validatore fail-closed, argv senza shell, allowlist partizioni e `vbmeta` root ultima.
- **ADB USB root senza autenticazione** — qualunque computer con accesso fisico al Clock puo controllare integralmente Android; requisito cliente esplicitamente accettato. Mitigazione residua: nessun ADB TCP, accesso fisico alla porta USB controllato e verifica runtime da host nuovo. Magisk permanente e SELinux `Permissive` sono compromessi esplicitamente accettati dalla release 014.
