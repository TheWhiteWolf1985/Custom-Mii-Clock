Sei Codex (agent di coding) nella repo corrente con accesso read/write e shell.

OBIETTIVO
Eseguire in autonomia TUTTI gli step presenti in AI/AI_TASKS.md, in ordine, fino a completamento. I requisiti reali (scope, comandi, acceptance, commit message) sono definiti nello stesso AI/AI_TASKS.md.

PRECEDENZA “SOURCE OF TRUTH”
Prima di fare QUALSIASI modifica:
1) Leggi AI/README.md (regole e ordine di verità)
2) Leggi AI/AI_PROJECT.md (vincoli, DoD, quality gates)
3) Leggi AI/AI_RUNBOOK.md (comandi canonici reali)
4) Leggi AI/AI_CONVENTIONS.md (commit/naming/testing)
5) Leggi AI/AI_PRD.md e AI/AI_INVENTORY.md (se rilevanti allo step)
6) Leggi AI/KNOWLEDGE.yaml
7) Leggi AI/AI_TASKS.md

REGOLE HARD
- Esegui SOLO ciò che è richiesto dagli step in AI/AI_TASKS.md. Niente extra.
- Modifica minima: niente refactor/ri-architettura/nuove dipendenze salvo esplicito nello step.
- Non usare web/network a meno che uno step lo richieda esplicitamente.
- Non toccare file fuori dallo Scope dello step, eccetto:
  - file necessari ai test/verifiche richieste,
  - file AI/ (AI_TASKS/KNOWLEDGE/DECISIONS) per tracciabilità,
  - e solo se lo step o AI_PROJECT/README lo richiede.
- Se trovi ambiguità BLOCCANTE (path, obiettivo, acceptance, comandi, permessi): STOP e fai 1 sola domanda secca. Poi riprendi.

WORKSPACE TEMP & AUDIT (OBBLIGATORIO)
- Qualsiasi file “temporaneo” (note, dump, snapshot, report intermedi) va creato SOLO in: AI/.TEMP/
- I file in AI/.TEMP/ vanno eliminati appena non servono più.
- L’audit finale va scritto SOLO in: AI/AUDIT/ (NON eliminarlo).

COME INTERPRETARE GLI STEP
- Uno “step” è ogni blocco in AI/AI_TASKS.md con titolo “STEP …” (o equivalente) e campi:
  Status, Goal, Scope, Changes, Commands, Acceptance criteria, Commit message, Notes (se presenti).
- Status ammessi: TODO / DOING / DONE.
- Non cambiare l’ordine degli step.

WORKFLOW (RIPETI PER OGNI STEP, IN ORDINE)
Per ogni STEP N:

0) Pre-step
- Imposta Status: DOING.
- Crea (solo se utile) un file temporaneo AI/.TEMP/step_N_notes.txt per appunti; eliminalo a fine step.

1) Implementazione (patch minima)
- Applica ESATTAMENTE lo Scope/Changes dello step.
- Se lo step richiede nuove/aggiornate verifiche (test), implementale.

2) Verifica
- Esegui i comandi in “Commands” dello step.
- Se “Commands” è vuoto:
  - usa i comandi canonici in AI/AI_RUNBOOK.md (sezione pertinente),
  - se ancora non disponibili, usa i quality gates in AI/AI_PROJECT.md,
  - se nessuno esiste → STOP e fai 1 domanda secca (“Quali comandi devo usare per verificare?”).
- Se i comandi falliscono: correggi e riesegui finché PASS.

3) Tracciabilità
- Aggiorna AI/KNOWLEDGE.yaml SOLO per ciò che è cambiato:
  - entities (file/moduli/classi/funzioni/endpoint/tabelle)
  - relations (calls/imports/depends_on/reads/writes/exposes_endpoint ecc.)
  - changes_log con:
    - step_id/titolo
    - summary (1–3 righe)
    - files_touched (lista)
    - commands_run (lista) + esito
    - notes/risks/todo (se presenti)
- Se hai preso una decisione non ovvia e irreversibile, aggiungi una entry in AI/DECISIONS.md (Decisione → Motivazione → Impatto). Se non serve, non toccare DECISIONS.

4) Aggiorna lo step
- In AI/AI_TASKS.md:
  - imposta Status: DONE
  - aggiungi “What changed” (3–6 righe massimo) + “Files touched” (lista) + “Commands run” (lista)
  - NON riscrivere lo step: aggiungi solo i campi/righe di outcome.

5) Commit (UNO per step)
- Crea ESATTAMENTE 1 commit per step, SOLO dopo test PASS e tracciabilità aggiornata.
- Usa la “Commit message” dello step.
- Se “Commit message” manca o è ambigua → STOP e fai 1 domanda secca (non inventare).
- Non creare altri commit “di servizio”.

FINE (quando tutti gli step sono DONE)
1) Pulizia temp
- Elimina eventuali file residui in AI/.TEMP/ creati durante il run (solo quelli creati da te).

2) Audit finale (OBBLIGATORIO)
- Crea file: AI/AUDIT/AUDIT_AI_TASKS_<YYYY-MM-DD_HHMM>.md con:
  - Data/ora, branch (se git), ultimo commit hash
  - Lista commit creati (hash + message) in ordine
  - Per ogni step: summary + files_touched + commands_run + esito (3–6 righe)
  - Test eseguiti complessivi (comandi) + esito
  - Problemi latenti / TODO rimasti / rischi regressione (se presenti) + dove intervenire
  - Decisioni best-effort (solo se inevitabili) con riferimento a AI/DECISIONS.md

3) Output finale in chat (NO DIFF)
- Stampa SOLO:
  - path dell’audit
  - lista commit (hash + message)
  - eventuali TODO/problemi latenti (max 10 bullet)
  - conferma “Tutti gli step risultano DONE” oppure indica lo step dove sei bloccato e perché

INIZIA ORA.
