# Installazione guidata della RC

**Italiano** · [English](../en/08-Installation.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

Percorso per prima installazione pulita con installer 1.5.2, firmware 032 e launcher 0.26.5/58. Leggere [requisiti Linux](07-Linux-Requirements.md), [sicurezza](11-Security-Limits.md) e [recupero](09-Recovery.md) prima delle operazioni. La prima installazione cancella account, rete, configurazioni e dati del Clock tramite reset manuale **dopo** il flash completo.

## 1. PC e dispositivo

1. Avviare il programma normalmente e scegliere la lingua. Eseguire **1.1 Controllo PC**.
2. In **1.2** scegliere una cartella personale nuova per questo Clock. Sono create `backup`, `firmware` e `log`. Non usare il backup di un altro esemplare; vecchi dump non sono adottati automaticamente.
3. **1.3a:** autorizzare e collegare il worker con Polkit. L'app non raccoglie password amministrative.
4. Entrare in Recovery: scollegare il Clock; tenere VOL+ e VOL− mentre lo si ricollega. Alla schermata **No command** rilasciare i volumi. Tenere Mute, premere/rilasciare brevemente VOL+, poi rilasciare Mute. Nel menu usare i volumi per spostarsi e Mute per confermare.
5. Selezionare **Reboot to bootloader**, non **Enter fastboot**. La sola scritta FASTBOOT non prova il protocollo.
6. **1.3b:** eseguire il test obbligatorio del Clock; product `mico_x04g`, identità e geometria devono risultare conformi.
7. **1.4:** sospendere temporaneamente ModemManager solo se necessario seguendo la GUI. Il programma ne gestisce il ripristino.

## 2. Backup personale verificato

1. Scollegare il Clock e premere **2.0 Collegamento BROM**. Quando il programma indica **PRONTO/READY**, ricollegare sulla stessa porta tenendo VOL+. Attendere la conferma di collegamento e identità.
2. Premere **2.1–2.4** per acquisire in sequenza full-flash, boot1, boot2 e partizioni. Non scollegare durante i dump; il full-flash può richiedere decine di minuti.
3. Per il preloader scollegare, premere **2.5a**, ricollegare con VOL+ quando richiesto e attendere il collegamento. Premere **2.5b** entro cinque minuti.
4. Eseguire **2.6 Verifica integrale del backup** e attendere l'esito finale verde. Il 100% di un singolo file non completa la fase.

La copia personale viene esportata automaticamente in `backup`, con manifest e checksum. Conservare tutta la cartella e una seconda copia privata. Un'acquisizione precedente non dimostra una connessione live nuova. Alla ripresa usare la stessa cartella/sessione e accettare la ripresa esplicita proposta; una scrittura incerta richiede recupero, non continuazione forzata.

## 3. Sblocco

Se il dispositivo è già sbloccato, seguire la verifica prevista senza ripetere la scrittura. Altrimenti:

1. Scollegare, premere **3.0**, attendere **READY** e ricollegare con VOL+.
2. **3.1:** confermare lo sblocco della sola `seccfg`, dopo backup 2.6 verificato. Il passaggio non installa firmware.
3. **3.2** completa soltanto un'eventuale rilettura interrotta; non è uno sblocco aggiuntivo da ripetere sempre.
4. Recovery → **Reboot to bootloader**, poi **3.3**: verificare `unlocked: yes` sul medesimo Clock.

## 4. Firmware

1. **4.1:** scaricare il firmware dal tag esplicito `firmware-v0.1.20`.
2. In alternativa **4.2:** selezionare insieme `miiclock-x04g-firmware-0.1.20.zip`, `firmware-manifest.json` e `firmware-manifest.sig` della stessa release, in qualsiasi ordine.
3. **4.3:** attendere verifica di firma, hash, dimensioni, metadata LP e catena AVB. Nessun errore può essere trattato come successo.
4. Con il Clock in **Reboot to bootloader**, premere **4.4 Installa**. Leggere la singola conferma Sì/No, che include il reset successivo obbligatorio e la perdita dati.
5. Attendere le sette scritture: `boot` → `recovery` → `dtbo` → `vbmeta_vendor` → `vbmeta_system` → `super` → `vbmeta`. Non interrompere alimentazione o USB. Al primo errore fermarsi senza reboot.
6. Solo dopo il successo di tutte le scritture usare **4.5 Riavvia da bootloader**.

## 5. Reset manuale e primo avvio

1. Aprire **4.6 Istruzioni reset obbligatorio**. Il pulsante mostra istruzioni: non cancella automaticamente i dati.
2. In Recovery scegliere **Factory data reset** se appare **Can't load Android system**, oppure **Wipe data/factory reset** dal menu. Questo reset avviene solo dopo il flash completo.
3. Attendere **Data wipe complete**, quindi **Reboot system now**. Non interrompere un reset lento.
4. Completare la configurazione Google/Home con i propri dati. Se appare il selettore HOME scegliere **MiiClock → Sempre/Always**; non ripetere un reset per il solo selettore.
5. Eseguire **4.7 Controllo avvio e immagini**. La verifica controlla reset effettivo, identità, Android, versione/immagini, HOME e root USB.
6. Se manca il setup Google, completarlo e ripetere 4.7 senza altro reset. Non è richiesto un secondo reboot obbligatorio.
7. Provare manualmente touch, Wi-Fi, orario/fuso, sveglie, Assistant, audio e schermate. Configurare [Home Assistant e MQTT](05-Home-Assistant-MQTT.md).

Il README di preparazione nello ZIP firmato non sostituisce questa procedura. Il recupero stock e l'aggiornamento del solo APK sono percorsi separati e non richiedono questo reset pulito. L'installer 1.5.2 resta un candidato per tester, con qualifica fisica completa pendente.
