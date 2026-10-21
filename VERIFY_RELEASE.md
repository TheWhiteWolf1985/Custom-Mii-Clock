# Verifica di una release MiiClock

## Installer1.5.2 e firmware032

Sono due manifest distinti: `installer-manifest.json/.sig` autentica DEB,
sorgenti e materiali della release installer; `firmware-manifest.json/.sig`
autentica il bundle firmware. Non sono intercambiabili. Il firmware032 usa
il tag esplicito `firmware-v0.1.20`, mai latest/RC1 come fallback.
La chiave di verifica ufficiale è la stessa chiave pubblica MiiClock già
pubblicata; nessuna chiave privata è inclusa negli asset. L'installer verifica
autonomamente firma, hash, geometria e AVB prima di poter scrivere.

Per il pacchetto installer, scaricare anche `SHA256SUMS`, manifest e firma.
Usare la chiave ufficiale dal repository (non fidarsi soltanto della copia
eventualmente affiancata al download):

```bash
sha256sum --ignore-missing -c SHA256SUMS
openssl dgst -sha256 \
  -verify keys/miiclock-ota-manifest-v1-public.pem \
  -signature installer-manifest.sig installer-manifest.json
```

Il checksum del DEB deve risultare OK e la firma `Verified OK`. Controllare
anche versione, dimensione e digest del DEB nel manifest appena autenticato.
La verifica dei soli checksum senza firma non autentica la provenienza.
Per il firmware032 il file checksum è
`miiclock-x04g-firmware-0.1.20-SHA256SUMS.txt`; la firma firmware usa gli stessi
argomenti sotto. Gli esempi RC1 che seguono riguardano solo quella release storica.

## Prima dell'installazione firmware

1. Confrontare i nomi degli asset con la pagina Release.
2. Calcolare SHA-256 e confrontarlo con `SHA256SUMS`.
3. Verificare `firmware-manifest.json` e la sua firma detached con la chiave
   pubblica `keys/miiclock-ota-manifest-v1-public.pem`.
4. Confermare che il device sia X04G con product `mico_x04g`.
5. Confermare che il firmware sia la release prevista dal manifest.

Esempio Linux:

```bash
sha256sum -c miiclock-x04g-firmware-1.0.0-RC1-SHA256SUMS.txt
```

La radice del repository pubblico non contiene script operativi o workflow.
La release installer contiene il programma DEB e un archivio sorgenti separato
per riproducibilità e licenze. Il controllo manuale resta
manuale e riproducibile; su Linux, dopo aver scaricato tutti e quattro gli
asset firmware:

```bash
sha256sum -c miiclock-x04g-firmware-1.0.0-RC1-SHA256SUMS.txt
openssl dgst -sha256 \
  -verify keys/miiclock-ota-manifest-v1-public.pem \
  -signature firmware-manifest.sig firmware-manifest.json
```

Entrambi i comandi devono terminare senza errori; il secondo deve riportare
`Verified OK`.

## Prima dell'installazione OTA

L'app esegue localmente la verifica del manifest firmato, di package, firma
Android, dimensione, hash, versione e compatibilità firmware. Per controllo
manuale, confrontare il digest APK mostrato nella Release con quello riportato
nel manifest firmato.

Non installare APK con stesso nome ma hash, certificato o `versionCode`
discordanti.
