# Media, Google Assistant and MiiPulse

[Italiano](../it/06-Media-Assistant-MiiPulse.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

## Media screen

MiiClock reads Android's exposed MediaSession through the privileged `MEDIA_CONTENT_CONTROL` permission. It can show title, artist, artwork, progress and state; play/pause, previous/next and seek are available only when advertised by the session.

Start playback through a compatible provider or Google Assistant. MiiClock does not supply a music catalogue or standalone player. Volume uses quick settings and hardware keys. Without an available session, the screen reports this rather than simulating playback.

`MediaForegroundGuardService` manages return to MiiClock when a MediaShell visual surface covers the launcher, within the defined policy. This does not certify every provider or Cast path. PC-to-clock audio mirroring is not qualified as working in this RC.

## Google Assistant

Assistant Core, MediaShell and provisioning remain in the firmware. Google Assistant still depends on its services, account, network and Google's availability. MiiClock does not replace the voice engine. Assistant voice alarms remain separate from MiiClock's local alarms.

HOME activation waits for provisioning. Integrations based on private contracts check compatibility: an unrecognized Assistant Core version may disable MiiPulse without requiring changes to Google packages.

## MiiPulse

MiiPulse is a transparent, non-interactive visual overlay. A Binder bridge reads only the minimal events required for conversation state and numeric voice level; it does not save audio, transcripts or voice commands. A bounded decoder and queue, timeouts and backoff limit load and failure cases.

States include `IDLE`, `LISTENING`, `START_OF_SPEECH`, `END_OF_SPEECH`, `THINKING`, `EXECUTING`, `TTS_PLAYING` and `ERROR`. The coloured wave uses 24 bars per side and at most 30 fps. The overlay hides on inactivity, screen off, disconnection or a ringing MiiClock alarm and does not intercept touch or keyboard input.

Diagnostics expose compatibility, bridge state, overlay permission and a brief error. Test Assistant activation and cancellation, microphone mute, music and an open dashboard on your own clock. A MediaSession or "playing" state alone does not prove audible playback.
