# Sicurezza, privacy e limiti noti

**Italiano** · [English](../en/11-Security-Limits.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

## Compromessi del firmware

La RC usa bootloader sbloccato, ADB root via USB e SELinux Permissive. Accesso USB fisico può dare accesso privilegiato; firme e AVB non annullano questo rischio. ADB TCP resta disabilitato. Non esporre debug o porte del ponte root in rete, non alterare le chiavi AVB e non ribloccare il bootloader.

La prima installazione firmware pulita richiede backup verificato e reset manuale dopo il flash, che cancella dati e configurazioni. Il recupero è limitato alle partizioni descritte e non garantisce i dati cifrati. Nessuna procedura è a rischio zero.

## Stato delle prove

Le verifiche software e dei file sono completate. Il proprietario riferisce funzionamento della RC sui suoi tre Clock; la revisione installer 1.5.2, il recupero su ulteriori esemplari e la prima migrazione OTA legacy non hanno una qualifica fisica completa. `hardware_qualified=false` resta esplicito. Non presentare il candidato come stabile universalmente qualificato.

## Limiti e problemi da conoscere

- Può comparire il selettore HOME: scegliere MiiClock e Sempre dopo provisioning. Non rifare reset per il solo selettore.
- La WebView persistente condivide il processo del launcher e il dispositivo ha poca RAM. Compatibilità con frontend/card Home Assistant moderni e sessioni prolungate richiede verifica sul proprio sistema.
- È stato osservato accumulo di processi terminati (zombie) figli del ponte root. La RC non include ancora una correzione specifica; il suo impatto nei test prolungati resta da valutare. Non è dimostrato come causa dei freeze touch.
- Il mirroring audio da PC al Clock non è qualificato come funzionante. La schermata Media controlla solo sessioni esposte da Android.
- Il calendario è consultivo: niente UI di creazione/modifica eventi o sincronizzazione cloud.
- L'installer richiede Linux fisico compatibile; un test in VM/rootfs non certifica USB o Polkit desktop sul PC del tester.
- Servizi Google, meteo e dashboard dipendono anche da rete e servizi esterni. Il motore sveglie MiiClock è separato dalle sveglie vocali Google.

## Dati personali

Backup completi, journal e log grezzi possono contenere dati privati. Tenerli fuori da repository e post pubblici; leggere il report redatto prima di condividerlo. Non pubblicare account, password, token, cookie, seriali, MAC o chiavi private.

Home Assistant usa la propria sessione nel profilo WebView. MQTT usa le credenziali inserite localmente: scegliere un broker fidato e privilegi adeguati. Niente credenziali reali negli screenshot di configurazione. Non è previsto upload automatico dei backup o telemetria dell'installer.

[Segnalazioni di sicurezza](../../SECURITY.md) · [Supporto](../../SUPPORT.md).
