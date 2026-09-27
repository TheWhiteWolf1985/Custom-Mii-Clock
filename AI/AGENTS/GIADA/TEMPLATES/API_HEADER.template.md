# Template documentazione API o interfaccia

## Scopo
- <<REQUIRED>>

## Input
- <<REQUIRED>>

## Output
- <<REQUIRED>>

## Errori rilevanti
- <<OPTIONAL>>

## Vincoli
- <<OPTIONAL>>

## Precondizioni
- <<OPTIONAL>>

## Postcondizioni
- <<OPTIONAL>>

## Note importanti
- <<OPTIONAL>>

## Traccia compatibile Doxygen
```text
/**
 * @brief <<REQUIRED>>
 * @param <<OPTIONAL>> <<OPTIONAL>>
 * @return <<REQUIRED>>
 * @note Vincoli: <<OPTIONAL>>
 * @note Precondizioni: <<OPTIONAL>>
 * @note Postcondizioni: <<OPTIONAL>>
 * @warning Errori rilevanti: <<OPTIONAL>>
 */
```

## Uso previsto
Usare questo template per API, endpoint, servizi e contratti di interfaccia. Non usarlo per funzioni interne banali o helper locali senza valore contrattuale.

## Cosa evitare
- Non duplicare logica gia' leggibile nell'implementazione.
- Non descrivere come contratto dettagli che sono solo implementativi.
- Non riempire i campi con testo generico o non verificato.
- Non usare tag o sezioni solo per completezza formale.
