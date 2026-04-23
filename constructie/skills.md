*Partea III, Construcție > 3.1 Skills*

# 3.1 Skills

**Cum transformi metodologia ta de lucru, modul în care scrii, cum faci un review, cum analizezi un brief, într-un fișier pe care Claude îl încarcă singur când contextul cere și îl aplică disciplinat.**

| Meta | |
|---|---|
| Timp de setup | 2-4 ore pentru primul skill solid |
| Timp de învățare până la fluență | 1-2 luni, după ce ai două sau trei skills reale în folosință |
| Valoare pentru solopreneur | Mare. Aici îți multiplici metodologia ta proprie și o faci reutilizabilă |
| Dependențe | 1.5 Memorie de proiect completă, cunoaștere bună a propriului mod de lucru |

**Acoperă:** ce e un skill și diferența față de subagent, hook sau comandă custom, structura `SKILL.md` cu YAML frontmatter (`name`, `description`), descrierea scrisă la persoana a treia ca Claude să decidă autonom când îl încarcă, locațiile `~/.claude/skills/` (personal, peste toate proiectele) și `.claude/skills/` (proiect specific), invocare automată sau manuală prin slash command, suport pentru fișiere auxiliare (template-uri, referințe) în folderul skill-ului.
**Nu acoperă:** împachetare ca plugin distribuibil cu marketplace (vezi 3.2), skills încărcate via API headless (vezi 3.3), reguli de enterprise skills (nu sunt cazul solopreneurului).

---

## TL;DR

Un skill e un dosar cu un fișier principal numit `SKILL.md` în care scrii metodologia ta: cum scrii un articol, cum faci un review de cod, cum procesezi un brief de client. Fișierul are în cap un bloc YAML care spune lui Claude când să-l folosească (descriere clară, la persoana a treia). Claude citește descrierile tuturor skills-urilor la începutul sesiunii, iar când contextul tău match-uiește descrierea, încarcă skill-ul și aplică instrucțiunile din interior. Tu capitalizezi o dată munca de a articula cum lucrezi, iar Claude o aplică de fiecare dată.

---

## De ce contează

Fiecare solopreneur cu experiență a construit, în timp, un mod al lui de a face lucrurile. Cum structurezi un articol de blog ca să nu piardă cititorul. Cum articulezi o propunere comercială. Cum revizuiești un text înainte de publicare. Cum procesezi un feedback negativ fără să reacționezi emoțional. Aceste metodologii sunt parte din valoarea ta. Dar trăiesc în capul tău. Când lucrezi singur, le aplici automat. Când încerci să le explici altcuiva (sau lui Claude), durează mult și ieși cu rezultate inconsistente.

Skills-urile rezolvă asta. Scrii metodologia o dată, clar, într-un fișier. Claude o citește, o înțelege, o aplică. Nu mai trebuie să explici de fiecare dată "structurează articolul cu hook narativ, apoi problemă, apoi soluție, apoi call to action". Skill-ul conține structura. Tu îi dai subiectul, el aplică structura.

Beneficiul real apare după ce construiești primele două sau trei skills. Atunci vezi că Claude începe să "sune ca tine" pe lucruri pe care nu mai trebuie să le reexplici. Stilul tău de scriere, filtrele tale de review, cadrul tău de analiză. Sunt reutilizate consistent, proiect după proiect, fără să mai rescrii instrucțiunile.

Pentru solopreneuri care livrează cu stilul lor ca diferențiator (educatori, consultanți, coach-i, freelanceri specializați), skills-urile sunt cea mai mare pârghie de scale din ghid. Convertesc tacit în explicit, o dată, pentru totdeauna.

---

## Ce vei putea face după

- Scrii primul tău `SKILL.md` cu YAML frontmatter corect
- Formulezi descrieri care fac Claude să încarce skill-ul la momentul potrivit
- Alegi între skill la user (peste toate proiectele) și skill la proiect
- Incluzi fișiere auxiliare în folderul skill-ului (template-uri, exemple, referințe)
- Invoci un skill manual când vrei să fii sigur că e folosit
- Recunoști diferența între skill, subagent, hook și custom slash command

---

## Modelul mental: manual de proceduri pe care Claude îl consultă

Imaginează-ți că ai un manual cu toate procedurile tale scrise ordonat. Fiecare procedură are un titlu clar și începe cu o descriere de o propoziție: "Această procedură se aplică atunci când trebuie să scrii un articol de blog în stilul casei". Claude, când îți citește promptul, se uită la titlurile și descrierile din manual. Când vede match, deschide procedura respectivă, citește detaliile, le aplică. Când nu e match, continuă normal.

Două consecințe ale modelului.

**Descrierea e cheia.** Toate skills-urile tale stau acolo, dar Claude nu le deschide pe toate. Se uită doar la descrieri. Dacă descrierea e vagă ("util pentru articole"), Claude nu știe când să o aleagă. Dacă descrierea e specifică ("aplică această procedură când userul cere un articol de blog în română, pentru solopreneuri, pe teme de AI sau automatizare"), atunci încarcă doar când contextul se potrivește. Bună descriere = skill invocat corect. Descriere slabă = skill ignorat sau invocat greșit.

**Conținutul e doar citit, nu rescris.** Când skill-ul se încarcă, conținutul lui devine parte din instrucțiunile lui Claude pentru turul respectiv. Nu schimbă cine e Claude, adaugă doar "pentru task-ul ăsta, urmează pașii ăștia". Skill-ul acționează ca o lentilă temporară.

---

## Cum folosești

### Structura unui skill

Un skill e un dosar. În dosarul ăla, fișierul principal e `SKILL.md`. Calea tipică:

```
~/.claude/skills/
  └── scriere-articole/
      └── SKILL.md
```

`SKILL.md` are două părți: YAML frontmatter (între `---`) și corpul în Markdown.

Exemplu concret, `~/.claude/skills/scriere-articole/SKILL.md`:

```markdown
---
name: scriere-articole
description: Scrie articole de blog în stilul Dănuț Mitrut pentru solopreneuri români. Se aplică atunci când utilizatorul cere redactarea sau rafinarea unui articol pe teme de AI, automatizare sau productivitate. Nu se aplică pentru texte scurte (social media, emailuri).
---

# Scriere articole

## Structura standard

1. **Hook narativ** în primul paragraf: o situație concretă cu care cititorul se identifică imediat
2. **Problema** articulată în 2-3 paragrafe, fără bullet-uri
3. **Soluția** explicată cu analogie dacă ajută, altfel tehnic dar accesibil
4. **Exemplu concret** din viața solopreneurului
5. **Call to action** scurt, conversațional

## Reguli de ton

- Română cu diacritice complete
- Fără em dash (folosește virgulă sau paranteză)
- Fără antiteze de tip "nu e X, e Y"
- Paragrafe de 2-4 fraze maximum
- Titluri declarative, fără clickbait

## Ce evităm

- Tricolonuri ritmate ("citește, gândește, aplică")
- Paralelisme decorative
- Postambule de politețe ("sper că te-am ajutat")
```

Să desfacem ce e important.

**`name:`** numele skill-ului, fără spații. Devine un slash command (`/scriere-articole`) dacă vrei să îl apelezi manual.

**`description:`** cel mai important câmp. Scris la persoana a treia (nu "eu scriu articole", ci "scrie articole"). Conține două lucruri: ce face skill-ul și când se aplică. Maximum 1024 caractere. Fără tag-uri XML în descriere.

**Corpul în Markdown** e metodologia efectivă. Structurează-l cum îți place, dar fii clar. Claude va citi și aplica.

### Descrierea, detaliat

Descrierea e ce face sau rupe skill-ul. Claude vede doar descrierile (nu conținutul) când alege ce să încarce. Dacă descrierea e proastă, skill-ul e invizibil.

Reguli de descriere bună:

**Persoana a treia.** Claude injectează descrierea în system prompt. "Scriu articole" la persoana întâi creează confuzie de voce. "Scrie articole" e clar.

**Ce + când.** Descrierea trebuie să conțină ambele. "Scrie articole de blog în stilul Dănuț" e ce. "Se aplică când utilizatorul cere un articol de blog pe teme de AI sau automatizare" e când.

**Fii specific peste generic.** "Util pentru scriere" e inutil. "Scrie articole de blog cu structură narativă pentru solopreneuri români, pe teme de AI" e util. Specificitatea e ce face diferența între skill invocat corect și skill ignorat.

### Locațiile: personal sau proiect

Două opțiuni principale.

**`~/.claude/skills/<nume>/SKILL.md`** e skill personal, peste toate proiectele tale. Aici pui metodologia ta generală: stil de scriere, structuri pentru texte, cadre de analiză, reguli de review. Disponibile peste tot, mereu.

**`.claude/skills/<nume>/SKILL.md`** e skill la proiect. Aici pui metodologii specifice proiectului respectiv. De exemplu, un proiect cu un client care are cerințe foarte particulare de ton și structură poate avea un skill dedicat ("scrie în stilul clientului X", aplicat doar în proiectul ăla).

Dacă un skill cu același nume există la ambele niveluri, cel personal câștigă (enterprise > personal > proiect e ordinea generală de prioritate).

### Live reload

Un detaliu practic util. Claude Code urmărește dosarul skills-urilor. Dacă modifici `SKILL.md` în timp ce sesiunea e activă, modificarea intră în vigoare fără restart. Iterezi rapid, testezi, rafinezi. Nu trebuie să închizi și să deschizi panoul.

### Invocare automată sau manuală

Două moduri de a folosi un skill.

**Automat.** Tu scrii promptul tău normal. Claude citește descrierile, vede match, încarcă skill-ul, aplică.

> **Prompt:**
> "Scrie-mi un articol despre cum un solopreneur folosește automatizarea de emailuri."
>
> **Ce ar trebui să vezi (dacă ai skill-ul `scriere-articole`):**
> Claude recunoaște că e un articol pe temă AI/automatizare pentru solopreneur. Încarcă skill-ul. Articolul urmează structura ta (hook narativ, problemă, soluție, exemplu concret, CTA) și regulile de ton.

**Manual.** Dacă vrei să forțezi un skill, îl apelezi ca slash command.

> **Prompt:**
> `/scriere-articole` urmat de "despre automatizarea de emailuri pentru solopreneuri"
>
> **Ce se întâmplă:**
> Skill-ul e încărcat sigur, indiferent de match-ul descrierii. Util când testezi un skill nou, sau când contextul e ambiguu și vrei garanție.

### Fișiere auxiliare în folderul skill-ului

Skills pot avea mai mult decât `SKILL.md`. În dosarul `scriere-articole/`, poți pune fișiere auxiliare:

```
~/.claude/skills/scriere-articole/
  ├── SKILL.md
  ├── exemple/
  │   ├── articol-ai-solopreneur.md
  │   └── articol-automatizare.md
  └── template.md
```

În `SKILL.md`, referi aceste fișiere:

```markdown
## Exemple de referință

Pentru claritate de stil, citește:
- exemple/articol-ai-solopreneur.md
- exemple/articol-automatizare.md

Folosește template.md ca punct de plecare pentru structură.
```

Claude, când încarcă skill-ul, va citi și fișierele referite (progressive disclosure: le încarcă doar dacă ai indicat explicit că sunt parte din skill). Util pentru exemple concrete pe care Claude să le folosească ca reper de stil.

---

## Trei skills utile pentru un solopreneur

### Skill de stil personal de scriere

Cazul cel mai evident. Capturezi stilul tău de scriere într-un skill. Structura standard, regulile de ton, ce eviți. Fiecare text pe care îl scrii Claude, de acum înainte, respectă skill-ul. Elimini să mai explici "scrie fără em dash", "nu folosi tricolonuri", "diacritice complete". Sunt în skill.

### Skill de review pentru feedback de client

Scenariu. Un client îți trimite feedback pe o livrabilă. Prima ta reacție ar fi emoțională. Ai o metodă proprie de a-l procesa: extragi punctele concrete, separi "e preferință" de "e problemă reală", formulezi un răspuns care confirmă ce e valabil și pledează ce susții. Un skill numit `procesare-feedback-client` capturează metoda. De fiecare dată când ai un feedback nou, îl dai lui Claude cu skill-ul aplicat, ieși cu un răspuns structurat care respectă metoda ta de procesare.

### Skill de brief pentru proiect nou

Când începi un proiect (articol, curs, produs, ofertă), ai un set de întrebări pe care ți le pui ca să definești scope-ul. Un skill numit `brief-proiect-nou` conține întrebările și structura brief-ului final. Îi dai lui Claude ideea, el te întreabă pe rând conform skill-ului, iese un brief gata formatat. Standardizezi start-ul de proiect fără să repeți munca de fiecare dată.

---

## Referință

### Structura de bază

```markdown
---
name: nume-skill
description: Descriere la persoana a treia, ce face + când se aplică, maximum 1024 caractere.
---

# Titlu

Instrucțiuni în Markdown pentru Claude.
```

### Locații

| Locație | Scope | Când folosești |
|---|---|---|
| `~/.claude/skills/<nume>/SKILL.md` | Personal, toate proiectele | Metodologie generală a ta (scriere, review, analiză) |
| `.claude/skills/<nume>/SKILL.md` | Proiect curent | Metodologii specifice unui client sau proiect particular |

### Câmpurile YAML frontmatter

| Câmp | Obligatoriu | Ce face |
|---|---|---|
| `name` | Da | Numele skill-ului, devine și slash command pentru invocare manuală |
| `description` | Da | Criteriul pe baza căruia Claude decide când să-l încarce. Persoana a treia. Maxim 1024 caractere |

### Invocare

| Metodă | Cum |
|---|---|
| Automată | Claude decide pe baza descrierii, nu faci nimic |
| Manuală | `/nume-skill` ca slash command, urmat de contextul tău |

---

## Capcane frecvente

Capcane acoperite: descriere vagă care nu încarcă skill-ul, skill scris la persoana întâi, suprapunere cu CLAUDE.md, skills prea multe care încurcă discovery-ul, conținut prea lung sau prea scurt, confuzie între skill și subagent.

### Descriere vagă, skill niciodată încărcat

**Simptom:** Ai scris un skill util, ești sigur că s-ar aplica la task-urile tale, dar Claude nu îl invocă niciodată. Task-urile curg ca și cum skill-ul nu ar exista.
**Cauză:** Descrierea nu conține suficient context ca Claude să match-uiască. "Util pentru scriere" nu îi spune nimic lui Claude despre când să aleagă.
**Fix:** Rescrie descrierea cu "ce face + când se aplică". Include cuvinte cheie care ar apărea probabil în promptul tău. De exemplu, pentru skill-ul de scriere articole, include termeni ca "articol de blog", "text lung", "post", "scriu despre", "stil casă".

### Descriere la persoana întâi

**Simptom:** Skill-ul e încărcat, dar Claude răspunde confuz, cu voce schimbată, uneori ca și cum ar fi user-ul, alteori ca asistent.
**Cauză:** Descrierea e scrisă la persoana întâi ("eu fac asta", "eu aplic metoda"). Descrierea e injectată în system prompt, iar confuzia de persoană creează confuzie de voce.
**Fix:** Rescrie la persoana a treia. "Scrie articole" nu "eu scriu articole". "Aplică metodologia X" nu "aplic metodologia X".

### Suprapunere cu CLAUDE.md

**Simptom:** Ai pus aceleași reguli în CLAUDE.md și în skill. Nu știi când contează una și când cealaltă, iar comportamentul lui Claude e inconsistent.
**Cauză:** CLAUDE.md e memorie permanentă pe proiect (Claude o respectă tot timpul). Skill-ul se încarcă condițional (doar când match-ul e bun).
**Fix:** Regula de separare. În CLAUDE.md pui lucruri care se aplică la orice interacțiune pe proiect (stack, reguli generale, convenții). În skill pui metodologii specifice pentru task-uri particulare (cum scrii un articol, cum faci un review, cum procesezi un brief). Dacă o regulă se aplică la orice, e CLAUDE.md. Dacă se aplică doar când faci X, e skill.

### Prea multe skills activate, discovery-ul suferă

**Simptom:** Ai construit 15 skills, unele se suprapun parțial ca descriere. Claude începe să aleagă skill-ul greșit sau niciunul.
**Cauză:** Descrierile tale se canibalizează. Două skills cu descrieri similare intră în competiție.
**Fix:** Periodic, fă un audit. Vezi dacă două skills pot fi consolidate. Fă descrierile mai exclusiviste (adaugă în fiecare ce anume le distinge de celelalte). Un skill bun poate acoperi 3-4 task-uri înrudite. Nu ai nevoie de un skill per task.

### Conținut prea scurt sau prea lung

**Simptom:** Skill-ul fie e încărcat și nu schimbă mai nimic în output (prea scurt, nu dă instrucțiuni clare), fie îngreunează vizibil fluxul și răspunsurile (prea lung, Claude e copleșit de instrucțiuni).
**Cauză:** Lipsă de calibrare. Skill-ul de 5 linii nu are ce să aplice. Skill-ul de 500 de linii e memorie adiacentă, nu metodologie aplicabilă.
**Fix:** Calibrare. Pentru cele mai multe metodologii, zona bună e 50-200 linii de instrucțiuni clare. Include reguli concrete, exemple, ce eviți. Dacă ai nevoie de mai mult, folosește fișiere auxiliare în dosarul skill-ului și referă-le în `SKILL.md`.

### Confuzie între skill și subagent

**Simptom:** Nu știi când creezi skill și când creezi subagent. Ai făcut pe rând câte unul pentru aceeași situație.
**Cauză:** Ambele captează metodologie, dar fac lucruri diferite. Skill-ul modifică comportamentul lui Claude principal în conversația ta. Subagentul rulează în conversație separată cu context izolat.
**Fix:** Dacă vrei ca Claude principal să aplice o metodologie în conversația actuală (să scrie el cu stilul tău, să facă el review-ul pe loc), folosești skill. Dacă vrei să delegi munca unei instanțe separate care să-ți întoarcă doar rezultatul (păstrând contextul tău curat), folosești subagent. În unele cazuri, le combini: un subagent poate avea instrucțiuni în system prompt care replică ce ar face un skill, dacă vrei delegare cu metodologie.

---

## Următorul pas

→ [3.2 Plugins](plugins.md). Ai construit skills pentru tine. Pasul următor e să le împachetezi împreună, eventual alături de subagenți și comenzi custom, într-un plugin distribuibil. Ceva ce alt solopreneur poate instala cu un click și primi toată metodologia ta gata de folosit. Tu îți transformi workflow-ul în produs digital.

---

## Surse

- [Extend Claude with skills](https://docs.claude.com/en/docs/claude-code/skills)
- [Agent Skills overview](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
- [Skill authoring best practices](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
