# text-humanizer

Rende naturale un testo in prosa, in qualsiasi lingua, senza inventare contenuti: niente fatti, numeri, nomi, date, citazioni o fonti che non siano già nel testo. Non serve a superare i rilevatori di testo generato: corregge i problemi di scrittura (vaghezza, enfasi, riempitivi, formule) che fanno sembrare un testo scritto da una macchina.

## Installazione

```
claude plugin marketplace add D:\Projects\text-humanizer
claude plugin install text-humanizer@text-humanizer
```

Per una sola sessione, senza installare: `claude --plugin-dir D:\Projects\text-humanizer`.

## Uso

```
/text-humanizer:humanize <testo o percorso di un file>
```

Si possono indicare il registro (`docs`, `business`, `personal`), il livello di intervento (`proofread` solo errori, `light` ritocchi dentro la frase, `full` riscrittura; il comando da solo vale `full`) e la richiesta di un voto di qualità. Senza registro, viene dedotto dal testo; nel dubbio vale `docs`, il più prudente.

Il risultato è un report: testo finale completo, modifiche raggruppate per pattern, punti aperti (affermazioni tagliate o lasciate generiche perché il testo non dava il fatto). Con un file in ingresso, il plugin chiede se sovrascriverlo, scrivere `<nome>.humanized.<ext>` o solo mostrare il risultato; senza risposta non scrive nulla.

## Componenti

| Componente | Ruolo |
|---|---|
| `skills/humanize/SKILL.md` | Il comando `/text-humanizer:humanize`: ingressi, brief all'agente, report, scrittura su file con conferma. |
| `skills/humanize/references/rules.md` | Le regole che legge l'agente: fedeltà, registri, lingue, 25 pattern, formato del report, voto. |
| `skills/humanize/references/italiano.md` | Profilo italiano: lessico, sintassi, tipografia, forma di cortesia, esempi. Si legge per ogni testo italiano. |
| `skills/humanize/references/pattern-catalog.md` | Esempi prima/dopo dei 25 pattern e un report completo. |
| `agents/text-humanizer.md` | L'editor che riscrive il testo in un contesto isolato. |

## Prove

`evals/` contiene tre casi italiani (email al cliente, risposta in un ticket, runbook con contenuti protetti), eseguibili con:

```
claude plugin eval D:\Projects\text-humanizer --no-publish
```

`evals/grade_outputs.py` controlla in modo meccanico che il report abbia le sue sezioni e che i valori letterali (orari, codici, citazioni, blocchi protetti) restino identici. Fedeltà semantica e naturalezza richiedono una lettura.

## Origine e fonti

Estratto dal plugin `text-humanizer` 1.0.0 di daodan (backup del 2026-11-03), dove era composto da skill `anti-ai-writing-patterns`, agente `text-humanizer` e workflow `/daodan:humanize-text`.

Il catalogo deriva da [blader/humanizer](https://github.com/blader/humanizer) (MIT, Copyright (c) 2025 Siqi Chen; licenza in `skills/humanize/references/upstream-license.txt`), riconciliato con la v3.1.0, commit `8c307f367c91bd974002f10f2be2965d642c13df`, il 2026-10-23, e segue [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), revisione 1377577399 (2026-09-30). La numerazione locale (25 pattern) differisce dai 26 dell'originale.

Da [hypnosdesign/claude-skill-scrittura-italiana](https://github.com/hypnosdesign/claude-skill-scrittura-italiana) (CC-BY-SA-4.0) vengono alcune idee, riscritte senza riprenderne il testo: i livelli di intervento, la verifica di fedeltà su cinque voci, la concretezza finta, alcune famiglie di tic italiani, l'eccezione per il testo informale e l'autonomia del testo pubblicabile.
