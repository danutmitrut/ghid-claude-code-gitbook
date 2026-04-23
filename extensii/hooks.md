*Partea II, Extensii > 2.3 Hooks*

# 2.3 Hooks

**Cum automatizezi verificări și acțiuni care se întâmplă singure, la momente fixe (înainte de fiecare edit, după un tool call, la începutul sesiunii), prin scripturi mici rulate în fundal.**

| Meta | |
|---|---|
| Timp de setup | 1-2 ore pentru primul hook funcțional |
| Timp de învățare până la fluență | 2-4 săptămâni |
| Valoare pentru solopreneur | Medie, util când identifici repetiție clară care merită automatizată |
| Dependențe | 1.1 până la 1.5 complete, un pic de familiaritate cu terminalul (explicat mai jos) |

**Acoperă:** ce e un hook și care e diferența față de skills/subagenți, evenimentele principale (`PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `SessionStart`, `Stop`), configurare în `.claude/settings.json` cu matcher pentru tool-uri, comunicare prin exit codes (0 = continuă, 2 = blochează), trei exemple concrete cu utilitate reală pentru solopreneur.
**Nu acoperă:** distribuție de hooks ca parte dintr-un plugin sau skill (vezi 3.1, 3.2), integrare în SDK headless (vezi 3.3), HTTP hooks și hooks complexe care depășesc nivelul solopreneurului mediu.

---

## TL;DR

Un hook e un script scurt care se rulează automat în fundal la un moment anume din conversație. De exemplu, înainte ca Claude să facă o comandă Bash, hook-ul poate verifica dacă comanda e sigură. După ce editează un fișier, hook-ul poate formata codul. La începutul sesiunii, hook-ul poate încărca variabile de mediu. Tu îl definești o dată într-un fișier JSON, Claude îl rulează singur de atunci înainte. Este stratul cel mai tehnic din Partea II, dar poate fi sărit dacă nu ai nevoie încă.

---

## De ce contează

Ai observat probabil că anumite lucruri se repetă de fiecare dată când lucrezi cu Claude. După fiecare edit, vrei să rulezi linter-ul. Înainte de fiecare commit, vrei să verifici că testele trec. La începutul sesiunii, vrei să-i reamintești lui Claude statusul proiectului. Unele dintre ele le poți pune în CLAUDE.md (memorie), dar asta te bazează pe faptul că Claude își amintește. Nu garantează execuție.

Hooks-urile garantează execuție. Sunt reflexele sistemului. Când se întâmplă evenimentul X, scriptul Y rulează, punct. Claude nu poate "uita" să-l ruleze, nu poate sări peste, nu depinde de atenția lui la CLAUDE.md.

Pentru solopreneur, asta se traduce în două beneficii concrete. Primul, auto-verificări pe care nu vrei să le negociezi (formatare cod, linting, teste, backup). Al doilea, injectare automată de context (la fiecare prompt, hook-ul adaugă informație despre proiect, ora, statusul Git).

Avertisment onest: dacă folosirea terminalului te intimidează complet, sari peste acest strat pentru moment. Valoarea hooks-urilor se deblochează după ce ești confortabil cu lucrurile de bază ale shell-ului (rulare comandă, scris un script mic în bash sau Python). Restul ghidului funcționează și fără hooks.

---

## Ce vei putea face după

- Înțelegi diferența între hook, skill, subagent și memorie
- Adaugi un hook în `.claude/settings.json` pentru un eveniment simplu
- Folosești matcher ca să rulezi hook-ul doar pe anumite tool-uri (ex. doar pe `Bash`, doar pe `Edit`)
- Blochezi o acțiune periculoasă cu un hook pe `PreToolUse`
- Injectezi context automat în conversație cu `UserPromptSubmit`
- Știi când un reflex merită hook și când rămâne în CLAUDE.md

---

## Modelul mental: reflex vs. instrucțiune

CLAUDE.md e ca o notă pe peretele biroului. "Când scrii funcții, folosește stilul X." Claude o citește, încearcă să o urmeze, dar o poate uita în mijlocul unui task complicat.

Hook-ul e ca un reflex. Nu mai e instrucțiune către Claude, e acțiune care se întâmplă independent. Când se întâmplă evenimentul, scriptul rulează fără ca Claude să decidă.

Diferența practică: scrie în CLAUDE.md "formatează codul cu Prettier" și Claude va încerca. Definește un hook pe `PostToolUse` pentru `Edit` care rulează Prettier și formatarea se întâmplă de fiecare dată, fără ca Claude să fie implicat în decizie.

Consecințele modelului:

**Hook-urile nu negociază.** Dacă exit code-ul e 2 (blochează), acțiunea e blocată. Claude primește mesajul de eroare și ajustează. Util pentru guardrails stricte (de exemplu, nu rula comenzi care șterg fișiere în `/`).

**Hook-urile trebuie să fie rapide.** Rulează sincron, blochează fluxul cât sunt active. Un hook care ia 10 secunde îngheață conversația timp de 10 secunde. Păstrează-le scurte (sub secundă ideal).

**Hook-urile sunt puternice și periculoase.** Rulează comenzi shell arbitrare pe mașina ta, cu drepturile tale. Un hook scris greșit poate șterge fișiere, modifica config, face request-uri nedorite. Tratează-le ca pe cod care rulează non-stop în fundalul calculatorului tău, pentru că asta sunt.

---

## Cum folosești

### Configurare de bază

Hook-urile se definesc în fișierul `.claude/settings.json` din proiect (sau `~/.claude/settings.json` pentru user global). Dacă fișierul nu există, îl creezi.

Structura generală:

```json
{
  "hooks": {
    "NumeEveniment": [
      {
        "matcher": "NumeTool",
        "hooks": [
          {
            "type": "command",
            "command": "comanda-de-rulat"
          }
        ]
      }
    ]
  }
}
```

Trei nivele:
1. **Evenimentul** (`PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `SessionStart`, `Stop`)
2. **Matcher-ul** (ce tool declanșează hook-ul, cu `*` pentru "orice tool")
3. **Comanda** (shell command care rulează efectiv)

### Primul hook concret: log pentru comenzi Bash

Scenariu: vrei să știi tot ce rulează Claude în shell, pentru un log pe care îl poți revedea dacă ceva merge prost.

Adaugi în `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '\"\\(.tool_input.command) - \\(now | todate)\"' >> ~/.claude/bash-log.txt"
          }
        ]
      }
    ]
  }
}
```

Ce se întâmplă. Înainte de fiecare tool call `Bash`, Claude trimite la scriptul tău un obiect JSON care conține comanda pe care urmează să o ruleze. Scriptul tău (aici, un lanț de comenzi cu `jq`) extrage comanda și data curentă, le adaugă în `~/.claude/bash-log.txt`. Exit code 0 (implicit), Claude continuă normal.

> **Prerequisit minim:** `jq` e un utilitar pentru procesare JSON. Pe Mac, îl instalezi cu `brew install jq` din terminal. Pe Windows/Linux, vezi [jqlang.github.io](https://jqlang.github.io). Dacă nu ai `jq`, poți folosi alte utilitare similare, dar jq e cel mai simplu.

> **Prompt de verificare:**
> "Rulează `ls` în terminal."
>
> **Ce ar trebui să vezi:**
> Claude rulează comanda. În `~/.claude/bash-log.txt` apare o linie nouă cu `ls - 2026-04-17T14:23:00Z`.

### Al doilea hook: blocare comenzi periculoase

Scenariu: vrei siguranță suplimentară că Claude nu rulează `rm -rf` pe directoare importante, chiar dacă tu îl ceri din greșeală sau el interpretează greșit.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -e 'select(.tool_input.command | test(\"rm -rf /\")) | halt_error(2)'"
          }
        ]
      }
    ]
  }
}
```

Ce se întâmplă. Scriptul verifică dacă comanda conține pattern-ul `rm -rf /` (sau variante asemănătoare). Dacă da, `halt_error(2)` termină cu exit code 2. Claude Code interpretează exit code 2 ca "blochează acțiunea" și îi arată lui Claude mesajul de eroare. Claude încearcă o altă cale.

În practică, vrei un pattern mai elaborat (verifici căi specifice, combinații periculoase). Ăsta e scheletul conceptual.

### Al treilea hook: injectare de context la fiecare prompt

Scenariu: la fiecare prompt nou al tău, vrei ca Claude să primească automat statusul Git-ului curent. Așa știe dacă sunt fișiere nesalvate, pe ce branch ești, dacă sunt commit-uri neîmpinse.

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo \"## Git status:\\n$(git status --short)\\n## Current branch:\\n$(git branch --show-current)\""
          }
        ]
      }
    ]
  }
}
```

Ce se întâmplă. La fiecare prompt pe care îl scrii, hook-ul rulează. Output-ul lui stdout e adăugat automat în contextul lui Claude înainte ca el să vadă promptul tău. Practic, Claude primește întotdeauna "iată statusul Git + promptul userului".

Util pentru solopreneuri care lucrează cu Git (chiar și ca backup simplu). Claude are întotdeauna context pe starea proiectului, nu trebuie să-i spui tu.

> ⚠️ `UserPromptSubmit` nu are matcher. Se aplică la fiecare prompt. Asta e atât beneficiu (nu trebuie configurat) cât și cost (rulează mereu, deci păstrează comenzile rapide).

### Evenimentele pe care le vei folosi

Claude Code are mai multe evenimente. Pentru solopreneur, patru sunt suficiente pentru majoritatea cazurilor.

| Eveniment | Când se declanșează | Caz de utilizare tipic |
|---|---|---|
| `PreToolUse` | Înainte de fiecare tool call | Validare sau blocare de acțiuni |
| `PostToolUse` | După ce un tool a rulat cu succes | Formatare cod după `Edit`, log după `Bash` |
| `UserPromptSubmit` | Când trimiți un prompt | Injectare context (Git status, timp, task curent) |
| `Stop` | Când Claude termină turul | Notificare (beep, sunet, mesaj) că e gata |

Există mai multe (`SessionStart`, `SessionEnd`, `TaskCompleted`, etc.), dar patru de mai sus rezolvă 80% din cazuri.

---

## Referință

### Structura `.claude/settings.json` pentru hooks

```json
{
  "hooks": {
    "Event": [
      {
        "matcher": "ToolName",
        "hooks": [
          { "type": "command", "command": "..." }
        ]
      }
    ]
  }
}
```

### Matcher-ul

| Valoare | Ce face |
|---|---|
| `"Bash"` | Se potrivește exact pe tool-ul `Bash` |
| `"Edit"` | Se potrivește pe `Edit` |
| `"*"` | Se potrivește pe orice tool |
| (fără matcher) | Obligatoriu pentru evenimente fără matcher (`UserPromptSubmit`, `SessionStart`, `Stop`, altele) |

### Comunicare cu Claude prin exit code

| Exit code | Rezultat |
|---|---|
| 0 | Acțiunea continuă. Stdout-ul e adăugat în contextul lui Claude pentru `UserPromptSubmit` și `SessionStart` |
| 2 | Blochează acțiunea. Stderr-ul ajunge la Claude ca feedback |
| Altele | Continuă, dar cu avertisment |

### Input primit de hook pe stdin

JSON cu câmpuri principale:
- `session_id`: identificator sesiune
- `cwd`: directorul curent
- `hook_event_name`: numele evenimentului (ex. `PreToolUse`)
- `tool_name`: tool-ul care se apelează (pentru evenimente cu matcher)
- `tool_input`: argumentele tool-ului (pentru `PreToolUse` și `PostToolUse`)
- `tool_response`: rezultatul tool-ului (doar pentru `PostToolUse`)

---

## Capcane frecvente

Capcane acoperite: hook lent care îngheață panoul, hook care șterge din greșeală, confuzie cu memoria sau skills, matcher greșit care nu declanșează niciodată, log-uri care cresc infinit, scripturi fără testare prealabilă.

### Hook lent care îngheață panoul

**Simptom:** Panoul Claude îngheață câteva secunde la fiecare acțiune. Răspunsurile vin cu întârziere.
**Cauză:** Ai pus într-un hook o comandă care durează (rulare teste, request HTTP lent, compilare). Hook-urile rulează sincron, deci fiecare tool call așteaptă hook-ul.
**Fix:** Regula e simplă. Sub o secundă per hook. Dacă ai nevoie de ceva mai lent (teste, lint complet), fă-l asincron (rulează în background cu `&` la finalul comenzii, sau folosește `nohup`). Sau mută task-ul la un script separat pe care îl rulezi manual.

### Hook care șterge sau modifică fișiere din greșeală

**Simptom:** Te-ai trezit cu un config șters, un fișier de lucru rescris, o bază de date modificată. Hook-ul a rulat ceva ce nu intenționai.
**Cauză:** Hook-urile rulează cu privilegiile tale complete. O greșeală de script (ex. path greșit, quoting prost) poate face acțiuni distrugătoare.
**Fix:** Trei reguli. 1) Testează scriptul hook-ului separat, rulat manual cu input de test, înainte să-l pui live. 2) Evită acțiuni distrugătoare în hooks (nu `rm`, nu `mv`, nu scrieri destructive). Hooks-urile sunt pentru read, log, notify, format. Pentru modificări riscante, lasă-le explicite. 3) Păstrează backup la `settings.json` când faci schimbări mari.

### Confuzie cu memoria (CLAUDE.md) sau skills

**Simptom:** Nu știi dacă un comportament vrei să-l pui ca hook, skill, sau instrucțiune în CLAUDE.md.
**Cauză:** Suprapunere parțială între cele trei mecanisme.
**Fix:** Model de decizie. Dacă vrei ca Claude să-și amintească (poate uita), CLAUDE.md. Dacă vrei o metodologie reutilizabilă pe care Claude o aplică conștient, skill (vezi 3.1). Dacă vrei un reflex garantat, care se întâmplă independent de voința lui Claude, hook. Hook-urile sunt ultima rampă, când ceva chiar trebuie să se întâmple indiferent ce.

### Matcher greșit, hook niciodată declanșat

**Simptom:** Ai scris un hook, ai salvat, dar nu se întâmplă nimic la execuția tool-urilor.
**Cauză:** De obicei matcher. `"bash"` cu b mic nu e același cu `"Bash"`. Sau ai pus matcher la un eveniment care nu îl suportă (ex. `UserPromptSubmit`).
**Fix:** Recheck matcher-ul. Case-sensitive. Pentru evenimente fără matcher, nici nu pune câmpul `matcher` (nu `""`, nu `"*"`, scoate-l complet). Verifică rulând un prompt simplu și văzând dacă hook-ul apare în log-urile de diagnostic ale extensiei.

### Log-uri care cresc infinit

**Simptom:** Ai un hook de log (ca primul exemplu) care scrie în fișier. După câteva săptămâni, fișierul are 100MB.
**Cauză:** Log-urile nu se curăță singure.
**Fix:** Fie rotirea log-ului (scrie într-un fișier cu data, ex. `~/.claude/bash-log-$(date +%Y-%m-%d).txt`). Fie curățare periodică manuală. Fie folosește `tail` la scriere (ex. păstrează doar ultimele 1000 de linii). Nu lăsa log-urile să crească nelimitat.

### Hook pus live fără testare prealabilă

**Simptom:** Ai copiat un hook dintr-un tutorial, l-ai pus direct în `settings.json`, a blocat conversația sau a rupt ceva.
**Cauză:** Scriptul făcea o presupunere care nu se potrivea cu mediul tău (path greșit, binar lipsă, permisiune insuficientă).
**Fix:** Înainte să pui orice hook în `settings.json`, rulează scriptul izolat. Pentru testare, creează un JSON mic de simulare (copiezi manual un exemplu din docs), îl dai ca input la script cu `echo '{...}' | script.sh`. Vezi ce returnează. Apoi îl pui live. Pentru cazuri mai complicate, păstrează un `settings.json.test` separat și pornești Claude dintr-un proiect de test înainte să-l pui pe cel real.

---

## Următorul pas

→ [3.1 Skills](../constructie/skills.md). Hooks-urile automatizează reflexe. Skills-urile, în schimb, capturează metodologie. Când ai un mod de lucru propriu (cum structurezi un articol, cum analizezi un feedback de client, cum faci un review), skill-ul e fișierul care învață Claude să-l aplice de fiecare dată. Pasul de la automatizare la transfer de know-how.

---

## Surse

- [Get started with Claude Code hooks](https://docs.claude.com/en/docs/claude-code/hooks-guide)
- [Hooks reference](https://docs.claude.com/en/docs/claude-code/hooks)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
