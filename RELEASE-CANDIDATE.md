# Scaricare la RC corrente

**Italiano** · [English](RELEASE-CANDIDATE.en.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

| Componente | Versione | Download |
| --- | --- | --- |
| Installer grafico Linux amd64 | 1.5.2 | [installer-v1.5.2](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/installer-v1.5.2) |
| Firmware X04G | 0.1.20 (032) | [firmware-v0.1.20](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/firmware-v0.1.20) |
| Launcher integrato e APK separato | 0.26.5 / code 58 | [launcher-v0.26.5](https://github.com/TheWhiteWolf1985/Custom-Mii-Clock/releases/tag/launcher-v0.26.5) |

Per partire da stock usare il DEB e la [guida completa](Documentazione/it/08-Installation.md). L'installer scarica il firmware esplicito 032; per importazione offline servono ZIP, `firmware-manifest.json` e `firmware-manifest.sig` insieme. La GUI verifica autenticità e compatibilità prima delle scritture. I sorgenti delle dipendenze non servono per la normale installazione.

La prima installazione pulita richiede **backup personale verificato, sblocco, flash completo e reset manuale successivo**, che cancella i dati. Usare un PC Linux fisico conforme ai [requisiti](Documentazione/it/07-Linux-Requirements.md). Non resettare prima del flash, modificare AVB o ribloccare il bootloader.

Il README di preparazione contenuto nello ZIP firmware firmato non descrive tutti i passaggi correnti; seguire la guida qui collegata. ZIP, firme e manifest delle release restano immutati.

La RC APK non è proposta automaticamente da `releases/latest`; il solo APK non aggiorna il firmware e non richiede reset. [Compatibilità e aggiornamenti](Documentazione/it/10-APK-Updates.md).

**Qualifica:** test software e integrità completati; funzionamento sui tre Clock riferito dal proprietario; nuovo collaudo fisico completo installer 1.5.2 e recupero pendente (`hardware_qualified=false`). Sono documentati anche un possibile selettore HOME e accumulo di processi zombie del ponte root, senza causalità dimostrata per freeze. Leggere [prove](Documentazione/it/01-Release.md), [limiti](Documentazione/it/11-Security-Limits.md) e [verifica firme](Documentazione/it/12-Verification.md).
