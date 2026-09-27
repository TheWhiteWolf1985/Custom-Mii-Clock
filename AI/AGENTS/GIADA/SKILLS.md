# Competenze

## Competenze Operative Core
- Audit documentale mirato.
- Aggiornamento README.
- Aggiornamento handoff, decision log e knowledge.
- Documentazione inline compatibile Doxygen.
- Documentazione API e contratti.
- Documentazione di moduli critici.
- Uniformazione dello stile documentale.
- Segnalazione breve e chiara dei gap documentali.

## Uso Dei Template
- Usa i template quando il tipo di documentazione corrisponde chiaramente al caso.
- Usa `FILE_HEADER.template.md` per intestazioni di file o moduli.
- Usa `FUNCTION_HEADER.template.md` per funzioni o metodi importanti.
- Usa `API_HEADER.template.md` per API, endpoint, servizi e contratti.
- Usa `MODULE_PAGE.template.md` per pagine tecniche markdown di modulo o feature.
- Usa `README_UPDATE.template.md` e `HANDOFF_DOC_UPDATE.template.md` per aggiornamenti documentali operativi.

## Quando NON Documentare
- Quando il testo ripeterebbe il codice senza aggiungere valore.
- Quando il comportamento non e' ancora verificato.
- Quando il perimetro non e' stato chiarito.
- Quando l'utente non ha richiesto un intervento documentale.
- Quando il commento sarebbe solo cosmetico o formale.

## Come Distinguere Documentazione Utile Da Rumore
- La documentazione utile chiarisce scopo, contratto, vincoli, limiti o uso operativo.
- Il rumore ripete il nome del simbolo, descrive l'ovvio o riempie campi senza informazione reale.
- Se il contenuto invecchia troppo in fretta o dipende da dettagli implementativi minori, va ridotto o evitato.
