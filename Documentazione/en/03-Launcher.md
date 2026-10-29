# Launcher, navigation and settings

[Italiano](../it/03-Launcher.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

Launcher `local.miiclock.launcher` 0.26.5/58 is designed for the X04G's 800 × 480 display. Home shows time, date, a weather summary, wallpaper and a dock for Home Assistant, alarms, settings and calendar.

## Screens and gestures

The carousel contains Home, weather, calendar, alarms, Home Assistant, media and App Drawer. **Screen management** configures order, visibility, default screen and timeout. Configuration is saved locally.

Swipe left/right to change screens. Home Assistant is a persistent WebView integrated into the carousel: changing pages does not deliberately recreate it on every visit. Gestures over web content are coordinated with carousel navigation; the touch keyboard allows form entry.

Swipe down from the top to open quick settings. Long presses open contextual actions where available. Timeout and return to the default screen depend on settings and screen state: a ringing alarm must not be treated as an idle page.

## Available settings

- Screen management, date/time and time zone.
- Italian, English, French, German or Spanish language.
- Appearance and wallpaper; 12/24-hour format under Date and time.
- Calendar, weather, Home Assistant and MQTT.
- Updates, device information, memory and diagnostics.
- App recovery: return to MiiClock, app configuration and Android settings. This is separate from firmware recovery in the installer.

Quick settings expose brightness, media volume, Wi-Fi, microphone and confirmed reboot according to device capabilities. The hardware microphone switch and volume keys retain their roles.

Diagnostics show firmware/app version, HOME status, ADB/root, Assistant/MiiPulse, MQTT and updates; Memory shows actual RAM/PSS values. Diagnostic values do not replace a functional test. Read [local features](04-Clock-Calendar-Weather-Alarms.md) and [known limitations](11-Security-Limits.md).
