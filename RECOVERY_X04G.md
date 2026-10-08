# Recovery X04G e raccolta supporto

## Quando usare questa guida

Usarla se un flash non termina, il Clock non si avvia entro dieci minuti o ADB
non torna disponibile dopo un aggiornamento firmware. Non usarla per tentare
azioni casuali su un device funzionante.

## Caso 1 — errore prima del reboot

Restare in Fastboot. Non riavviare e non eseguire erase/format. Applicare il
rollback previsto dal manifest della stessa release, rispettando l'ordine e
scrivendo `vbmeta` per ultima.

## Caso 2 — Android non completa l'avvio

1. Attendere fino a dieci minuti.
2. Entrare manualmente in Recovery.
3. Selezionare **Reboot to bootloader**.
4. Verificare tre risposte `mico_x04g` da `sudo fastboot getvar product`.
5. Applicare il rollback firmware verificato, senza wipe.

## Caso 3 — Fastboot non risponde

Non usare immagini o tool di recupero trovati online. BROM/MTKClient è un
recupero di ultima istanza e richiede il backup fisico dell'esemplare. Aprire
una segnalazione includendo soltanto dati redatti: modello, release,
operazione eseguita, risultato Fastboot e log privi di seriali/credenziali.

## Factory reset

Il factory reset cancella dati e provisioning Google. Non è una correzione
generica e non deve essere effettuato per aggirare un preflight o una build
non valida. Dopo il reset, Setup Wizard deve concludersi prima che MiiClock
diventi HOME.
