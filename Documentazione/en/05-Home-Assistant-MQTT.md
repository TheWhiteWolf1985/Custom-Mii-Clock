# Home Assistant and MQTT

[Italiano](../it/05-Home-Assistant-MQTT.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

Dashboard and MQTT are complementary configurations: the dashboard displays your server's interface, while MQTT exposes clock state and commands. A working connection in one does not prove the other is configured.

## Persistent dashboard in the carousel

1. Connect the clock to Wi-Fi and check that it can reach Home Assistant.
2. Under Settings → Home Assistant, enter the server origin and dashboard path separately. Example: origin `https://ha.example.com`, path `/lovelace/clock`. Replace these example values with your own.
3. The origin must contain scheme, host and optional port; no path, credentials, query or fragment. The dashboard path must begin with `/`.
4. Open Home Assistant from the dock or carousel and sign in on the page using the touch keyboard.
5. Return to Home and reopen the dashboard to check the session and navigation against your own server.

The RC uses a single persistent `HomeAssistantPage` inside the launcher, with a cached last frame for navigation. It is not a WebView Activity in a dedicated process. Persistence avoids recreation on each swipe, but Android can still terminate the process under memory pressure.

HTTP/HTTPS URLs are supported; HTTP is limited to private hosts, loopback and `.local`. HTTPS requires a valid certificate: TLS errors are not ignored. Navigation outside the configured origin is rejected; no native JavaScript bridge is exposed and file/content-provider access is disabled. Check that authentication redirects and the dashboard work within the same origin.

App configuration stores the origin and path; authentication, cookies and storage belong to the WebView profile. Clearing the session requires signing in again. Do not copy cookies or credentials between clocks. Android 10 WebView can limit compatibility with modern frontends and cards: test your own setup.

## MQTT Discovery setup

Enter broker host/port, authentication and device name in MQTT settings, then enable the connection. Broker details and credentials are local user data. Configure Home Assistant's MQTT integration against the same broker and check the MiiClock device and Discovery entities.

The RC includes availability, app/firmware information, display state, brightness, volume, screen selection, MiiClock alarms and permitted media functions. Actual entities depend on device capabilities and state. Use Discovery controls rather than invented topics or payloads.

Wallpaper URLs require a validated address without embedded credentials, loopback or redirects; the download limit is 8 MiB and replacement is atomic. A failed download must leave the previous wallpaper active.

Commands do not manage Wi-Fi, microphone, ADB, root, AVB, updates or calendar events. Reboot requires `PREPARE` and `CONFIRM`, a request identifier and expiry; it is not a single retained command. Timestamp/replay checks and bounded payloads restrict message execution.

An unreachable broker should not prevent local clock and alarms. Reconnection uses backoff; a connected broker does not prove Discovery, dashboard or commands work. Verify each path and read [security and limitations](11-Security-Limits.md).
