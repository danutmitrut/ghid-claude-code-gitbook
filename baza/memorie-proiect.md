*Partea I, Bază > 1.5 Memorie de proiect*

# 1.5 Memorie de proiect

**Cum scrii o dată lucrurile importante despre proiect și Claude le ține minte în toate sesiunile, fără să repeți. CLAUDE.md, `/compact`, `/clear`, slash commands proprii.**

| Meta | |
|---|---|
| Timp de setup | 30 minute |
| Timp de învățare până la fluență | 1-2 săptămâni pe proiecte reale |
| Valoare pentru solopreneur | Mare, nu mai explici context la fiecare sesiune |
| Dependențe | 1.1, 1.2, 1.3 și 1.4 complete |

**Acoperă:** `CLAUDE.md` la rădăcina proiectului pentru context specific proiectului, `~/.claude/CLAUDE.md` pentru preferințe personale valabile peste tot, ierarhia de încărcare, notația `@` pentru includere externă, HTML comments pentru note fără cost, `/compact` pentru compactare istoric, `/clear` pentru sesiune nouă, slash commands proprii în `.claude/commands/`.
**Nu acoperă:** skills complete cu YAML frontmatter (vezi 3.1), plugins pentru distribuire (vezi 3.2), memorie automată scrisă de Claude pe baza corecțiilor tale (subiect avansat, menționat doar în treacăt).

---

## TL;DR

CLAUDE.md e un fișier text unde scrii ce ar trebui Claude să știe despre proiectul tău: convenții, decizii, comenzi utile, preferințe de scriere. Îl pui la rădăcina proiectului și Claude îl încarcă automat la începutul fiecărei sesiuni. Poți avea și unul personal, în `~/.claude/CLAUDE.md`, valabil peste toate proiectele (ex. "răspunde-mi în română"). `/compact` îți comprimă istoricul când se umple fereastra, `/clear` pornește sesiune nouă. Pentru task-uri repetitive, îți faci slash commands proprii în `.claude/commands/`.

---

## De ce contează

În primele sesiuni cu Claude, simți o repetiție enervantă. Explici ce e proiectul. Spui că folosești Python, nu TypeScript. Îl rogi să răspundă în română. Menționezi că preferă teste pytest, nu unittest. După cinci sesiuni, ai explicat aceleași lucruri de cinci ori.

CLAUDE.md rezolvă asta prin delegare către un fișier. Scrii acolo ce ar trebui Claude să știe mereu despre proiectul tău sau despre cum lucrezi. Claude citește fișierul la începutul sesiunii și intră deja cu contextul încărcat. Tu nu mai repeți, el nu mai întreabă.

Există și o a doua problemă pe care o rezolvă: pierderea contextului într-o sesiune lungă. Când conversația se lungește, fereastra lui Claude se umple și începe să uite lucrurile de la început. `/compact` comprimă inteligent (păstrează esența, aruncă detaliile) și eliberează spațiu, re-citind CLAUDE.md ca să rămână ancorat.

A treia problemă, mai subtilă: ai task-uri pe care le repeți. "Scrie-mi un email de follow-up, stilul meu, cu structura X". Îl ceri săptămânal, îl formulezi mereu un pic diferit, rezultatele variază. Cu un slash command custom (ex. `/follow-up`), ai comanda scurtă care execută mereu același prompt bine formulat.

---

## Ce vei putea face după

- Creezi un `CLAUDE.md` la rădăcina proiectului cu convențiile tale de lucru
- Ai un `~/.claude/CLAUDE.md` personal cu preferințe valabile peste tot
- Folosești `/compact` când sesiunea se lungește și `/clear` când începi curat
- Referențiezi alte fișiere în CLAUDE.md prin `@` ca să nu dublu-documentezi
- Îți faci primele slash commands proprii pentru task-uri repetitive

---

## Modelul mental: două tipuri de memorie

Claude Code recunoaște două feluri de memorie persistentă.

**Memoria personală.** Trăiește în `~/.claude/CLAUDE.md`, pe computerul tău, valabilă peste toate proiectele. Aici pui lucruri care te caracterizează pe tine, nu proiectul. "Răspunde în română cu diacritice", "preferă explicații narative, nu liste de bullet-uri", "când scrii cod, adaugă type hints în Python".

**Memoria proiectului.** Trăiește în proiectul tău, la rădăcină sau în dosarul `.claude/`. Valabilă doar în proiectul ăla. Aici pui "acest proiect folosește React 18 cu TypeScript, gestiune de state cu Zustand, stil în Tailwind". Sau "regula aici e că fișierele de date stau în `data/`, niciodată în `src/`".

Când deschizi o sesiune într-un proiect, Claude încarcă ambele: întâi personal (context general despre tine), apoi proiect (context specific). Dacă amândouă menționează ceva, mai aproape de proiect câștigă (mai specific).

> ⚠️ Implicația practică: nu dublezi informația. Dacă pui "răspunde în română" în `~/.claude/CLAUDE.md`, nu mai trebuie să pui în CLAUDE.md de proiect. Dacă un proiect e în engleză (pentru un client internațional), acolo specifici "pentru proiectul ăsta, răspunde în engleză" și se suprascrie personalul.

---

## Cum folosești

### CLAUDE.md la rădăcina proiectului

Creezi un fișier text numit `CLAUDE.md` la rădăcina proiectului (lângă `package.json`, `pyproject.toml` sau ce ai acolo). Îl scrii în format Markdown obișnuit.

Conținut util pentru un solopreneur:

```markdown
# Context proiect

Aplicație de facturare pentru freelanceri români. Stack: Next.js 14, TypeScript, Prisma, PostgreSQL.

## Convenții

- Componente React în `src/components/`, fiecare în dosarul propriu cu index.tsx
- Testele stau lângă fișierul testat, cu sufix `.test.ts`
- Pentru formulare, folosim react-hook-form plus zod pentru validare
- Limbă interfață: română. Erori și log-uri: engleză.

## Comenzi frecvente

- Rulare dev: `npm run dev`
- Rulare teste: `npm test`
- Migrare DB: `npx prisma migrate dev`

## Decizii importante

- Nu folosim Redux. Am încercat, e prea mult pentru scara asta.
- API-ul e REST, nu GraphQL (schimbat decizia în martie 2026).
```

Salvezi, deschizi o sesiune nouă cu Claude, el citește fișierul automat. De aici înainte, când îi ceri ceva, știe deja contextul.

### CLAUDE.md personal în `~/.claude/`

Creezi `~/.claude/CLAUDE.md` pentru preferințe valabile peste tot. Pe macOS și Linux e `/Users/numele-tau/.claude/CLAUDE.md`. Pe Windows e `C:\Users\numele-tau\.claude\CLAUDE.md`.

Exemplu:

```markdown
# Preferințe personale

- Răspunde-mi în română cu diacritice
- Scris dens, fără preambule de politețe
- La cod, adaugă comentarii doar când logica nu e evidentă din nume
- Folosesc VS Code pe macOS, shortcut-urile mele sunt mac-style
```

Claude intră în fiecare sesiune cu aceste preferințe încărcate. Îți scutește explicațiile repetate.

### Notația `@` pentru includere externă

Dacă ai documentație existentă care descrie proiectul, nu dublezi. În CLAUDE.md folosești `@` ca să incluzi alt fișier.

```markdown
# Context proiect

Aplicație de facturare. Detalii tehnice complete:

@docs/architecture.md

## Convenții de cod

@docs/coding-conventions.md
```

Când Claude încarcă CLAUDE.md, injectează și conținutul fișierelor referențiate, ca intrări separate în context. Utile când ai deja un `docs/` bine scris și nu vrei să-l rescrii.

### HTML comments pentru note fără cost

Dacă vrei să lași note pentru tine (sau pentru colegul tău care deschide CLAUDE.md), dar fără ca Claude să le consume ca tokens, folosești HTML comments. Ele sunt eliminate înainte de a fi trimise către Claude.

```markdown
# Context proiect

Aplicație de facturare pentru România.

<!-- Notă pentru mine: data completă a re-branding e 15 mai,
     până atunci folosesc numele vechi Bolta. După, se cheamă Ridica. -->

## Stack
```

Claude nu "vede" comentariul, tu îl vezi.

### `/compact` și `/clear`

Pe măsură ce sesiunea se lungește, fereastra de context se umple. Claude începe să uite detalii din mijloc. Ai două unelte.

**`/compact`.** Comprimă istoricul existent. Claude rezumă conversația de până acum într-un bloc scurt, aruncă detaliile repetitive, păstrează esența. După compactare, re-citește CLAUDE.md de la rădăcina proiectului și îl reinjectează, ca să rămâi ancorat. Util când vrei să continui aceeași sesiune fără să pierzi contextul general.

> ⚠️ `/compact` nu reîncarcă automat fișierele CLAUDE.md din subdirectoare. Dacă ai context important într-un subdirectoar, Claude îl va reîncărca natural data următoare când citește un fișier de acolo.

**`/clear`.** Șterge conversația curentă, pornești de la zero într-o sesiune nouă. Util când termini un task și începi unul complet diferit. Istoricul rămâne salvat în dropdown, nu pierzi nimic. Doar resetezi "placa" pentru un nou început.

> **Când folosești care:** `/compact` când vrei continuitate tematică dar fereastra se umple. `/clear` când schimbi complet subiectul și nu vrei să amesteci contexte.

### Slash commands proprii în `.claude/commands/`

Ai un task repetitiv. În loc să formulezi de fiecare dată același prompt, faci un slash command propriu.

Creezi dosarul `.claude/commands/` în proiectul tău (dacă nu există). Apoi un fișier Markdown cu numele comenzii pe care o vrei. Exemplu, `.claude/commands/follow-up.md`:

```markdown
Scrie un email de follow-up pe baza conversației sau a notelor despre întâlnire.

Structura:
- Salut scurt, prenume
- Trei bullet-uri cu punctele cheie discutate
- Un singur next step concret, cu deadline dacă există
- Semnătura mea: "Dan Mitrut, arhitect AI"

Ton: profesional direct, nu prea cald, nu prea rece. În română cu diacritice.
```

Salvezi. În panel tastezi `/follow-up`, apare în meniu, îl selectezi. Claude execută promptul definit în fișier.

Pentru comenzi valabile peste toate proiectele, le pui în `~/.claude/commands/` în loc de `.claude/commands/`.

> **Legătură către 3.1 Skills.** Slash commands simple ajung cu `.claude/commands/`. Când vrei funcționalitate mai puternică (instrucțiuni cu fișiere auxiliare, invocare automată de Claude, permisiuni de tool-uri), construiești skills complete cu YAML frontmatter. Vezi 3.1.

---

## Referință

### Ierarhia de încărcare CLAUDE.md

| Nivel | Locație | Scop | Când îl folosești |
|---|---|---|---|
| Personal | `~/.claude/CLAUDE.md` | Preferințe tale globale | Răspund în RO, preferințe de scriere, stil cod |
| Proiect (rădăcină) | `CLAUDE.md` în rădăcina proiectului | Context proiect | Stack, convenții, comenzi, decizii |
| Proiect (dedicat) | `.claude/CLAUDE.md` în proiect | Context proiect, variantă alternativă | Unele echipe preferă să nu polueze rădăcina |
| Subdirectoare | `CLAUDE.md` într-un subdirectoar | Context pentru acel dosar | Dosar specializat cu reguli proprii |

**Ordinea de încărcare:** personal mai întâi, proiect peste. Dacă se suprapun, proiectul câștigă (mai specific).

**Subdirectoarele** nu se încarcă la start. Se încarcă on-demand, când Claude citește un fișier din dosarul respectiv.

### Slash commands built-in esențiale

| Comandă | Ce face | Când o folosești |
|---|---|---|
| `/compact` | Comprimă istoricul, re-citește CLAUDE.md | Sesiune lungă, context se umple |
| `/clear` | Resetează sesiunea curentă, istoric vechi rămâne salvat | Schimbi complet subiectul |
| `/resume` | Deschide picker cu sesiuni anterioare | Vrei să continui un task vechi |
| `/plan` | Intră în plan mode | Task sensibil sau explorare |

### Formatul unui slash command custom

- Fișier Markdown simplu: `.claude/commands/nume.md`
- Numele fișierului devine numele comenzii: `nume.md` → `/nume`
- Conținutul fișierului devine promptul executat
- Poți folosi `@` notation și aici pentru a include alt fișier

---

## Capcane frecvente

Capcane acoperite: CLAUDE.md prea lung, secrete comise accidental, dubluri între nivele, așteptare că `/compact` reîncarcă tot, slash command custom care nu apare în meniu.

### CLAUDE.md devenit prea lung

**Simptom:** Ai scris tot ce știi despre proiect în CLAUDE.md. Fișierul are 500 de linii. Claude pare distras, răspunde generic, ratează detalii.
**Cauză:** CLAUDE.md se încarcă în fereastra de context la fiecare start de sesiune. Dacă e prea mare, ocupă spațiu care ar fi fost folosit pentru conversație și gândire.
**Fix:** Păstrează CLAUDE.md concentrat (50-150 de linii). Mută secțiunile voluminoase în fișiere separate (`docs/architecture.md`, `docs/conventions.md`) și include-le prin `@docs/architecture.md`. Claude le citește la nevoie, nu la fiecare start.

### Secrete comise accidental în CLAUDE.md

**Simptom:** Ai pus chei API sau parole în CLAUDE.md pentru comoditate. Comiți în Git. Repo public. Panică.
**Cauză:** CLAUDE.md e un fișier text ca oricare altul. Git nu-i dă tratament special. Dacă e commit-uit, e public.
**Fix:** Niciodată secrete în CLAUDE.md. Pentru chei, folosește variabile de mediu și referențiezi convenția (`cheia e în .env, numele e OPENAI_API_KEY`). Pentru date sensibile temporare, `CLAUDE.local.md` e o variantă alternativă, pe care o adaugi în `.gitignore` ca să nu fie commit-uită.

### Dubluri între `~/.claude/CLAUDE.md` și `CLAUDE.md` de proiect

**Simptom:** Ai scris "răspunde în română" și într-unul, și în altul. Tot merge, dar fișierele au drift.
**Cauză:** Reflex de copy-paste. La un moment dat nu mai știi ce e în care.
**Fix:** Disciplină. În personal merg doar lucrurile care te caracterizează pe tine (limbă, ton, preferințe de cod general). În proiect merg doar specificele proiectului (stack, convenții, comenzi). Dacă te trezești că un proiect are reguli contrare celor personale (proiect în engleză, tu scrii în română în rest), specifici în CLAUDE.md de proiect și acela suprascrie.

### `/compact` a pierdut context din subdirectoare

**Simptom:** Ai făcut `/compact` într-o sesiune lungă. După, Claude pare să fi uitat reguli specifice unui subdirectoar în care lucrai.
**Cauză:** `/compact` re-citește CLAUDE.md de la rădăcină și cel personal, dar nu reîncarcă automat CLAUDE.md din subdirectoare. Acestea se încarcă on-demand când Claude citește un fișier de acolo.
**Fix:** După `/compact`, dacă știi că lucrezi în subdirectoarele cu reguli proprii, trimite un prompt care referențiază un fișier de acolo (ex. `@src/components/Form.tsx`) ca să forțezi reîncărcarea contextului local. Sau menționezi regulile în CLAUDE.md rădăcină dacă sunt critice.

### Slash command custom care nu apare în meniu

**Simptom:** Ai creat `.claude/commands/follow-up.md`. În panel scrii `/` și nu vezi `/follow-up` în listă.
**Cauză:** Fie e o problemă de path (fișierul nu e exact în `.claude/commands/`), fie numele are caractere speciale sau spații, fie sesiunea nu a preluat schimbarea.
**Fix:** Verifică path exact: `.claude/commands/follow-up.md` (lowercase, cratimă, nu spații). Închide și redeschide panelul sau fă `/clear`. Dacă tot nu apare, rulează `/help` și vezi dacă Claude îl listează acolo. Dacă lipsește, fișierul nu e găsit.

---

## Următorul pas

→ [2.1 Subagenți](../extensii/subagenti.md). Până acum ai folosit Claude într-un singur fir conversațional. Pentru task-uri cu multe subetape sau pentru verificări independente fără să îți poluezi conversația principală, deleghezi către un subagent, un al doilea Claude cu context izolat. Definit într-un fișier din `.claude/agents/`.

Alternativ, dacă vrei valoare directă imediat, treci la [2.2 MCP servers](../extensii/mcp-servers.md) care îți aduce Claude conectat la Gmail, Notion, GitHub.

---

## Surse

- [How Claude remembers your project (memory)](https://docs.claude.com/en/docs/claude-code/memory)
- [Extend Claude with skills (slash commands)](https://docs.claude.com/en/docs/claude-code/slash-commands)
- [Common workflows](https://docs.claude.com/en/docs/claude-code/common-workflows)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
