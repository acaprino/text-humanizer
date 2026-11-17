# text-humanizer

Rende naturale un testo in prosa, in qualsiasi lingua, senza inventare contenuti: niente fatti, numeri, nomi, date, citazioni o fonti che non siano già nel testo. Non serve a superare i rilevatori di testo generato: corregge i problemi di scrittura (vaghezza, enfasi, riempitivi, formule) che fanno sembrare un testo scritto da una macchina.

## Installazione

```
claude plugin marketplace add acaprino/text-humanizer
claude plugin install text-humanizer@text-humanizer
```

Per una sola sessione, da una copia locale e senza installare: `claude --plugin-dir <cartella del repo>`.

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
claude plugin eval . --no-publish
```

`evals/grade_outputs.py` controlla in modo meccanico che il report abbia le sue sezioni e che i valori letterali (orari, codici, citazioni, blocchi protetti) restino identici. Fedeltà semantica e naturalezza richiedono una lettura.

## Licenza

MIT, vedi `LICENSE`.

## Origine e fonti

Estratto dal plugin `text-humanizer` 1.0.0 di daodan, dove era composto da skill `anti-ai-writing-patterns`, agente `text-humanizer` e workflow `/daodan:humanize-text`.

### Repository

- [blader/humanizer](https://github.com/blader/humanizer) di Siqi Chen (MIT, Copyright (c) 2025 Siqi Chen; licenza in `skills/humanize/references/upstream-license.txt`). È l'origine del catalogo dei pattern, riconciliato con la v3.1.0, commit `8c307f367c91bd974002f10f2be2965d642c13df`, il 2026-10-23. La numerazione locale (25 pattern) differisce dai 26 dell'originale; le regole di fedeltà, di tipografia e di scrittura su file sono locali.
- [hypnosdesign/claude-skill-scrittura-italiana](https://github.com/hypnosdesign/claude-skill-scrittura-italiana) (CC-BY-SA-4.0). Ne vengono alcune idee, riscritte senza riprenderne il testo: i livelli di intervento, la verifica di fedeltà su cinque voci, la concretezza finta, alcune famiglie di tic italiani, l'eccezione per il testo informale e l'autonomia del testo pubblicabile.

### Riferimenti

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/w/index.php?title=Wikipedia:Signs_of_AI_writing&oldid=1377577399), revisione 1377577399 del 2026-09-30, a cura di WikiProject AI Cleanup. Il catalogo ne segue l'impianto.
- Angela Ferrari, [*La semplicità sintattica in prospettiva testuale. Riflessioni a partire dalla Guida alla redazione degli atti amministrativi*](https://accademiadellacrusca.it/it/contenuti/la-semplicit-sintattica-in-prospettiva-testuale-riflessioni-a-partire-dalla-guida-alla-redazione-deg/9234), Accademia della Crusca, 19 marzo 2021. Sostiene le indicazioni su sintassi, ordine delle parole, passivo e soggetto sottinteso. Riguarda la scrittura italiana, non il riconoscimento di testi generati.
- Le norme redazionali dell'Accademia della Crusca, per virgolette e punteggiatura.

### Studi citati

- D. Kobak, R. González-Márquez, E.-Á. Horvát, J. Lause, [*Delving into LLM-assisted writing in biomedical publications through excess vocabulary*](https://doi.org/10.1126/sciadv.adt3813), Science Advances 11(27), 2025. Citato per il lessico in eccesso e per come cambia fra i modelli del 2023-24 e quelli successivi (pattern 7).
- A. Reinhart e altri, *Do LLMs write like humans? Variation in grammatical and rhetorical styles*, PNAS, 2025. Citato per la frequenza delle proposizioni participiali (pattern 3).
- T. S. Juzek, [*AI-Associated Lexical Shifts Across 34 Languages: Cross-Lingual Convergence and Diachronic Uptake in News Writing*](https://arxiv.org/abs/2605.25358), preprint arXiv, 2026. Citato nel profilo italiano per i verbi di enfasi e il lessico dell'importanza.
- G. Antonelli, citato nel profilo italiano come «Antonelli, 2025» per antilingua, registro innalzato, parallelismi negativi, chiusure e calchi dall'inglese. Il plugin d'origine non ne riportava il titolo.

Nessuno di questi studi è stato riletto per questa versione: le citazioni sono quelle del plugin d'origine.
