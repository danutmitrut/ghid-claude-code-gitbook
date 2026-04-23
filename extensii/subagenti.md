*Partea II, Extensii > 2.1 Subagenți*

# 2.1 Subagenți

**Cum deleghezi task-urile cu multe subetape la un al doilea Claude cu context izolat, care lucrează separat și îți aduce doar rezultatul. Fără să-ți umple conversația principală.**

| Meta | |
|---|---|
| Timp de setup | 30 minute |
| Timp de învățare până la fluență | 1-2 săptămâni |
| Valoare pentru solopreneur | Mediu, util la task-uri cu mulți pași sau verificări independente |
| Dependențe | 1.1 până la 1.5 complete |

**Acoperă:** definire subagent într-un fișier din `.claude/agents/` cu YAML frontmatter (`name`, `description`, `tools` opțional), invocare autonomă de Claude principal pe baza descrierii, context izolat default (subagentul nu vede conversația părinte), opțiunea `isolation: worktree` pentru copie izolată de repo, locații `~/.claude/agents/` (user, peste toate proiectele) și `.claude/agents/` (proiect, partajabil cu echipa).
**Nu acoperă:** conectare la sisteme externe ca Gmail sau Notion (vezi 2.2), automatizare pe evenimente prin hooks (vezi 2.3), skills cu metodologie capturată (vezi 3.1).

---

## TL;DR

Subagentul e un al doilea Claude, definit într-un fișier text, pe care Claude principal îl invocă când are nevoie de ajutor pe un task specific. Rulează într-o conversație proprie (nu vede istoricul tău, nu îți umple fereastra principală), face munca, îți întoarce doar rezultatul final. Definești subagentul o dată, îl invocă automat când e potrivit. Exemple tipice pentru solopreneur: un subagent de code review care verifică fiecare edit important, un subagent de cercetare care scanează codebase-ul pentru pattern-uri existente.

---

## De ce contează

Ai întâlnit probabil situația asta. Întrebi Claude ceva aparent simplu ("există deja o funcție pentru validare email?"). El deschide 20 de fișiere, citește pe rând, îți răspunde corect, dar acum conversația voastră e plină de tool call-uri din citirea fișierelor. Următoarea ta întrebare e mai grea, pentru că Claude împarte atenția între munca ta și ce a citit el.

E un cost ascuns al modului în care Claude explorează autonom. Fiecare pas pe care îl face lasă urmă în context. După suficiente explorări, conversația devine lentă, Claude devine confuz, tu ajungi la `/compact` sau `/clear` înainte să termini task-ul principal.

Subagenții rezolvă asta prin delegare. În loc ca Claude principal să facă munca grea și să plătească costul de context, el cheamă un subagent dedicat. Subagentul are conversația lui proprie, face explorarea acolo, întoarce doar răspunsul esențial. Claude principal primește "uite, răspunsul e X, iată unde l-am găsit", fără să înghită toate tool call-urile.

Analogia cea mai aproape e freelancer-ul. Când ai un proiect mare și angajezi pe cineva pentru o parte specifică (research, design, audit), nu stai cu el la birou să vezi fiecare pas. Îi dai brief-ul, el lucrează singur, îți aduce livrabilul. Exact așa funcționează subagentul. Tu (sau Claude principal) dai brief-ul, el lucrează separat, îți aduce rezultatul.

---

## Ce vei putea face după

- Creezi un subagent definit într-un fișier Markdown cu YAML frontmatter
- Scrii descrieri clare care fac Claude principal să-l invoce la momentul potrivit
- Alegi între subagenți la nivel de user (peste toate proiectele) și de proiect
- Limitezi tool-urile pe care le poate folosi subagentul, pentru siguranță
- Știi când folosești subagent și când preferi să lucrezi direct

---

## Modelul mental: freelancer într-o cameră separată

Claude principal (cel cu care vorbești în panel) are acces la conversația voastră, la fișierele pe care le-ai atins, la tot istoricul. E colegul tău de birou.

Subagentul e freelancer-ul din camera alăturată. Când Claude principal decide că are nevoie de el, îi deschide o ușă, îi dă un brief (o instrucțiune scurtă, ca un prompt separat), închide ușa. Freelancer-ul lucrează în camera lui, are tool-urile lui, are contextul pe care i l-ai dat tu în fișierul de definiție plus brief-ul momentului. Nu aude ce vorbiți voi în biroul principal. Când termină, redeschide ușa și îți întoarce rezultatul.

Două implicații importante ale modelului ăsta.

**Subagentul nu știe ce ai discutat.** Tot ce trebuie să facă munca lui, trebuie să fie fie în fișierul de definiție (system prompt-ul lui), fie în brief-ul pe care Claude principal îl transmite la invocare. Dacă te aștepți ca subagentul să "înțeleagă" din conversația voastră anterioară, nu va funcționa. Trebuie să-i dai tot contextul necesar explicit.

**Rezultatul subagentului e un mesaj final, nu un traseu.** Tu vezi concluzia ("am găsit pattern-ul X în 5 locuri, iată-le"), nu fiecare tool call pe care l-a făcut ca să ajungă acolo. Asta e chiar beneficiul: contextul tău rămâne curat. Dacă vrei să vezi cum a lucrat, poți cere subagentul să scrie și procesul, dar oricum rezultatul final e un mesaj scurt, nu o sesiune lungă.

> ⚠️ Implicația practică: subagenții sunt buni pentru task-uri unde brief-ul poate fi formulat clar și unde rezultatul final încape într-un mesaj (câteva paragrafe, o listă, un fragment de cod). Pentru lucruri în care tu trebuie să vezi pașii intermediari și să intervii la fiecare, subagentul nu e răspunsul. Lucrezi direct cu Claude principal.

---

## Cum folosești

### Creezi un fișier de definiție

Subagenții trăiesc în dosarul `.claude/agents/` din proiect sau în `~/.claude/agents/` pentru user global (valabili peste toate proiectele tale). Fiecare subagent e un fișier Markdown.

Structura fișierului are două părți: un bloc de configurare (numit YAML frontmatter, delimitat de `---` la început și la sfârșit) și corpul (instrucțiunile pentru subagent).

Exemplu concret, `.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: Revizuiește cod nou sau modificat pentru bug-uri logice, probleme de securitate și claritate. Invocă-l după edit-uri importante sau înainte de a finaliza un task de dezvoltare.
tools: Read, Grep, Glob
---

Ești un reviewer de cod exigent, dar constructiv. Analizezi codul primit pentru:

- Bug-uri logice și edge case-uri neacoperite
- Probleme de securitate (SQL injection, XSS, secrete expuse, validare lipsă)
- Claritate și lizibilitate (nume ambigue, funcții prea lungi, logică implicită)
- Probleme de mentenabilitate (cuplaj excesiv, repetare, magie fără comentariu)

Structura răspunsului tău:
1. Sumar: două propoziții despre impresia generală
2. Probleme critice: bug-uri sau vulnerabilități care trebuie rezolvate
3. Sugestii de îmbunătățire: lucruri opționale care ar ajuta
4. Ce e bine: recunoaște explicit deciziile bune

Răspunde în română cu diacritice. Ton direct, fără preambule.
```

Să desfacem ce am scris.

**`name:`** numele subagentului, fără spații. Apare ca referință internă.

**`description:`** descrierea e cel mai important câmp. Pe baza ei, Claude principal decide dacă să-l invoce. Formulează descrierea ca răspuns la întrebarea "când ar trebui folosit acest subagent?". Vagă ("verifică cod") = rar invocat. Precisă ("revizuiește cod nou pentru bug-uri și securitate după edit-uri importante") = invocat la momentul potrivit.

**`tools:`** lista de unelte pe care are voie să le folosească. Opțional. Dacă îl omiți, subagentul moștenește toate tool-urile de la Claude principal. Pentru un reviewer, `Read, Grep, Glob` e suficient (citește cod, caută pattern-uri, listează fișiere). Nu îi dai `Edit` sau `Bash` pentru că rolul lui e să analizeze, nu să modifice.

**Corpul (după `---`)** e system prompt-ul subagentului. Aici îi spui cine e, ce face, cum răspunde. E echivalentul instrucțiunii permanente pe care un freelancer ar avea pe peretele biroului lui.

### Cum îl invocă Claude principal

Nu trebuie să spui explicit "cheamă subagentul X". Claude principal citește descrierea tuturor subagenților disponibili la începutul sesiunii. Când tu îi ceri ceva, el decide autonom dacă are sens să delegheze.

Exemplu de flux:

> **Tu în panel:** "Am terminat de scris funcția de import CSV din `@src/utils/csv-import.ts`. Vreau să arunci o privire."
>
> **Ce face Claude principal:** Vede că descrierea `code-reviewer` se potrivește ("revizuiește cod după edit-uri importante"). Îl invocă pe el. Îi dă un brief ("revizuiește funcția `parseCSVImport` din `src/utils/csv-import.ts`"). Subagentul citește fișierul, analizează, întoarce raportul.
>
> **Ce vezi tu:** Un mesaj de la Claude principal care zice "am rugat code-reviewer-ul să arunce o privire" (sau similar), urmat de raportul structurat al subagentului.

Pentru moment, mecanica exactă de invocare e gestionată de Claude. Tu nu apăși nimic special. Ce faci tu e să scrii descrieri clare, ca invocarea să se întâmple când trebuie.

### Forțezi invocare manuală

Dacă vrei să fii sigur că un anumit subagent e folosit, îl poți cere explicit.

> **Prompt:**
> "Folosește subagentul `code-reviewer` să verifice @src/utils/csv-import.ts."
>
> **Ce ar trebui să vezi:**
> Claude principal invocă subagentul numit, îi transmite fișierul, întoarce raportul.

Util când descrierea e ambiguă și nu știi dacă invocarea s-ar fi întâmplat automat, sau când testezi subagenți noi.

### Context izolat default, `isolation: worktree` opțional

Default, subagentul rulează într-o conversație izolată, fără acces la conversația ta curentă, dar operează pe același filesystem. Poate citi și modifica (dacă are tool-urile) aceleași fișiere ca Claude principal.

Pentru situațiile în care vrei ca subagentul să lucreze pe o copie complet separată a proiectului (util pentru experimente care nu trebuie să afecteze working directory-ul tău), adaugi în frontmatter:

```markdown
---
name: experimenter
description: Rulează experimente pe cod fără să afecteze starea curentă.
tools: Read, Edit, Bash
isolation: worktree
---
```

Cu `isolation: worktree`, subagentul primește o copie izolată a repo-ului (într-un Git worktree). Modificările lui nu ajung în fișierele tale reale decât dacă alegi explicit să le integrezi.

> **Caveat pentru nontehnici:** `worktree` e un concept din Git. Necesită proiect inițializat cu Git și înțelegerea de bază a branch-urilor. Dacă nu ești acolo încă, sari peste opțiunea asta și folosește context izolat default (care e deja suficient pentru majoritatea cazurilor).

---

## Exemple utile pentru solopreneur

Trei subagenți care acoperă majoritatea nevoilor unui solopreneur nontehnic care lucrează cu Claude Code zilnic.

### Code reviewer

Așa cum am arătat mai sus. Revizuiește cod după edit-uri. Tool-uri: `Read, Grep, Glob`. Util înainte de a considera un task terminat.

### Researcher

Scanează codebase-ul să răspundă la întrebări "există deja?", "cum e implementat X?", "unde se folosește Y?".

```markdown
---
name: codebase-researcher
description: Scanează codebase-ul pentru a răspunde la întrebări de tip "există deja?", "cum e implementat?", "unde se folosește?". Invocă-l când trebuie să înțelegi codul existent înainte să scrii cod nou.
tools: Read, Grep, Glob
---

Ești un cercetător de cod. Când primești o întrebare despre codebase:

1. Folosește Grep și Glob să găsești fișiere relevante
2. Citește fișierele găsite
3. Întoarce un raport cu: ce ai găsit, unde (path exact), cu fragmente scurte de cod ca exemple

Nu modifici niciodată cod. Doar răspunzi cu ce ai descoperit. Răspunsuri concise, fără preambul.
```

Util înainte de a începe un feature nou, pentru a evita duplicarea.

### Writer de documentație

Pentru solopreneurii care livrează produse sau colaborează cu alții, documentația se amână mereu. Un subagent dedicat îți scrie draft-uri de documentație după ce închei un feature.

```markdown
---
name: doc-writer
description: Scrie draft de documentație pentru o funcție, componentă sau feature nou. Invocă-l după ce ai terminat o piesă de cod importantă care va fi folosită de alții.
tools: Read, Grep, Write
---

Ești un writer tehnic care produce documentație clară, nu academică. Pentru fiecare piesă de cod primită:

1. Citește codul și înțelege ce face
2. Scrie un draft în Markdown cu: scop într-o frază, exemplu de utilizare, parametri/argumente, valori returnate, limitări cunoscute
3. Salvează în `docs/` cu nume potrivit

Stil: concis, orientat pe cazul de utilizare. Evită jargon inutil. Exemple concrete peste abstracțiuni.
```

---

## Referință

### Locații pentru subagenți

| Locație | Scop | Când folosești |
|---|---|---|
| `~/.claude/agents/` | User global, toate proiectele | Subagenți generici (code review, research) care ajută indiferent de proiect |
| `.claude/agents/` | Proiect specific, partajabil cu echipa | Subagenți specializați pe contextul proiectului (domeniu, stack) |

### Câmpurile YAML frontmatter

| Câmp | Obligatoriu | Ce face |
|---|---|---|
| `name` | Da | Nume intern al subagentului, fără spații |
| `description` | Da | Criteriul pe baza căruia Claude principal decide când să-l invoce |
| `tools` | Nu | Lista de unelte permise. Dacă lipsește, moștenește tot |
| `isolation` | Nu | `worktree` pentru copie izolată de repo |

### Format fișier

```markdown
---
name: nume-subagent
description: Când și cum să fie folosit
tools: Tool1, Tool2, Tool3
---

System prompt-ul subagentului. Aici scrii cine e, ce face,
cum răspunde, ce stil adoptă.
```

### Ce primește și ce întoarce un subagent

- **Primește:** un prompt de brief de la Claude principal (instrucțiunea specifică pentru invocarea curentă), plus system prompt-ul propriu din fișierul de definiție
- **Nu primește:** conversația ta cu Claude principal, tool call-urile anterioare, CLAUDE.md-ul personal (dar CLAUDE.md de proiect se aplică la fel)
- **Întoarce:** un singur mesaj final cu rezultatul muncii lui

---

## Capcane frecvente

Capcane acoperite: descriere vagă care nu declanșează invocarea, subagent creat pentru task prea simplu, tool-uri prea restrictive, rezultat care oricum umple contextul, confuzie între subagent și skill.

### Descriere vagă, subagent niciodată invocat

**Simptom:** Ai scris un subagent, ești sigur că ar fi util, dar Claude principal nu-l invocă niciodată. Faci task după task, subagentul ignorat.
**Cauză:** Descrierea e prea generală. "Ajută cu cod" sau "util pentru verificări" nu spun lui Claude principal când să-l invoce.
**Fix:** Rescrie descrierea ca răspuns la întrebarea "când ar fi acest subagent invocat". Concret, nu generic. "Revizuiește cod TypeScript după edit-uri importante pentru probleme de type safety" e mult mai util decât "verifică cod". Include context specific (tipul task-ului, momentul, cuvinte cheie care ar apărea în conversație).

### Subagent creat pentru task prea simplu

**Simptom:** Ai definit un subagent care face o singură chemare de Grep. Îl invoci, simte greoi, nu ai beneficiul pe care îl așteptai.
**Cauză:** Subagenții au overhead (o conversație nouă, tool call-uri proprii, trecere de rezultat înapoi). Pentru task-uri simple, Claude principal îl face direct mai rapid.
**Fix:** Folosește subagenți pentru task-uri cu 3+ pași sau pentru verificări independente unde vrei context curat. Pentru orice task care ar fi rezolvat de Claude principal în 1-2 tool call-uri, rămâi pe direct.

### Tool-uri prea restrictive

**Simptom:** Subagentul e invocat, încearcă să facă munca, eșuează pentru că nu are acces la un tool de care are nevoie. Raportul revine cu "nu am putut, nu am tool-ul X".
**Cauză:** Ai limitat `tools` mai mult decât trebuie. De exemplu, i-ai dat doar `Read` dar task-ul cere și `Bash` (rulare de teste).
**Fix:** Regândește tool-urile necesare pentru task-ul tipic. Dacă nu ești sigur, omite câmpul `tools` complet (moștenește toate). Restrânge doar dacă ai motive de securitate (ex. reviewer care sigur nu ar trebui să modifice).

### Rezultat prea mare revine și umple oricum contextul

**Simptom:** Ai invocat un subagent pentru a-ți economisi contextul, dar el îți întoarce un raport de 500 de linii. Beneficiul a dispărut.
**Cauză:** System prompt-ul subagentului nu cere concizie. Claude-ul ca subagent, la fel ca Claude principal, tinde să fie detaliat dacă nu-i spui altfel.
**Fix:** În system prompt-ul subagentului, specifică lungimea așteptată a răspunsului. "Raport concis, maximum 20 de puncte." Sau "sumar în trei secțiuni, fiecare de maximum cinci rânduri." Structura clară disciplinează output-ul.

### Confuzie între subagent și skill

**Simptom:** Nu înțelegi când ai nevoie de subagent și când de skill. Creezi subagenți pentru orice, sau creezi skill-uri pentru totul.
**Cauză:** Ambele capturi metodologie, dar fac lucruri diferite.
**Fix:** Subagenții sunt pentru task-uri care se execută într-o conversație separată cu context izolat (când vrei delegare). Skills sunt pentru metodologie care modifică comportamentul lui Claude principal în conversația ta (când vrei ca Claude să aplice un pattern pe care îl ai tu). Pentru detalii despre skills, vezi 3.1.

---

## Următorul pas

→ [2.2 MCP servers](mcp-servers.md). Până acum, Claude a lucrat pe fișierele din workspace-ul tău. Pasul următor e să-i dai acces la sisteme externe: Gmail, Notion, GitHub, baze de date, API-uri. Configurat prin `.mcp.json`, gestionat prin slash commands.

---

## Surse

- [Create custom subagents](https://docs.claude.com/en/docs/claude-code/sub-agents)
- [Subagents in the SDK](https://docs.claude.com/en/docs/agent-sdk/subagents)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
