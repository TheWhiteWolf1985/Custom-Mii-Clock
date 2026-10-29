# Media, Google Assistant e MiiPulse

**Italiano** · [English](../en/06-Media-Assistant-MiiPulse.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

## Schermata Media

MiiClock legge la MediaSession esposta da Android tramite il permesso privilegiato `MEDIA_CONTENT_CONTROL`. Può mostrare titolo, artista, copertina, avanzamento e stato; play/pausa, precedente/successivo e seek sono disponibili solo se la sessione li dichiara.

La riproduzione va avviata da un provider compatibile o da Google Assistant. MiiClock non fornisce un catalogo musicale o un player autonomo. Il volume usa impostazioni rapide e tasti fisici. Se non c'è una sessione disponibile, la schermata lo indica senza simulare una riproduzione.

`MediaForegroundGuardService` gestisce il ritorno alla UI MiiClock quando una superficie visiva MediaShell copre il launcher, entro la policy prevista. Questo non certifica ogni provider o percorso Cast. Il mirroring audio da PC al Clock non è qualificato come funzionante nella RC.

## Google Assistant

Assistant Core, MediaShell e provisioning restano nel firmware. Google Assistant continua a dipendere dai propri servizi, account, rete e disponibilità lato Google. MiiClock non sostituisce il motore vocale. Le sveglie vocali Assistant restano distinte dalle sveglie locali MiiClock.

L'attivazione HOME attende il provisioning. Le integrazioni che dipendono da contratti privati verificano la compatibilità: una versione Assistant Core non riconosciuta può disattivare MiiPulse senza giustificare modifiche ai package Google.

## MiiPulse

MiiPulse è un overlay visivo trasparente e non interattivo. Un bridge Binder legge solo gli eventi minimi necessari allo stato conversazione e al livello vocale numerico; non salva audio, trascrizioni o comandi vocali. Decoder limitato, coda bounded, timeout e backoff riducono carico e casi di errore.

Gli stati comprendono `IDLE`, `LISTENING`, `START_OF_SPEECH`, `END_OF_SPEECH`, `THINKING`, `EXECUTING`, `TTS_PLAYING` ed `ERROR`. L'onda colorata usa 24 barre per lato e un massimo di 30 fps. L'overlay si nasconde a inattività, schermo spento, disconnessione o sveglia MiiClock attiva e non intercetta touch o tastiera.

La diagnostica espone compatibilità, stato del bridge, permesso overlay ed errore sintetico. Provare sul proprio Clock chiamata e annullamento Assistant, microfono disattivato, musica e dashboard aperta. La presenza di una MediaSession o di uno stato "playing" non dimostra da sola audio effettivamente udibile.
