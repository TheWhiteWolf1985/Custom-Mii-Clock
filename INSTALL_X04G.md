# Installazione firmware — solo X04G

> [!warning]
> Questa guida vale soltanto per Xiaomi Mi Smart Clock X04G. Il flash modifica
> il firmware del device: non continuare se il prodotto Fastboot non è
> `mico_x04g`.

## Prerequisiti

- backup e piano di rollback disponibili;
- alimentazione stabile e cavo USB affidabile;
- Linux con Android Platform Tools/Fastboot funzionanti;
- bundle della release e checksum verificati;
- tempo per attendere il primo boot e verificare ADB/UI.

## Verifica prima del flash

1. Entrare in Recovery sul Clock.
2. Selezionare **Reboot to bootloader**.
3. Eseguire tre volte `sudo fastboot getvar product`.
4. Proseguire solo se ogni risposta contiene `mico_x04g`.
5. Verificare il bundle con [VERIFY_RELEASE.md](VERIFY_RELEASE.md).

La sola presenza di `fastboot devices` o del device USB Google non certifica
che il protocollo Fastboot sia utilizzabile sullo X04G.

## Flash

Usare esclusivamente gli script e il manifest allegati alla specifica Release.
Essi stabiliscono quale profilo sia ammesso e l'ordine delle immagini. Nel
profilo di aggiornamento da una baseline MiiClock verificata, `vbmeta` viene
scritta sempre per ultima.

Non usare mai:

- `fastboot erase`;
- `fastboot format`;
- `--disable-verity` o `--disable-verification`;
- immagini di un'altra release, di LX04 o dump community;
- scritture a `userdata`, `metadata`, preloader o bootloader nel flusso normale.

## Dopo il flash

Attendere fino a dieci minuti per il primo boot. Verificare UI MiiClock, ADB
USB, firmware marker, AVB, funzioni principali e un secondo riavvio. Se non
si ottiene boot o ADB nel tempo previsto, rientrare in Recovery → Reboot to
bootloader e applicare soltanto il rollback indicato dal manifest della stessa
release.

Vedere anche [Recovery X04G](RECOVERY_X04G.md).
