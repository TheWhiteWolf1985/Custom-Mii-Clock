# Changelog

## 1.0.0-RC1 — firmware 0.1.16 (028), launcher 0.25.2

Prima release candidata pubblica per Xiaomi Mi Smart Clock X04G.

- Launcher MiiClock consolidato con Home, gestione schermate, App Drawer,
  Quick Settings, impostazioni, diagnostica e recovery.
- Calendario AOSP locale, meteo MET Norway e sveglie MiiClock autonome.
- Home Assistant WebView isolata, tastiera touch e MQTT Discovery con
  allowlist di comandi.
- Controllo MediaSession e overlay Assistant MiiPulse.
- OTA dell'APK con manifest firmato, verifica hash/certificato/compatibilità e
  rollback applicativo.
- Google Launcher rimosso dal firmware; Assistant Core, MediaShell e Provision
  conservati.

### Limiti noti

- supporto esclusivo X04G `mico_x04g`;
- nessuna sincronizzazione cloud del calendario;
- Media non è un player proprietario;
- il firmware richiede Fastboot: non esiste aggiornamento OTA del firmware;
- la release è candidata al test, non una garanzia universale.

## Release storiche

Le release `launcher-v0.17.1` e `launcher-v0.17.3` rimangono disponibili per
continuità degli aggiornamenti OTA degli installati precedenti. Non sono la
baseline RC1 e non devono essere usate come riferimenti per firmware o test
attuali.
