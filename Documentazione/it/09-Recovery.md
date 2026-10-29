# Recupero, errori e interruzioni

**Italiano** · [English](../en/09-Recovery.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

La sezione Recupero dell'installer è opzionale e distinta dall'installazione. Usa esclusivamente il backup personale verificato dello stesso Clock. Non è una garanzia di recupero universale o dei dati cifrati.

## Errore prima di una scrittura

Se un prerequisito, identità, firma, hash o controllo AVB fallisce, fermarsi e risolvere il requisito. Non sostituire loader, manifest, immagini o tool con copie trovate online. Un report software o un vecchio backup non autorizzano una scrittura sul dispositivo sbagliato.

## Errore durante il flash

Non riavviare, non scollegare per tentativi e non eseguire erase/format. Conservare il journal e leggere l'errore. L'installer blocca il seguito e il reboot quando una scrittura è fallita o incerta. Usare il percorso esplicito di recupero dello stesso dispositivo o chiedere supporto con un report redatto.

Il recupero ripristina soltanto sette partizioni firmware stock dal backup personale: `boot`, `recovery`, `dtbo`, `vbmeta_vendor`, `vbmeta_system`, `super`, `vbmeta`. È previsto Fastboot o BROM con rilettura di verifica; `vbmeta` viene scritta per ultima. Non ripristina intera memoria, GPT, preloader, bootloader, `seccfg`, `userdata` o `metadata` e non riblocca il bootloader.

## Avvio o connessione non completati

Il primo avvio può richiedere tempo. Se Android non parte, distinguere il reset postflash obbligatorio ancora da eseguire da un errore di scrittura. Solo dopo flash completo riuscito eseguire il reset come descritto nell'[installazione](08-Installation.md). Non usare il reset per aggirare verifiche fallite.

Se Fastboot enumera ma non risponde, entrare in Recovery e scegliere **Reboot to bootloader**. Non presumere che **Enter fastboot**, VID/PID o numero di porta dimostrino una sessione valida. BROM richiede la sequenza e il backup previsti dal programma, non una procedura improvvisata.

## Chiusura o aggiornamento della GUI

La chiusura della GUI lascia terminare soltanto il comando in corso, poi il worker si arresta al checkpoint. Non avvia rollback, retry o reboot automatici. Riaprire con la stessa cartella e controllare lo stato prima della ripresa. Uno stato `recovery_required` non va cancellato o sostituito per forzare il seguito.

Il reset dell'app nelle impostazioni MiiClock riguarda la sua configurazione e non ripristina il firmware stock. Non confonderlo con Recovery Android o con la sezione Recupero dell'installer.

Aprire la cartella `log`, leggere il report redatto e seguire [Supporto](../../SUPPORT.md). Non pubblicare dump, seriali, MAC o credenziali.
