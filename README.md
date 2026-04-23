# Ghid Claude Code pentru Business Architect AI

**De la instalare la construcție de skill-uri proprii, pe straturi, pentru solopreneuri români care vor Claude Code ca unealtă de lucru reală.**

Ghidul îți arată cum folosești Claude Code în VS Code ca unealtă de lucru zilnică, nu ca autocomplete scump. Fiecare strat introduce o capabilitate nouă, cu prompturi de verificare, capcane frecvente și o punte către stratul următor. E referință pe care o navighezi punctual, când ai nevoie, deși poți parcurge și liniar dacă pornești de la zero.

---

## Cum e organizat

Trei părți, unsprezece straturi, plus anexe:

**Partea I. Bază** (5 straturi) acoperă setup-ul și folosirea zilnică. La final, lucrezi productiv în VS Code cu Claude fără extensii suplimentare.

**Partea II. Extensii** (3 straturi) introduce delegarea, conectarea la sisteme externe și automatizarea pe evenimente. La final, Claude ajunge la Gmail, Notion, GitHub și poate rula scripturi automat la momente definite.

**Partea III. Construcție** (3 straturi) te trece din consumator în constructor. La final, îți împachetezi metodologia în skills și plugins reutilizabile.

**Anexe** pentru glosar, prompturi esențiale și troubleshooting cross-strat.

---

## Matricea straturilor

| Strat | Nume | Efort setup | Valoare solopreneur | Când sari direct aici |
|---|---|---|---|---|
| 1.1 | [Instalare](baza/instalare.md) | 15-45 min | Necesară | Nu sari, e prerechizit |
| 1.2 | [Primul contact](baza/primul-contact.md) | 10 min | Mare | Nu sari, e prerechizit |
| 1.3 | [Integrare cu editorul](baza/integrare-editor.md) | 5 min | Mare | Nu sari, e prerechizit |
| 1.4 | [Workflow de editare](baza/workflow-editare.md) | 15 min | Mare | Nu sari, e prerechizit |
| 1.5 | [Memorie de proiect](baza/memorie-proiect.md) | 30 min | Mare | Ai trei-patru proiecte recurente |
| 2.1 | [Subagenți](extensii/subagenti.md) | 30 min | Mediu | Ai task-uri cu mulți pași |
| 2.2 | [MCP servers](extensii/mcp-servers.md) | 1-2 ore | Mare | Vrei Claude conectat la Gmail, Notion, DB-uri |
| 2.3 | [Hooks](extensii/hooks.md) | 1-2 ore | Mediu | Automatizezi pe evenimente (pre-commit, formatare) |
| 3.1 | [Skills](constructie/skills.md) | 2-4 ore | Mare | Ai metodologie proprie de capturat |
| 3.2 | [Plugins](constructie/plugins.md) | 4-8 ore | Variabil | Împachetezi pentru echipă sau comunitate |
| 3.3 | [SDK și headless](constructie/sdk-headless.md) | 4+ ore | Low-Mediu | Scoți Claude din VS Code în pipeline-uri |

---

## Trei porți de intrare

👉 **Ești la început cu Claude Code?** Pornește de la [1.1 Instalare](baza/instalare.md) și parcurge Partea I în ordine.

👉 **Ai bazele, vrei să conectezi Claude la sistemele tale?** Începe direct cu [2.2 MCP servers](extensii/mcp-servers.md).

👉 **Vrei să construiești skills pentru workflow-ul tău?** Sari la [3.1 Skills](constructie/skills.md), cu prerechizitul [1.5 Memorie de proiect](baza/memorie-proiect.md).

---

## Anexe

- [Glosar](anexe/glosar.md). Termeni tehnici folosiți în ghid, cu definiție scurtă.
- [Prompturi esențiale](anexe/prompturi-esentiale.md). Formatele standard pentru prompturi, cu exemple.
- [Troubleshooting](anexe/troubleshooting.md). Probleme care traversează mai multe straturi.

---

## Surse și actualitate

Ghidul e ancorat în documentația oficială Anthropic ([docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code/overview) și [docs.claude.com](https://docs.claude.com)). Fiecare strat are secțiune "Surse" la final cu link-uri directe către paginile oficiale.

Claude Code evoluează rapid. Ghidul e verificat trimestrial. Ultima revizuire generală: 2026-04-23.

---

*Ghid scris de Dănuț Mitrut pentru solopreneuri care vor Claude Code ca unealtă de lucru, nu ca jucărie.*
