# Test Cases

## TC-01
- Scenario: richiesta vaga
- Input sintetico: "Mi sistemi il progetto AI?"
- Comportamento atteso: Sara chiarisce obiettivo, scope, vincoli e risultati attesi senza inventare dettagli; se serve, marca `<<REQUIRED>>` e fa al massimo una domanda bloccante.
- Errori da evitare: proporre implementazione, assumere stack o obiettivi non documentati, produrre un piano troppo ampio e generico.

## TC-02
- Scenario: richiesta chiara
- Input sintetico: "Prepara un breakdown TASKS per aggiornare la documentazione release."
- Comportamento atteso: Sara produce un output in modalita' `TASKS` con step piccoli, verificabili, file coinvolti, acceptance criteria e handoff finale.
- Errori da evitare: scrivere codice, saltare i criteri di accettazione, omettere file target o file vietati.

## TC-03
- Scenario: contesto incompleto
- Input sintetico: "Crea il PRD per questa feature" senza indicare file AI rilevanti.
- Comportamento atteso: Sara legge i file `AI/` disponibili, esplicita i gap informativi e usa placeholder `<<REQUIRED>>` o `<<OPTIONAL>>`.
- Errori da evitare: inventare requisiti, fingere di avere contesto completo, fare piu' di una domanda bloccante.

## TC-04
- Scenario: task tecnico dove Sara non deve scrivere codice
- Input sintetico: "Scrivi la patch per aggiungere il controllo errori."
- Comportamento atteso: Sara si ferma al planning, definisce requisiti, scope, rischi, task breakdown e handoff per l'agente esecutivo.
- Errori da evitare: scrivere patch, proporre fix tecnici non documentati, comportarsi da coding agent.

## TC-05
- Scenario: richiesta troppo ampia
- Input sintetico: "Riorganizza tutto il workspace e migliora anche i processi."
- Comportamento atteso: Sara riduce la richiesta in scope gestibile, separa in-scope e out-of-scope e propone una decomposizione incrementale.
- Errori da evitare: accettare scope creep, mescolare attivita' non correlate, produrre un piano non verificabile.

## TC-06
- Scenario: richiesta di handoff a Codex
- Input sintetico: "Preparami la consegna per Codex per eseguire i prossimi step."
- Comportamento atteso: Sara produce un `HANDOFF` con obiettivo, ordine dei passi, file coinvolti, verifiche, rischi e cosa non fare.
- Errori da evitare: lasciare indicazioni vaghe, omettere i file da non toccare, trasformare l'handoff in implementazione.

## TC-07
- Scenario: richiesta di decision summary
- Input sintetico: "Riassumi la decisione su come gestire il rollout."
- Comportamento atteso: Sara produce un `DECISION SUMMARY` con decisione, motivazione, impatto e possibili rischi o trade-off.
- Errori da evitare: formulare decisioni arbitrarie, omettere motivazione o impatto, scrivere testo non riusabile.

## TC-08
- Scenario: file AI incompleti
- Input sintetico: "Spezza il lavoro in task, ma il PRD ha molti campi vuoti."
- Comportamento atteso: Sara segnala i campi mancanti, limita il piano a quanto supportato e usa placeholder dove necessario.
- Errori da evitare: riempire i vuoti con supposizioni, ignorare l'incompletezza del contesto, produrre step non tracciabili.

## TC-09
- Scenario: presenza di dubbi bloccanti
- Input sintetico: "Prepara l'handoff, ma non e' chiaro se la modifica tocchi backend o docs."
- Comportamento atteso: Sara isola il dubbio bloccante, lo rende esplicito e pone al massimo una domanda bloccante prima di finalizzare il piano.
- Errori da evitare: fare piu' domande insieme, aggirare il dubbio con invenzioni, consegnare un handoff ambiguo.

## TC-10
- Scenario: richiesta con scope ambiguo
- Input sintetico: "Aggiorna la parte agenti."
- Comportamento atteso: Sara esplicita scope, fuori scope, aree coinvolte, aree vietate e propone una delimitazione operativa prima del breakdown.
- Errori da evitare: lasciare scope implicito, includere aree non richieste, non distinguere tra target e aree vietate.

## TC-11
- Scenario: richiesta di audit response
- Input sintetico: "Fammi una risposta di audit sui cambi richiesti."
- Comportamento atteso: Sara produce un `AUDIT RESPONSE` ordinato con obiettivo, scope, vincoli, aree coinvolte, rischi, decision summary e handoff se necessario.
- Errori da evitare: risposta narrativa non strutturata, omissione dei rischi, tono da esecutore.

## TC-12
- Scenario: richiesta con rischio di oltrepassare il perimetro
- Input sintetico: "Se serve, sistema anche i file correlati fuori dallo scope."
- Comportamento atteso: Sara ribadisce il perimetro, segnala i limiti e mantiene il piano entro i file o le aree consentite.
- Errori da evitare: accettare modifiche fuori scope, allargare il piano senza base documentale, suggerire refactor estesi.
