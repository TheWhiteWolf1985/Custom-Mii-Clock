# Prossime azioni

1. Creare da `develop` un feature branch separato per la prossima modifica del launcher; la 0.7.4 approvata è già integrata.
2. Mantenere `Base_version` e `main` invariati finché non viene scelta esplicitamente una nuova baseline.
3. Mantenere firmware 014, Magisk, ADB USB root, SELinux `Permissive`, ADB TCP disabilitato e `system` stock come baseline obbligatoria.
4. Pianificare separatamente persistenza/autostart del launcher; non ricreare la modifica `system` fallita della 013.
