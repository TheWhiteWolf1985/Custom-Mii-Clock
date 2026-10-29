# Glossario

**Italiano** · [English](../en/99-Glossary.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

| Termine | Significato nella RC |
| --- | --- |
| X04G / mico_x04g | Modello e product supportati; LX04 non compatibile |
| RC / prerelease | Candidato per tester, non qualifica stabile universale |
| Firmware 032 | Bundle 0.1.20 con componenti nativi e launcher integrato |
| APK | Applicazione Android MiiClock 0.26.5/58 |
| HOME | Ruolo del launcher assegnato dopo provisioning |
| Recovery | Ambiente Android per avvio bootloader e reset manuale |
| Fastboot | Protocollo usato dal programma per identità e scritture previste |
| BROM | Modalità MediaTek usata dal programma per acquisizione e operazioni autorizzate |
| product / super | Partizione logica dell'app integrata e relativo contenitore dinamico |
| AVB | Verifica della catena di immagini firmware; non equivale a bootloader bloccato |
| Manifest / firma detached | Metadati autenticati e file firma separato; diversi per installer, firmware e APK |
| SHA-256 | Digest di integrità da confrontare con il manifest autentico |
| OTA | Aggiornamento del solo APK, senza flash firmware o reset pulito |
| WebView | Componente Android che mostra Home Assistant nel carosello |
| MQTT Discovery | Configurazione che permette a Home Assistant di creare le entità del Clock |
| MediaSession | Stato e controlli esposti dal provider multimediale Android |
| MiiPulse | Overlay visivo dello stato conversazionale Assistant |
| Journal / checkpoint | Stato persistente delle operazioni per ripresa o recupero esplicito |
| hardware_qualified=false | Qualifica fisica completa dell'installer non ottenuta |
| Zombie | Processo terminato il cui stato di uscita non è stato raccolto dal padre |
