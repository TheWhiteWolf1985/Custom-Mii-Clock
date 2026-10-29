# Verificare file e risultati

**Italiano** · [English](../en/12-Verification.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

Ci sono tre manifest distinti: `installer-manifest.json/.sig`, `firmware-manifest.json/.sig` e `ota-manifest.json/.sig`. Non sono intercambiabili. Scaricare file, firma e checksum della stessa release. La chiave pubblica ufficiale è `keys/miiclock-ota-manifest-v1-public.pem` nel repository; non fidarsi soltanto di una chiave arrivata insieme a un download sconosciuto.

## Controlli manuali Linux

Eseguire gli esempi dalla cartella contenente gli asset scaricati e la sottocartella `keys` copiata dal repository ufficiale. Per l'installer:

```bash
sha256sum --ignore-missing -c SHA256SUMS
openssl dgst -sha256   -verify keys/miiclock-ota-manifest-v1-public.pem   -signature installer-manifest.sig installer-manifest.json
```

Per il firmware:

```bash
sha256sum -c miiclock-x04g-firmware-0.1.20-SHA256SUMS.txt
openssl dgst -sha256   -verify keys/miiclock-ota-manifest-v1-public.pem   -signature firmware-manifest.sig firmware-manifest.json
```

Per l'APK, in una cartella separata con i quattro asset della release launcher:

```bash
sha256sum --ignore-missing -c SHA256SUMS
openssl dgst -sha256   -verify keys/miiclock-ota-manifest-v1-public.pem   -signature ota-manifest.sig ota-manifest.json
```

I file che si intende usare devono risultare `OK`; nessun checksum trovato o un file mancante da installare non è un successo. La firma deve risultare `Verified OK`. Verificare anche nome, versione, dimensione e digest dentro il manifest appena autenticato. I soli checksum non autenticano il produttore.

## Identità APK della RC

- Package: `local.miiclock.launcher`.
- Versione: `0.26.5`, versionCode `58`.
- SHA-256 APK: `3b03ed8eb03972b589a032388f3e233f6e66eddff73688b4d8880f17b2655c5d`.
- Certificato Android SHA-256: `7340f6343323e0474c4b6afc0e5891aa9e7f5d5176606be82b6124a39631c029`.
- ZIP firmware SHA-256: `d7b5db518c5468d9ad01e04af6495692b9bc8883fe28680d124033eb70a4f9bb`.

## Cosa dimostrano i controlli

L'installer verifica prima di scrivere firma/hash, metadata LP, geometria e AVB. Il confronto con un manifest autentico identifica i file; non prova che il proprio Clock abbia completato flash, reset o provisioning.

Il controllo 4.7 verifica lo stato osservabile di avvio/immagini, ma touch, Wi-Fi, audio, sveglie e interazione Home Assistant devono essere provati manualmente. La qualifica fisica completa dell'installer e dei percorsi di recupero resta pendente. [Stato e prove](01-Release.md).
