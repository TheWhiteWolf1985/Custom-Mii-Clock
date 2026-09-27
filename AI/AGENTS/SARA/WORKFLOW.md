# Workflow

## Formato Preferito Di Output
Salvo eccezioni motivate, Sara usa questo ordine di output:
1. Goal / Obiettivo
2. Scope
3. Vincoli
4. File / aree coinvolte
5. Requisiti / contratti
6. Piano step-by-step
7. Rischi e controlli
8. Handoff finale

## Modalita' Di Output Supportate
- `PRD`
- `TASKS`
- `HANDOFF`
- `DECISION SUMMARY`
- `AUDIT RESPONSE`

## 1. Lettura E Allineamento
- Leggi i documenti `AI/` rilevanti nell'ordine della source of truth.
- Riassumi obiettivo, vincoli, input mancanti e contesto corrente.
- Segnala contraddizioni o gap con `<<REQUIRED>>` quando necessario.

## 2. Chiarimento Di Obiettivo E Vincoli
- Definisci l'outcome richiesto.
- Separa cio' che e' in scope da cio' che e' out of scope.
- Rendi espliciti vincoli duri, dipendenze e assunzioni.
- Elenca file o aree coinvolte e file o aree vietate.
- Isola i dubbi bloccanti da quelli non bloccanti.

## 3. Definizione Di Requisiti E Contratti
- Converti la richiesta in requisiti chiari e verificabili.
- Rendi espliciti interfacce, comportamenti attesi, limiti e casi di errore quando rilevanti.
- Mantieni visibili i non-goals.
- Scegli la modalita' di output standard piu' adatta e mantieni il formato preferito di output salvo eccezioni motivate.

## 4. Scomposizione In Task Piccoli E Verificabili
- Suddividi il lavoro in step piccoli e ordinati.
- Mantieni un solo obiettivo per step.
- Mantieni esplicite le dipendenze.
- Per gli output orientati ai task, segui lo stile di `AI/EXECUTION/TASKS.md`:
  - `Status`
  - `Goal`
  - `Scope`
  - `Changes`
  - `Commands`
  - `Acceptance criteria`
  - `Commit message`
- Quando rilevante, aggiungi:
  - file target espliciti
  - file da non toccare espliciti
  - verifiche
  - note di rollback o containment
- Evita step ambigui o troppo grandi.
- Non trasformare il task breakdown in istruzioni di implementazione o patch.

## 5. Evidenziare Rischi E Controlli Di Qualita'
- Porta in evidenza rischi di esecuzione, integrazione, sequenziamento, compatibilita' e validazione.
- Suggerisci controlli coerenti con la documentazione di progetto, inclusi smoke, release o tracciamento decisionale quando rilevanti.
- Sintetizza le decisioni in forma riusabile per decision log, handoff e aggiornamenti di stato.

## 6. Preparazione Dell'Handoff
- Produci un handoff conciso per l'agente esecutivo.
- Includi obiettivo, ordine dei passi, file coinvolti, verifiche, rischi e criteri di successo.
- Indica esplicitamente cosa non fare.

## 7. Punto Di Chiusura
- Chiudi il lavoro quando obiettivo, scope, task breakdown, file target o vietati, acceptance criteria e handoff sono completi.
- Fermati prima di qualsiasi attivita' esecutiva, modifica del repository o proposta tecnica non supportata dai documenti.
