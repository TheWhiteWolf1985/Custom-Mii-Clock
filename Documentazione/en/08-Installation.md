# Guided RC installation

[Italiano](../it/08-Installation.md) · **English**

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Current tester release candidate. X04G only.

Clean first-installation workflow for installer 1.5.2, firmware 032 and launcher 0.26.5/58. Read [Linux requirements](07-Linux-Requirements.md), [security](11-Security-Limits.md) and [recovery](09-Recovery.md) before operating. First installation erases clock accounts, networking, configuration and data through a manual reset **after** the complete flash.

## 1. PC and device

1. Launch the application normally and select the language. Run **1.1 PC check**.
2. At **1.2**, select a new personal project folder for this clock. `backup`, `firmware` and `log` are created. Do not use another unit's backup; old dumps are not adopted automatically.
3. **1.3a:** authorize and connect the worker through Polkit. The application does not collect administrator passwords.
4. Enter Recovery: disconnect the clock; hold VOL+ and VOL− while reconnecting it. At **No command**, release the volume keys. Hold Mute, briefly press/release VOL+, then release Mute. In the menu, move with volume keys and confirm with Mute.
5. Select **Reboot to bootloader**, not **Enter fastboot**. A FASTBOOT label alone does not prove the protocol works.
6. **1.3b:** run the mandatory clock test; product `mico_x04g`, identity and geometry must match.
7. **1.4:** temporarily suspend ModemManager only if needed, following the GUI. The application handles restoring it.

## 2. Verified personal backup

1. Disconnect the clock and select **2.0 Connect BROM**. When the application displays **PRONTO/READY**, reconnect to the same port while holding VOL+. Wait for connection and identity confirmation.
2. Select **2.1–2.4** to acquire full-flash, boot1, boot2 and partitions in sequence. Do not disconnect during dumps; full-flash can take tens of minutes.
3. For the preloader, disconnect, select **2.5a**, reconnect with VOL+ when prompted and wait for connection. Select **2.5b** within five minutes.
4. Run **2.6 Full backup verification** and wait for the final green result. A single file at 100% does not complete this phase.

The personal copy is exported automatically under `backup`, with manifest and checksums. Keep the whole folder and a second private copy. An earlier acquisition does not establish a fresh live connection. On resumption, use the same folder/session and explicitly accept the offered resume; an uncertain write requires recovery rather than forced continuation.

## 3. Unlocking

If the device is already unlocked, follow the verification path without repeating the write. Otherwise:

1. Disconnect, select **3.0**, wait for **READY** and reconnect with VOL+.
2. **3.1:** confirm `seccfg`-only unlocking after verified backup 2.6. This step does not install firmware.
3. **3.2** only completes an interrupted readback; it is not an additional unlock to run every time.
4. Recovery → **Reboot to bootloader**, then **3.3**: verify `unlocked: yes` on the same clock.

## 4. Firmware

1. **4.1:** download firmware from the explicit `firmware-v0.1.20` tag.
2. Alternatively, **4.2:** select `miiclock-x04g-firmware-0.1.20.zip`, `firmware-manifest.json` and `firmware-manifest.sig` from the same release together, in any order.
3. **4.3:** wait for signature, hash, size, LP metadata and AVB chain verification. No failure can be treated as success.
4. With the clock in **Reboot to bootloader**, select **4.4 Install**. Read the single Yes/No confirmation, including the mandatory subsequent reset and data loss.
5. Wait for all seven writes: `boot` → `recovery` → `dtbo` → `vbmeta_vendor` → `vbmeta_system` → `super` → `vbmeta`. Do not interrupt power or USB. Stop without rebooting at the first failure.
6. Only after every write succeeds, use **4.5 Reboot from bootloader**.

## 5. Manual reset and first boot

1. Open **4.6 Required reset instructions**. This button displays instructions; it does not erase data automatically.
2. In Recovery, choose **Factory data reset** if **Can't load Android system** appears, or **Wipe data/factory reset** from the menu. This reset takes place only after the complete flash.
3. Wait for **Data wipe complete**, then select **Reboot system now**. Do not interrupt a slow reset.
4. Complete Google/Home setup using your own details. If a HOME chooser appears, select **MiiClock → Sempre/Always**; do not repeat a reset just for the chooser.
5. Run **4.7 Check boot and images**. Verification checks the actual reset, identity, Android, version/images, HOME and USB root.
6. If Google setup is incomplete, finish it and repeat 4.7 without another reset. No second mandatory reboot is required.
7. Manually test touch, Wi-Fi, time/time zone, alarms, Assistant, audio and screens. Configure [Home Assistant and MQTT](05-Home-Assistant-MQTT.md).

The preparation README inside the signed ZIP does not replace this procedure. Stock recovery and APK-only updates are separate workflows and do not require this clean-install reset. Installer 1.5.2 remains a tester candidate with full physical qualification pending.
