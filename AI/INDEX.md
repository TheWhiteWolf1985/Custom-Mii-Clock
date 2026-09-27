# Indice operativo — MiiClock / Xiaomi Mi Smart Clock X04G

Fonte iniziale: `xiaomi-mi-smart-clock-x04g-passaggio-consegne.md`. Le informazioni sono riportate distinguendo fra stato confermato, tentativi falliti e attività ancora da verificare.

## Ordine di lettura

1. `PROJECT/PROJECT.md` — obiettivo, perimetro e vincoli.
2. `PROJECT/PRD.md` — risultato desiderato e accettazione.
3. `PROJECT/INVENTORY.md` e `PROJECT/STACK.md` — asset, partizioni e ambiente.
4. `KNOWLEDGE/DECISIONS.md`, `KNOWLEDGE/RISKS.md`, `KNOWLEDGE/ASSUMPTIONS.md`.
5. `PROJECT/RUNBOOK.md` — comandi noti e procedura sicura.
6. `EXECUTION/CLEANUP_BASELINE_014.md` — allowlist, delete-list e struttura Git della baseline.
7. `EXECUTION/TASKS.md` e `EXECUTION/NEXT_ACTIONS.md` — attività ordinate.

## Stato sintetico

- Firmware v0.1.2 `x04g-magisk-visible-launcher-014` flashato e verificato fisicamente con due boot.
- Baseline attiva: Android 10, Magisk, ADB USB immediato/root, SELinux `Permissive`, AVB attiva e ADB TCP disabilitato.
- Launcher MiiClock 0.1.0 visibile e navigazione verso Applicazioni, Settings e UI stock verificata via webcam.
- Repository consolidato attorno a stock compresso, backup fisico cifrato e sola baseline modificata 014.
- Ogni futura release parte dalla baseline e richiede validazione offline, rollback, preflight e autorizzazione esplicita al flash.
