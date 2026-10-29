# Home Assistant e MQTT

**Italiano** · [English](../en/05-Home-Assistant-MQTT.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

Dashboard e MQTT sono due configurazioni complementari: la dashboard mostra l'interfaccia del proprio server, MQTT espone stato e comandi del Clock. Il funzionamento di una non dimostra che l'altra sia configurata.

## Dashboard persistente nel carosello

1. Collegare il Clock al Wi-Fi e verificare che possa raggiungere Home Assistant.
2. In Impostazioni → Home Assistant inserire separatamente l'origine del server e il percorso della dashboard. Esempio: origine `https://ha.example.com`, percorso `/lovelace/clock`. Sono valori di esempio da sostituire.
3. L'origine deve contenere schema, host e porta eventuale; non percorso, credenziali, query o frammento. Il percorso dashboard deve iniziare con `/`.
4. Aprire Home Assistant dal dock o dal carosello ed eseguire il login nella pagina usando la tastiera touch.
5. Tornare alla Home e riaprire la dashboard per verificare sessione e navigazione sul proprio server.

La RC usa un'unica `HomeAssistantPage` persistente nel launcher, con cache dell'ultimo fotogramma per la navigazione. Non è una Activity WebView in un processo dedicato. La persistenza evita ricreazioni a ogni swipe, ma il sistema può comunque terminare il processo per memoria insufficiente.

Sono ammessi URL HTTP/HTTPS; HTTP è limitato a host privati, loopback e `.local`. HTTPS deve avere certificato valido: gli errori TLS non vengono ignorati. Navigazioni fuori dall'origine configurata sono rifiutate; non è esposto un bridge JavaScript nativo e l'accesso a file/content provider è disabilitato. Verificare che redirect di autenticazione e dashboard siano compatibili con la stessa origine.

La configurazione dell'app conserva origine e percorso; autenticazione, cookie e storage appartengono al profilo WebView. La pulizia della sessione richiede un nuovo login. Non copiare cookie o credenziali tra Clock. WebView Android 10 può limitare la compatibilità con frontend e card moderne: provarli sul proprio sistema.

## Configurazione MQTT Discovery

Configurare nelle impostazioni MQTT host/porta broker, autenticazione e nome del dispositivo, poi abilitare il collegamento. Broker e credenziali sono dati locali dell'utente. In Home Assistant configurare l'integrazione MQTT sullo stesso broker e verificare il dispositivo MiiClock e le entità Discovery.

La RC comprende disponibilità, informazioni app/firmware, stato display, luminosità, volume, selezione schermata, sveglie MiiClock e funzioni media consentite. Le entità effettive dipendono dalle capacità e dallo stato del dispositivo. Usare i controlli Discovery, non topic o payload inventati.

Lo sfondo tramite URL richiede un indirizzo validato, senza credenziali incorporate, loopback o redirect; limite download 8 MiB e sostituzione atomica. Un download errato deve lasciare attivo lo sfondo precedente.

I comandi non consentono gestione di Wi-Fi, microfono, ADB, root, AVB, aggiornamenti o eventi calendario. Il riavvio richiede `PREPARE` e `CONFIRM`, identificatore richiesta e scadenza; non è un singolo comando retained. Le verifiche di timestamp/replay e payload limitano l'esecuzione dei messaggi.

Un broker non raggiungibile non deve impedire orologio e sveglie locali. La riconnessione usa backoff; un broker connesso non prova che Discovery, dashboard o comandi funzionino. Verificare ciascun percorso e leggere [sicurezza e limiti](11-Security-Limits.md).
