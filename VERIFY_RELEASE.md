# Verifica di una release MiiClock

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

La distribuzione pubblica non contiene script eseguibili. Il controllo resta
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
