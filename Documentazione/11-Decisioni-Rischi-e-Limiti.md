---
title: Decisioni, rischi e limiti della RC1
aliases:
  - ADR MiiClock
  - Rischi RC1
  - Limiti MiiClock
tags:
  - miiclock
  - decisions
  - risks
  - security
  - rc1
status: canonico
versione_documentazione: 1.0.0-RC1
last_reviewed: 2026-10-08
tipo: governance
---

# Decisioni, rischi e limiti della RC1

> [!abstract]
> Questo documento spiega perché la RC1 è costruita sul firmware stock modificato, perché conserva alcuni componenti Google e quali limiti rimangono espliciti. Serve a evitare regressioni dovute a decisioni apparentemente più semplici ma incompatibili con X04G.

## Decisioni consolidate

### D1 — Ricostruire lo stock, non usare GSI o LineageOS

**Decisione:** modificare il firmware stock X04G con catena AVB coerente.

**Motivo:** GSI e ROM generiche hanno causato bootloop/incompatibilità. La base stock conserva driver, vendor, provisioning e contratti Google del device.

**Conseguenza:** la pipeline firmware è più rigorosa e richiede immagini, hash, AVB e rollback; in cambio evita una piattaforma Android genericamente instabile.

### D2 — MiiClock come HOME, Google Launcher rimosso

**Decisione:** eliminare solo `com.google.assistant.launcher` da `product`, rendendo MiiClock HOME post-provisioning.

**Motivo:** recuperare 37.101.568 byte e impedire il ritorno della UI stock davanti al launcher personalizzato.

**Conseguenza:** le card/Clock private Google non sono più disponibili. Assistant Core, MediaShell e Provision restano obbligatori e byte-verificati.

### D3 — AVB attivo, rollback reale

**Decisione:** nessun bypass AVB e nessun flash non documentato.

**Motivo:** ridurre il rischio di device non avviabile e rendere ogni modifica attribuibile/reversibile.

**Conseguenza:** chiavi private e backup vivono nel vault LUKS; flash soltanto con manifest e gate. Vedere [[Documentazione/10-Flash-Recovery-e-Quality-Gates|flash sicuro]].

### D4 — ADB USB root, nessun ADB TCP

**Decisione:** mantenere l'accesso ADB root richiesto dal progetto tramite USB e Magisk, disabilitando ADB TCP.

**Motivo:** serve operatività e diagnostica diretta senza esporre un servizio debug sulla rete locale.

**Conseguenza:** l'accesso root USB resta una superficie ad alto rischio fisico; il Clock non deve essere lasciato con USB esposta a host non fidati. Non bisogna descriverlo come hardening completo.

### D5 — Calendario e sveglie locali

**Decisione:** CalendarProvider locale e motore sveglie MiiClock, senza scrivere backend privati Google.

**Motivo:** contratti Google assenti/non affidabili; CRUD locale verificabile.

**Conseguenza:** non esiste sync automatica con Google Calendar o con le sveglie Assistant. Vedere [[Documentazione/05-Servizi-Locali-Calendario-Meteo-Sveglie|servizi locali]].

### D6 — MediaSession privilegiata, non NotificationListener

**Decisione:** usare `MEDIA_CONTENT_CONTROL` dalla copia privileged app in firmware.

**Motivo:** Notification Listener era inaffidabile su Android Q low-RAM; MediaSession è più generale e aderente alla piattaforma.

**Conseguenza:** l'APK OTA non può introdurre questi privilegi; la base firmware deve già esserci.

### D7 — OTA APK firmato, GitHub solo trasporto

**Decisione:** manifest firmato, hash e certificato Android verificati localmente.

**Motivo:** prevenire asset sostituiti, downgrade o release GitHub non autorizzate.

**Conseguenza:** firmware e OTA restano canali distinti; la pubblicazione GitHub è manuale. Vedere [[Documentazione/08-OTA-e-Distribuzione-APK|OTA]].

### D8 — Home Assistant senza Fully Kiosk

**Decisione:** WebView process-isolata e MQTT, senza app proprietarie/licenze.

**Motivo:** visualizzare una dashboard definita e integrare stati/comandi con un'implementazione controllata.

**Conseguenza:** il login è un normale login WebView; URL e broker devono essere configurati dall'utente. La policy blocca bridge JavaScript e comandi MQTT sensibili.

## Rischi correnti e mitigazioni

| Rischio | Mitigazione | Residuo |
| --- | --- | --- |
| Brick/boot failure | preflight, AVB, doppia build, rollback fisico, Fastboot verificato | alto impatto, probabilità ridotta |
| USB Fastboot instabile | Recovery → Reboot to bootloader e tre `getvar` | richiede presenza/azione fisica |
| Device low-RAM | processi isolati, nessun polling pesante, PSS diagnostico | dipende da dashboard/media esterne |
| Compromissione broker MQTT | allowlist, replay guard, reboot a due fasi, esclusioni | sicurezza del broker resta dell'utente |
| Asset OTA malevolo | manifest firmato, hash, signer, compatibilità | compromissione chiave privata richiede revoca/nuova chiave |
| WebView dashboard ostile | origin lock, nessun JS bridge, isolamento processo | dashboard può comunque consumare memoria/rete |
| Integrazione Assistant privata | fail-closed su hash/versione e overlay non touch | futuri update Google possono disattivare MiiPulse |

## Limiti dichiarati RC1

- supporto limitato a X04G `mico_x04g`;
- nessuna sincronizzazione cloud del calendario;
- nessuna sincronizzazione fra sveglie MiiClock e Assistant;
- Media non è un player proprietario e dipende da MediaSession/servizi esterni;
- MiiPulse è un layer visivo, non un assistente vocale alternativo;
- aggiornamenti firmware non sono OTA: richiedono Fastboot e preflight;
- la RC1 è soggetta a test distribuiti; non dichiara compatibilità universale;
- factory reset è distruttivo e richiede una procedura separata.

## Criteri per accettare una modifica

Una modifica deve dichiarare: impatto firmware/APK, autorizzazioni, spazio `product`, compatibilità OTA, test host, test fisici, rollback e aggiornamento della documentazione. Se una decisione cambia uno dei vincoli qui sopra, va trattata come nuova decisione, non come refactoring invisibile.

## Collegamenti

[[Documentazione/00-Indice-Wiki|Indice]] · [[Documentazione/03-Architettura-Firmware-e-Build|Firmware]] · [[Documentazione/07-Home-Assistant-WebView-MQTT|Home Assistant]] · [[Documentazione/08-OTA-e-Distribuzione-APK|OTA]] · [[Documentazione/12-Implementazioni-Future|Futuro]].
