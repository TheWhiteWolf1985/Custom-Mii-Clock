# Orologio, calendario, meteo e sveglie

**Italiano** · [English](../en/04-Clock-Calendar-Weather-Alarms.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

## Orologio e calendario

L'orologio usa data, ora, fuso e formato configurati nelle impostazioni. Il calendario consulta il CalendarProvider AOSP locale: permette navigazione mensile, selezione del giorno e lettura dell'agenda. Le impostazioni includono primo giorno della settimana e numeri delle settimane.

Nella UI corrente non sono esposte creazione, modifica o cancellazione degli eventi. Non è fornita sincronizzazione Google Calendar, CalDAV o calendario Home Assistant. I permessi Android del provider non implicano una funzione di scrittura disponibile all'utente.

## Meteo

Le previsioni provengono da MET Norway Locationforecast 2.0. La ricerca città/paese/CAP usa Open-Meteo/GeoNames; è disponibile l'impostazione manuale delle coordinate. Non è richiesto GPS. L'utente sceglie località e unità.

Home mostra il riepilogo; la pagina Meteo presenta condizioni e previsione a cinque giorni. Senza dati validi compare un'indicazione indisponibile, non una temperatura dimostrativa. La cache può restare visibile come non aggiornata in assenza di rete. Ricerca e aggiornamento richiedono Internet; un problema meteo non deve impedire l'uso locale di orologio e sveglie.

## Sveglie MiiClock

Le sveglie usano un motore locale indipendente, con persistenza, giorni di ripetizione, nome, abilitazione, suoneria, snooze e stop. La UI permette creazione, modifica e cancellazione delle sveglie. L'integrazione MQTT espone le azioni consentite dal relativo modello.

Le sveglie vocali di Google Assistant appartengono a un sistema distinto e non vengono sincronizzate automaticamente con quelle MiiClock. Una sveglia MiiClock attiva sospende l'overlay MiiPulse.

Dopo configurazione, riavvio o installazione verificare manualmente ora/fuso, volume e una sveglia reale con snooze e stop. La persistenza locale non promette funzionamento durante una perdita di alimentazione. Prima di affidarsi al dispositivo per una scadenza importante, eseguire le proprie prove sulla RC.

[Home Assistant e MQTT](05-Home-Assistant-MQTT.md) · [Media e Assistant](06-Media-Assistant-MiiPulse.md).
