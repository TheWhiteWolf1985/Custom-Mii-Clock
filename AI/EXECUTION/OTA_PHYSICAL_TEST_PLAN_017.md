# Piano di collaudo fisico OTA 0.17.0

## Fase A — installazione iniziale del motore OTA

Questa fase avverrà solo dopo consenso esplicito. L'APK 0.17.0 verrà installato
con il percorso ADB già provato, non tramite OTA, perché la 0.16.0 non possiede
ancora il client.

Preflight:

1. ADB `device`, boot completato, shell root, firmware marker 015.
2. HOME risolta su MiiClock; 0.16.0/code 29 attiva e certificato ufficiale.
3. Hash APK 0.17.0 uguale al candidato versionato; firma v2 valida.
4. Spazio `/data` sufficiente; copia `/product` presente e invariata.
5. Nessuna operazione fastboot o partizione.

Accettazione 0.17.0:

1. installazione ADB `-r` e versionCode 30;
2. HOME e foreground MiiClock;
3. apertura `Impostazioni > Aggiornamenti` e verifica valori reali;
4. job periodico unico, processo `:updater` non residente a riposo;
5. reboot, ADB root, HOME, stato OTA e launcher ancora validi;
6. screenshot digitale, webcam e logcat senza crash/ANR.

## Fase B — transport e release firmata

1. Pubblicare una release stabile con i tre asset canonici.
2. Da 0.17.0 eseguire `Controlla ora`: stessa versione deve risultare aggiornata, senza download.
3. Simulare offline/host irraggiungibile: errore comprensibile, launcher invariato, nessun retry continuo.
4. Ripristinare rete e verificare che un controllo manuale recuperi.

## Fase C — aggiornamento reale successivo

Con una futura 0.17.1/code 31 firmata:

1. check mostra versione, changelog e compatibilità;
2. nessun download parte prima della conferma;
3. interrompere un primo download: dopo riavvio il `.part` viene scartato e la 0.17.0 resta attiva;
4. completare download: stato `DOWNLOADED`, hash/cert/package conformi;
5. nessuna installazione parte prima della seconda conferma;
6. installare e verificare `INSTALLING`, `PENDING_VERIFICATION`, `SUCCESSFUL`;
7. provare due reboot, HOME, avvio, ADB root e assenza crash/ANR;
8. confermare che rollback verificato e copia `/product` esistano ancora.

## Fase D — rollback controllato

Il rollback automatico richiede una finestra di prova dedicata e un candidato
Android firmato costruito per fallire il solo health check, mai dati o boot.
La release difettosa non resterà latest oltre la prova presidiata. Atteso:

1. installazione atomica completata;
2. health check fallisce su HOME/avvio;
3. watchdog ripristina l'APK precedente verificato;
4. stato finale `ROLLED_BACK`, HOME precedente in primo piano;
5. reboot conferma stabilità; boot guard e file transitori sono rimossi.

Ripetere separatamente l'interruzione tramite reboot durante `INSTALLING` per
provare il guard Magisk. Non togliere alimentazione durante una scrittura senza
backup e osservazione ADB disponibili.

## Evidenze richieste

- output redatto di package/versione/certificato/hash;
- dump job e stato JSON redatto per ogni transizione;
- log watchdog senza percorsi sensibili;
- screenshot digitale e webcam delle tile principali;
- logcat limitato alle finestre di test;
- checksum di tutte le evidenze;
- risultato PASS/FAIL per ogni fase e stop immediato al primo criterio ambiguo.
