# Contribuire a MiiClock

Le contributioni devono essere concentrate, riproducibili e compatibili con il
display 800 × 480 e il profilo Android 10 low-RAM dell'X04G.

## Regole essenziali

- non aggiungere firmware stock, dump, APK proprietari, chiavi o credenziali;
- non modificare AVB, bootloader, `userdata` o pipeline firmware senza un
  piano di rollback esplicito;
- aggiungere test host per logica, parsing o migrazione di stato;
- aggiornare la documentazione quando cambia un contratto utente o operativo;
- non introdurre dipendenze pesanti senza valutazione PSS/CPU;
- mantenere le traduzioni JSON complete per tutte le lingue supportate.

Aprire prima una issue per cambiamenti a firmware, OTA, sicurezza, MQTT,
Assistant o permessi privilegiati.
