# Verifica della RC

**Italiano** · [English](VERIFY_RELEASE.en.md)

> Installer 1.5.2 · firmware 0.1.20 (032) · launcher 0.26.5 / versionCode 58
>
> Release candidate corrente per tester. Solo X04G.

Le tre release usano manifest distinti per installer, firmware e APK. Verificare checksum e firma con la chiave pubblica ufficiale, poi confrontare nome, versione, dimensione e digest dei file. Il solo checksum non autentica il produttore.

[Comandi Linux completi e identità APK](Documentazione/it/12-Verification.md) · [Versioni e download](Documentazione/it/01-Release.md).

L'installer verifica anche identità del Clock, geometria, metadata LP e AVB prima di scrivere. Integrità software e risultato di un controllo non sostituiscono la prova fisica di tutte le funzioni e del recupero. `hardware_qualified=false` resta il limite dell'installer 1.5.2.
