# Clock, calendar, weather and alarms

[Italiano](../it/04-Clock-Calendar-Weather-Alarms.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

## Clock and calendar

The clock uses the date, time, time zone and format configured in settings. The calendar reads the local AOSP CalendarProvider: it supports month navigation, day selection and agenda viewing. Settings include the first weekday and week numbers.

The current UI does not expose event creation, editing or deletion. Google Calendar, CalDAV and Home Assistant calendar synchronization are not provided. Android provider permissions do not imply a user-facing editing feature.

## Weather

Forecasts come from MET Norway Locationforecast 2.0. City/country/postcode search uses Open-Meteo/GeoNames; manual coordinate entry is available. GPS is not required. The user selects location and units.

Home shows a summary; the Weather screen presents conditions and a five-day forecast. Without valid data it shows an unavailable indication rather than a demonstration temperature. Cached data may remain visible as stale while offline. Search and refresh require Internet; weather failures should not prevent local clock and alarm use.

## MiiClock alarms

Alarms use an independent local engine with persistence, repeat weekdays, name, enabled state, ringtone, snooze and stop. The UI supports creating, editing and deleting alarms. MQTT integration exposes the actions allowed by its model.

Google Assistant voice alarms belong to a separate system and are not automatically synchronized with MiiClock alarms. A ringing MiiClock alarm suspends the MiiPulse overlay.

After setup, reboot or installation, manually check time/time zone, volume and a real alarm including snooze and stop. Local persistence does not promise operation during a power loss. Perform your own RC tests before relying on the device for an important deadline.

[Home Assistant and MQTT](05-Home-Assistant-MQTT.md) · [Media and Assistant](06-Media-Assistant-MiiPulse.md).
