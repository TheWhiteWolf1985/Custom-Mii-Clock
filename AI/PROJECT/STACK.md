# Stack e ambiente tecnico

- Firmware: Android 10 stock su Xiaomi Mi Smart Clock X04G.
- Partizionamento: dynamic partitions contenute in `super`; le partizioni logiche di interesse sono indicate come system, vendor e product.
- Accesso dispositivo: ADB con shell root e fastboot; host USB affidabile Xubuntu/Linux reale.
- Client kiosk: Fully Kiosk Browser (`de.ozerov.fully`), URL configurato `http://192.168.1.5:8123/mii-clock/0`.
- Tool noti: `adb`, `fastboot`, `mtkclient`, `simg2img`, `e2fsck`, `resize2fs`, `lpmake`, `curl`, `wget`, `unzip`, `md5sum`, `sha256sum`.
- Tool di repack proposti/storici: `x04g_tools`; disponibilità e riproducibilità da verificare sull'host di lavoro.
- Nessun progetto sorgente Android o sistema di build del firmware è disponibile in questo workspace: il lavoro previsto è a livello di immagini binarie/offline.
