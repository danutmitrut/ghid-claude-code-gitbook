*Ghid Claude Code > Anexe > Troubleshooting*

# Troubleshooting

**Probleme care traversează mai multe straturi. Simptom, cauză, fix, strat de referință pentru context mai larg.**

---

## Cum folosești anexa asta

Dacă ceva nu merge, caută simptomul în tabele. Găsești cauza probabilă și fix-ul. Pentru context complet, te trimit la stratul relevant.

Pentru probleme care apar doar într-un strat specific, vezi capcanele din stratul respectiv. Aici sunt doar cele care traversează sau care sunt cel mai des întâlnite la start.

---

## Install și setup

### Extensia nu apare după install în VS Code

**Simptom:** Ai instalat extensia Claude Code din marketplace-ul VS Code, dar nu găsești panoul sau iconița.
**Cauză:** VS Code nu s-a relansat complet după instalare, sau extensia e dezactivată implicit.
**Fix:** Restartează VS Code complet (`Cmd/Ctrl+Shift+P` → "Developer: Reload Window"). Verifică în panoul Extensions că Claude Code e marcată activă. Caută iconița cu scânteia în sidebar-ul din stânga. Dacă tot lipsește, reinstalare cu dezinstalare completă prealabilă. (Referință: 1.1)

### Signin eșuează la primul start

**Simptom:** La prima deschidere, Claude îți cere signin, dar fluxul nu se completează sau întoarce eroare.
**Cauză:** Conexiune de rețea blocată, firewall corporate, pop-up blocker activ în browser, sau credențiale Anthropic invalide.
**Fix:** Verifică internet-ul. Dezactivează VPN temporar. Deschide manual link-ul din prompt dacă browserul nu se deschide automat. Pentru firewall corporate, contactează IT-ul pentru a albi domeniile Anthropic. (Referință: 1.1)

### Panoul dispare sau nu se deschide

**Simptom:** Ai avut panoul Claude activ, acum nu mai apare. Sau iconița există, dar click-ul nu deschide nimic.
**Cauză:** Stare coruptă a extensiei sau a layout-ului VS Code.
**Fix:** `Cmd/Ctrl+Shift+P` → "View: Reset View Locations". Restart VS Code. Dacă nu merge, `Cmd/Ctrl+Shift+P` → "Developer: Reload Window" forțat. Ca ultimă rezolvare, dezinstalezi și reinstalezi extensia. (Referință: 1.1)

---

## Interacțiune de bază

### Tool call blocat în așteptarea permisiunii

**Simptom:** Claude a propus un tool call (ex. Bash, Edit), panoul arată butonul de aprobare, dar nu se întâmplă nimic când dai click.
**Cauză:** Fereastră popup acoperită sau focus pierdut. Uneori, click-ul nu ajunge la panou.
**Fix:** Click în altă zonă a VS Code, apoi reia click pe butonul de aprobare. Verifică modul de permisiuni (`Shift+Tab` pentru a cicla). Dacă ești în Plan Mode, niciun tool nu se aplică până nu ieși din el. (Referință: 1.3)

### Claude citește multe fișiere și contextul se umple rapid

**Simptom:** O conversație care a început simplu devine lentă, răspunsurile vin întârziate, Claude pare confuz pe lucruri de bază.
**Cauză:** Contextul e aproape de limită. Prea multe fișiere citite, prea multe tool call-uri lungi.
**Fix:** Rulează `/compact` pentru a comprima conversația păstrând esența. Dacă task-ul permite, `/clear` pentru restart complet cu CLAUDE.md-ul reîncărcat. Pentru task-uri cu explorare masivă, consideră un subagent (2.1) ca să păstrezi contextul principal curat. (Referință: 1.4, 2.1)

### `@`-mention-ul nu găsește fișierul

**Simptom:** Scrii `@nume-fisier` dar nu apare în lista de sugestii.
**Cauză:** Fișierul nu e în workspace-ul curent, sau numele e scris greșit.
**Fix:** Verifică că ai deschis proiectul corect în VS Code (fișierul așteptat să apară în file explorer din stânga). Folosește calea parțială (`@src/utils/` pentru a vedea ce e în dosar). Dacă fișierul e într-un dosar exclus prin `.gitignore` sau `.claudeignore`, nu apare implicit. (Referință: 1.2)

---

## Memorie și comenzi

### CLAUDE.md pare ignorat

**Simptom:** Ai instrucțiuni clare în `CLAUDE.md`, dar Claude le ignoră sau nu le aplică consistent.
**Cauză:** Fișierul e în locul greșit, e prea lung, sau conține instrucțiuni contradictorii.
**Fix:** Verifică locația. `CLAUDE.md` în rădăcina proiectului e încărcat automat. `.claude/CLAUDE.md` de asemenea. Alte locații nu sunt încărcate automat. Verifică lungimea (sub 100 linii ideal). Caută contradictii între instrucțiuni. După modificări, rulează `/compact` pentru a forța reîncărcarea root-ului. (Referință: 1.4)

### Slash command custom nu apare în meniu

**Simptom:** Ai creat `.claude/commands/nume.md`, dar tastând `/` nu apare în listă.
**Cauză:** Numele fișierului nu respectă convenția (doar litere, cifre, liniuțe), sau formatul fișierului e greșit.
**Fix:** Numele fișierului (fără extensie) devine slash command-ul. `follow-up.md` → `/follow-up`. Evită spații și caractere speciale. Verifică conținutul fișierului (Markdown simplu e suficient). Restart panou sau schimbă focus pe alt fișier și înapoi pentru reîncărcare. (Referință: 1.4)

### `/compact` șterge context important

**Simptom:** După `/compact`, Claude pare să fi uitat detalii pe care le discutaserăți.
**Cauză:** Compact-ul compresează conversația, dar nu garantează păstrarea fiecărui detaliu. E sinteză, nu arhivă.
**Fix:** Pentru detalii critice pe care nu vrei să le pierzi, mută-le în `CLAUDE.md` (memorie permanentă) sau într-un fișier de referință pe care îl ataș explicit cu `@`. `/compact` e util pentru context mediu, nu pentru informații cruciale care trebuie păstrate. (Referință: 1.4)

---

## Extensii avansate

### Subagent nu răspunde când îl aștepți

**Simptom:** Ai definit un subagent, e salvat în `.claude/agents/`, dar Claude principal nu-l invocă în situații unde credeai că ar face-o.
**Cauză:** Descrierea subagentului e prea vagă.
**Fix:** Rescrie descrierea ca răspuns la întrebarea "când să fie folosit acest subagent?". Include cuvinte cheie probabile în promptul tău. Testează manual invocarea explicită ("folosește subagentul X pentru..."). Dacă merge manual, problema e doar descrierea. (Referință: 2.1)

### MCP server apare "error" la `/mcp`

**Simptom:** Ai configurat un MCP server, rulezi `/mcp`, vezi status "error" sau "failed to start".
**Cauză:** Top 3 în ordine: lipsă Node.js/Python, token API incorect, pachet greșit în `args`.
**Fix:** Verifică mediul (`node --version`, `python --version` în terminal). Verifică token-ul (copiat corect, fără spații). Verifică numele pachetului din documentație. Restart panou după corecție. Verifică și `/mcp` pentru mesajul exact de eroare, care conține adesea clue-uri. (Referință: 2.2)

### Hook configurat dar nu se declanșează

**Simptom:** Ai adăugat un hook în `.claude/settings.json`, dar scriptul nu rulează la evenimentul așteptat.
**Cauză:** Matcher greșit (case-sensitive), hook pus pe eveniment care nu suportă matcher, sau JSON-ul e invalid.
**Fix:** Verifică JSON-ul cu un validator online sau în VS Code (extensiile JSON detectează erori). Verifică matcher-ul (`"Bash"` nu `"bash"`). Pentru evenimente fără matcher (`UserPromptSubmit`, `SessionStart`), scoate complet câmpul `matcher`. Testează scriptul izolat cu `echo '{...}' | script.sh`. (Referință: 2.3)

### Skill nu e auto-loaded

**Simptom:** Ai creat un skill în `~/.claude/skills/<nume>/SKILL.md`, dar Claude nu îl încarcă când contextul pare să corespundă.
**Cauză:** Descriere vagă, scrisă la persoana întâi, sau fără cuvinte cheie relevante.
**Fix:** Verifică descrierea: persoana a treia, conține "ce face + când se aplică", max 1024 caractere. Include cuvinte cheie care ar apărea în promptul tău. Testează cu invocare manuală: `/<nume-skill>` în panou. Dacă merge manual, problema e doar în descriere. (Referință: 3.1)

### Plugin nu se instalează

**Simptom:** Rulezi `/plugin install <cale sau URL>`, primești eroare sau instalarea aparent reușește dar componentele nu apar.
**Cauză:** Top 3: `plugin.json` în locul greșit (trebuie în `.claude-plugin/`), structura dosarelor greșită (nu se pun sub `.claude-plugin/`), sau nume rezervat folosit.
**Fix:** Verifică structura: `.claude-plugin/plugin.json` și atât în `.claude-plugin/`. `skills/`, `agents/`, `commands/`, `hooks/`, `.mcp.json` la rădăcina plugin-ului. Nu folosi nume care impersonează marketplace-uri oficiale. Rulează `/plugin list` pentru a vedea dacă instalarea a reușit parțial. (Referință: 3.2)

---

## SDK și headless

### `claude -p` întoarce eroare sau răspuns gol

**Simptom:** Rulezi `claude -p "prompt"` în terminal, primești eroare sau output gol.
**Cauză:** Claude Code nu e instalat global, autentificare expirată, sau prompt-ul conține caractere care rup parsing-ul shell.
**Fix:** Verifică `claude --version`. Dacă eroare, reinstalează: `npm install -g @anthropic-ai/claude-code`. Pentru autentificare, rulează `claude` interactiv o dată ca să verifici signin-ul. Pune prompt-ul între ghilimele duble și escape-uie interiorul dacă are caractere speciale. (Referință: 3.3)

### SDK Python primește `ImportError`

**Simptom:** `from claude_agent_sdk import query` dă `ImportError: No module named 'claude_agent_sdk'`.
**Cauză:** Pachetul nu e instalat, sau e instalat într-un virtual environment diferit.
**Fix:** `pip install claude-agent-sdk --break-system-packages`. Dacă folosești `venv`, verifică că rulezi Python-ul din venv-ul unde l-ai instalat. Verifică versiunea Python (3.10+ necesar). (Referință: 3.3)

### Pipeline automat rulează cu costuri mari

**Simptom:** La sfârșitul lunii, factura Anthropic e mult mai mare decât te-ai aștepta.
**Cauză:** Fie volumul apelurilor e mai mare decât ai estimat, fie tokenii per apel sunt mai mulți decât credeai (fișiere mari în context).
**Fix:** Adaugă logging în pipeline care înregistrează numărul de tokeni pentru fiecare apel. Calculează costul așteptat lunar. Setează limite de buget și alerte în dashboard-ul Anthropic. Consideră modele mai mici (Haiku) pentru task-uri simple. Evită să trimiți fișiere mari ca context dacă doar un fragment e relevant. (Referință: 3.3)

---

## Probleme care traversează mai multe straturi

### Comportament inconsistent între sesiuni

**Simptom:** Claude aplică o regulă într-o sesiune, nu o aplică în alta. Par aleatoriu.
**Cauză:** Regula există în locuri diferite cu conflict (CLAUDE.md personal vs proiect, skill vs instrucțiune în prompt), sau doar într-unul (personal) care nu se aplică pe proiectul curent.
**Fix:** Auditul surselor de instrucțiuni. Rulează `/memory` (dacă există în versiunea ta) sau verifică manual `~/.claude/CLAUDE.md`, `CLAUDE.md` din proiect, skills-urile active, custom commands. Rezolvă conflictele prin eliminare sau prin clarificare explicită care sursă câștigă.

### Claude începe cu voce schimbată sau în engleză

**Simptom:** Conversație în română care primește brusc răspunsuri în engleză, sau cu ton diferit de cel așteptat.
**Cauză:** Un skill sau subagent s-a activat și a schimbat contextul. Sau descrierea unui skill e la persoana întâi și a stricat voce.
**Fix:** Verifică dacă s-a invocat un skill sau subagent neașteptat (vizibil în panou). Dacă descrierea e la persoana întâi, rescrie la a treia. Pentru limba de răspuns, include-o explicit în CLAUDE.md de proiect ("Răspunsuri în română cu diacritice. Excepție: termeni tehnici pot rămâne în engleză.").

### Acțiuni pe care nu le-ai cerut

**Simptom:** Claude face edit-uri sau rulează comenzi pe care nu le-ai cerut explicit. Sau rulează mai multe tool call-uri decât te aștepți.
**Cauză:** Permission mode prea permisiv (Auto-Accept sau bypassPermissions), hooks care declanșează acțiuni automate, sau subagenți invocați fără să-i menționezi explicit.
**Fix:** Verifică modul de permisiuni (`Shift+Tab` cycle). Pentru task-uri sensibile, rămâi în Normal sau Plan Mode. Auditul hooks-urilor din `.claude/settings.json` și `~/.claude/settings.json`. Restricționează tool-urile subagenților. (Referință: 1.3, 2.3)

### Plugin instalat dar componentele sale nu funcționează

**Simptom:** Ai instalat un plugin, `/plugin list` îl arată activ, dar skills-urile sau subagenții din el nu par să se aplice.
**Cauză:** Structura plugin-ului e parțial ruptă, sau componentele depind de prerequisite nedeclarate (Node.js, Python, credențiale).
**Fix:** Citește README-ul plugin-ului pentru prerequisite. Verifică structura dosarelor. Rulează `/mcp` dacă plugin-ul aduce MCP servers. Contactează autorul plugin-ului dacă lista de prerequisite e neclară. (Referință: 3.2)

---

## Când nimic din ce e mai sus nu funcționează

### Restart inteligent

În ordine, de la cel mai ușor la cel mai greu:

1. Close & reopen panoul Claude în VS Code
2. `/compact` pentru context plin, `/clear` pentru restart conversațional
3. Reload window VS Code (`Cmd/Ctrl+Shift+P` → "Developer: Reload Window")
4. Restart VS Code complet
5. Restart calculator
6. Dezinstalare și reinstalare extensie Claude Code

### Căutare eroare specifică

Copiază exact mesajul de eroare și caută:
- Pe [docs.claude.com](https://docs.claude.com)
- În [release notes Claude Code](https://docs.claude.com/en/release-notes/claude-code) (poate e un bug cunoscut)
- Pe GitHub în [Claude Code issues](https://github.com/anthropics/claude-code/issues)

### Raportare

Dacă ai identificat un bug real, raportează-l pe GitHub cu: versiunea extensiei, versiunea VS Code, OS, pași de reproducere, output-ul erorii. Pentru ajutor rapid, Anthropic Support e disponibil.

---

*Ultima revizuire: 2026-04-17.*
