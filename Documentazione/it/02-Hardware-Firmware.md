# Hardware e firmware X04G

**Italiano** · [English](../en/02-Hardware-Firmware.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

## Dispositivo supportato

Xiaomi Mi Smart Clock X04G, identificatore Fastboot `mico_x04g`, Android 10/API 29, display 800 × 480 e profilo low-RAM. Il nome commerciale o una sola enumerazione USB non dimostrano l'identità: l'installer verifica product, identità e geometria dello stesso esemplare attraverso le fasi della sessione.

## Composizione del firmware

Il firmware 0.1.20 (032) integra il launcher 0.26.5/58 come applicazione privilegiata in `product/priv-app`, dentro la partizione dinamica `super`. `system` e `vendor` restano stock. AVB e le sue catene devono restare coerenti con le chiavi pubbliche del progetto; la firma del manifest e gli hash identificano il bundle previsto.

Il launcher Google viene rimosso dal candidato. Assistant Core, MediaShell e i componenti di provisioning sono conservati. La RC include bootstrap del primo avvio, ponte privilegiato limitato al package MiiClock e watchdog di recupero OTA. Il solo APK non può installare questi componenti nativi.

Il device usa bootloader sbloccato, ADB root via USB e SELinux Permissive. ADB TCP è disabilitato. Questi compromessi fanno parte del firmware, non sono eliminati dalla presenza di AVB o delle firme.

## Attivazione e partizioni

Il provisioning Google/Home deve completarsi prima dell'attivazione HOME di MiiClock. Può comparire il selettore: scegliere MiiClock e Sempre. Il reset della prima installazione pulita viene eseguito dopo il flash, non prima.

L'installer scrive soltanto questa sequenza autorizzata: `boot`, `recovery`, `dtbo`, `vbmeta_vendor`, `vbmeta_system`, `super`, `vbmeta`. `vbmeta` è ultima. Non riscrive GPT, preloader, bootloader, `userdata` o `metadata` come scorciatoia alla procedura.

USB Fastboot visibile non basta: dalla Recovery selezionare **Reboot to bootloader**. La voce **Enter fastboot** può enumerare un'interfaccia che non risponde al protocollo richiesto. Seguire [installazione](08-Installation.md) e [recupero](09-Recovery.md).
