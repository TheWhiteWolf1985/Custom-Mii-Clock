# Launcher, navigazione e impostazioni

**Italiano** · [English](../en/03-Launcher.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

Il launcher `local.miiclock.launcher` 0.26.5/58 è progettato per il display 800 × 480 dell'X04G. La Home presenta orario, data, riepilogo meteo, sfondo e dock per Home Assistant, sveglie, impostazioni e calendario.

## Schermate e gesture

Il carosello comprende Home, meteo, calendario, sveglie, Home Assistant, media e App Drawer. **Gestione schermate** configura ordine, visibilità, schermata predefinita e timeout. La configurazione viene salvata localmente.

Scorrere a destra/sinistra per cambiare schermata. La vista Home Assistant è una WebView persistente integrata nel carosello: i cambi pagina non la ricreano deliberatamente a ogni passaggio. Le gesture sul contenuto web sono arbitrate con la navigazione del carosello; la tastiera touch permette di compilare i campi.

Uno swipe dall'alto apre le impostazioni rapide. Le pressioni prolungate aprono le azioni contestuali dove previste. Timeout e ritorno alla schermata predefinita dipendono dalle impostazioni e dallo stato della schermata: una sveglia attiva non deve essere trattata come una pagina inattiva.

## Impostazioni disponibili

- Gestione schermate, data/ora e fuso orario.
- Lingua italiana, inglese, francese, tedesca o spagnola.
- Aspetto e sfondo; formato 12/24 ore nelle impostazioni Data e ora.
- Calendario, meteo, Home Assistant e MQTT.
- Aggiornamenti, informazioni, memoria e diagnostica.
- Recovery dell'app: ritorno a MiiClock, configurazione dell'app e impostazioni Android. Non equivale al ripristino firmware dell'installer.

Le impostazioni rapide espongono luminosità, volume media, Wi-Fi, microfono e riavvio con conferma secondo le capacità del dispositivo. Il tasto hardware del microfono e i tasti volume mantengono il loro ruolo.

La diagnostica mostra versione firmware/app, stato HOME, ADB/root, Assistant/MiiPulse, MQTT e aggiornamenti; Memoria mostra valori reali di RAM/PSS. I valori diagnostici non sostituiscono un test di funzionamento. Leggere [funzioni locali](04-Clock-Calendar-Weather-Alarms.md) e [limiti noti](11-Security-Limits.md).
