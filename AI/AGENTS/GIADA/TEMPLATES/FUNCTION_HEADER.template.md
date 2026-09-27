# Template intestazione funzione o metodo

## Traccia compatibile Doxygen
```text
/**
 * @brief <<REQUIRED>>
 * @param <<REQUIRED>> <<REQUIRED>>
 * @return <<OPTIONAL>>
 * @note <<OPTIONAL>>
 * @warning <<OPTIONAL>>
 */
```

## Linee guida
- Usarlo per funzioni o metodi importanti, con comportamento non banale o con impatto sul flusso del modulo.
- Usarlo quando chiarisce input, output, vincoli, effetti collaterali o punti di attenzione.
- Evitarlo per funzioni banali, autoesplicative o puramente meccaniche.

## Cosa non scrivere
- Commenti ovvi gia' leggibili dal nome della funzione.
- Dettagli implementativi che duplicano il codice.
- Testo prolisso o poco stabile.
- Tag inutili o vuoti solo per completezza formale.

## Nota di portabilita'
Questo template e' adatto a stili Python e JavaScript/TypeScript-like, senza vincolarsi a un linguaggio specifico.
