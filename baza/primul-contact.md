*Partea I, Bază > 1.2 Primul contact*

# 1.2 Primul contact

**Cum arată Claude Code în VS Code după setup: elementele interfeței, primul prompt, primul diff, plus ce îl diferențiază de Copilot și Cursor ca să știi la ce să te aștepți.**

| Meta | |
|---|---|
| Timp de setup | 10 minute |
| Timp de învățare până la fluență | 1-2 ore de practică |
| Valoare pentru solopreneur | Mare, setează așteptările corect pentru tot restul |
| Dependențe | 1.1 Instalare completă (VS Code, extensia Claude Code, signin reușit), cont Anthropic Pro/Max/Team/Enterprise sau Console |

**Acoperă:** elementele UI principale (Spark icon, panel, status bar), primul prompt, ciclul prompt → tool call → diff → accept, diferența concretă față de Copilot și Cursor, formatul unui prompt bun la începuturi.
**Nu acoperă:** instalare și setup (vezi 1.1), `@`-mentions și selecție (vezi 1.3), plan mode și auto-accept (vezi 1.4), CLAUDE.md (vezi 1.5), MCP (vezi 2.2).

---

## TL;DR

Extensia Claude Code aduce un agent în VS Code care citește fișiere, scrie cod și rulează comenzi singur. Funcționează diferit față de Copilot (care completează inline la tastare) și față de Cursor (care editează direct pe selecție). Conversația se întâmplă într-un panel drag-able, iar modificările la fișiere vin ca diff-uri side-by-side pe care le accepți sau le respingi.

---

## Ce vei putea face după

- Instala și autentifica extensia Claude Code în VS Code
- Recunoaște și folosi Spark icon, panelul Claude, status bar-ul
- Trimite primul prompt funcțional și interpreta tool call-urile vizibile
- Accepta sau respinge un diff propus de Claude
- Explica diferența față de Copilot și Cursor cuiva care nu a folosit Claude Code

---

## Diferența față de Copilot și Cursor

| Aspect | Copilot | Cursor | Claude Code |
|---|---|---|---|
| Interacțiune principală | Autocomplete inline | Edit asistat pe selecție | Conversație în panel |
| Cine decide ce fișiere citește | Tu, prin context deschis | Tu, prin selecție | Claude, autonom pe baza cererii |
| Poate rula comenzi shell | Nu | Limitat | Da, cu confirmare |
| Cum aplică schimbări | Direct la tastare | Edit direct cu approve | Diff side-by-side propus, accepți sau respingi |

> ⚠️ Confuzia frecventă: "Claude Code e ca Copilot, dar cu chat." Claude Code funcționează ca agent cu tool use. Primește un obiectiv, decide singur ce fișiere citește, ce comenzi rulează, ce modificări propune. Implicația practică: prompt-urile arată diferit. Îi ceri obiectiv de rezolvat, nu fragment de cod de completat.

---

## Primul prompt și primul diff

Presupunem că ai parcurs 1.1 Instalare: VS Code e deschis, extensia Claude Code instalată, ești logat cu contul Anthropic, iconița Spark apare în Activity Bar. Deschide un folder oarecare cu `File > Open Folder` (alege un folder cu câteva fișiere, nu gol). Click pe Spark, panoul Claude se deschide.

### Primul prompt

În câmpul de input al panoului, scrii:

> **Prompt:**
> "Spune-mi ce fișiere sunt în workspace-ul curent și la ce par să servească."
>
> **Ce ar trebui să vezi:**
> Claude rulează un tool call vizibil în conversație (ex. `List` sau `Bash`), apoi dă un rezumat. Tool call-ul apare ca bloc expandabil, poți da click să vezi exact ce a executat.

### Primul diff

Scrii al doilea prompt care forțează o modificare:

> **Prompt:**
> "Creează un fișier test.md în workspace cu trei linii despre ce ai găsit aici."
>
> **Ce ar trebui să vezi:**
> Claude deschide un diff side-by-side cu conținutul propus. Ai trei opțiuni: accept (aplică diff-ul), reject (renunți), sau să-i spui lui Claude ce să facă altfel (rescrie propunerea).

Dacă ambele au mers, înseamnă că Claude Code e complet funcțional și ai parcurs ciclul complet de bază: prompt → tool call → răspuns, apoi prompt → diff → accept.

> ⚠️ Contul gratuit Claude.ai nu include acces la Claude Code. Ai nevoie de Pro, Max, Team, Enterprise sau Console account. Dacă primești eroare de acces la signin, probabil asta e cauza. Pentru solopreneur, planul Pro e minimul.

---

## Referință

### Elementele UI pe care le vezi

- **Spark icon**: iconița care marchează Claude Code. Apare în Activity Bar (sidebar stâng, totdeauna vizibilă) și în Editor Toolbar (top-right în editor).
- **Panelul Claude Code**: locul conversației. Se deschide cu click pe Spark. Poate fi mutat prin drag în secondary sidebar (dreapta), primary sidebar (lângă Explorer) sau ca tab în editor.
- **Status bar**: bottom-right în VS Code, afișează `✱ Claude Code` când extensia e activă.
- **Câmpul de input**: în panou, scrii prompt-urile aici. Suportă `@` pentru referențiere fișiere (detalii în 1.2).
- **Tool calls vizibile**: fiecare pas executat de Claude apare ca bloc separat, expandabil, în conversație.
- **Diff side-by-side**: modificările la fișiere se propun prin diff vizual, nu se aplică direct.
- **Opțiunile pe diff**: accept, reject, sau instrucțiune nouă către Claude.
- **Eye-slash icon**: ascunde selecția de Claude când nu vrei să-i dai context din editor.
- **X pe attachments**: elimină un fișier referențiat din context.
- **Dropdown conversation history**: top panel, accesezi conversațiile anterioare.

### Comenzi și shortcut-uri esențiale

| Scop | Comandă/shortcut | Context |
|---|---|---|
| Deschide Extensions view | `⌘+Shift+X` / `Ctrl+Shift+X` | VS Code global |
| Command Palette | `⌘+Shift+P` / `Ctrl+Shift+P` | VS Code global |
| Reîncarcă VS Code (troubleshooting) | Command Palette → "Developer: Reload Window" | Când extensia nu apare |
| Deschide panel Claude | Click pe Spark icon | Activity Bar sau Editor Toolbar |
| Trimite prompt | `Enter` | În panel |
| Linie nouă în prompt | `Shift+Enter` | În panel |

### Formatul unui prompt bun la nivelul ăsta

Scopul clar în prima linie. Context opțional. Constrângeri la final.

> **Exemplu:**
> "Caută în workspace toate fișierele .md care menționează 'TODO' și fă-mi un rezumat. Nu modifica nimic."

### Prompt cu cod inclus

Când promptul conține cod (schiță, snippet de integrat, exemplu de rafinat), structura e: blockquote pentru instrucțiune, code fence separat cu limbaj specificat, blockquote pentru rezultatul așteptat.

> **Prompt:**
> "Adaugă funcția de mai jos în utils.ts și actualizează importul din index.ts ca să o folosească."

```typescript
export function calcTVA(pret: number): number {
  return pret * 0.19;
}
```

> **Ce ar trebui să vezi:**
> Claude propune două diff-uri succesive. Primul pentru utils.ts cu funcția adăugată. Al doilea pentru index.ts cu importul actualizat.

---

## Capcane frecvente

Capcane acoperite aici: prompt-uri stil Copilot, respingere reflexă a diff-urilor, panel dispărut, confuzie Claude Code versus Claude pe claude.ai, încercare cu cont gratuit.

### Prompt-urile stil Copilot nu funcționează

**Simptom:** Scrii `// funcție care calculează TVA` într-un fișier și aștepți completare. Nu se întâmplă nimic.
**Cauză:** Claude Code funcționează ca agent, ascultă doar prompt-urile din panel. Nu ascultă în fundal la ce scrii în editor.
**Fix:** Scrie prompt-uri orientate spre obiectiv în panel. "Adaugă în utils.ts o funcție care calculează TVA pentru România" funcționează.

### Respingerea diff-urilor din reflex, ciclu infinit

**Simptom:** Claude propune o schimbare, apeși reject fără să citești, ceri iar, se repetă.
**Cauză:** Reflex de control din IDE-uri tradiționale. Diff-ul rămâne revizuibil până la accept, nu se aplică automat.
**Fix:** Citește diff-ul, identifică ce nu-ți place, folosește a treia opțiune (spune-i lui Claude ce să facă altfel) în loc de reject. "Pune-l în alt fișier" sau "fără comentarii auto-generate".

### Panelul a dispărut și crezi că extensia nu merge

**Simptom:** Nu mai vezi panelul Claude, crezi că s-a dezinstalat.
**Cauză:** L-ai drag-at din greșeală în altă poziție, sau VS Code a reorganizat layout-ul.
**Fix:** Click pe Spark icon din Activity Bar sau Editor Toolbar. Dacă tot nu apare, Command Palette → "View: Reset View Locations".

### Confuzia Claude Code versus Claude pe claude.ai

**Simptom:** Ceri Claude Code să rezume un URL din web. Răspunde că nu poate.
**Cauză:** Claude Code e agent local pe fișierele tale. Nu are browser sau acces direct la web fără MCP dedicat.
**Fix:** Pentru conținut web, rămâi pe claude.ai sau instalezi un MCP de browsing (vezi 2.2). Pentru PDF-uri, pune-le în workspace și le referențiezi.

### Încercarea cu cont gratuit

**Simptom:** Instalezi extensia, dai signin, primești eroare de acces.
**Cauză:** Contul gratuit Claude.ai nu include Claude Code.
**Fix:** Upgrade la Pro (minimul pentru solopreneur), sau Max/Team/Enterprise dacă ai caz special. Alternativ, Console account cu API credits (plată per-usage, nu abonament).

---

## Următorul pas

→ [1.3 Integrare cu editorul](integrare-editor.md). Cum folosești `@`-mentions, shortcut-ul `Option+K`/`Alt+K` pentru selecție, și context automat ca să nu mai explici fișiere prin text.

---

## Surse

- [Use Claude Code in VS Code (ide-integrations)](https://docs.anthropic.com/en/docs/claude-code/ide-integrations)
- [Claude Code overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Interactive mode](https://docs.anthropic.com/en/docs/claude-code/interactive-mode)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
