*Partea III, Construcție > 3.3 SDK și headless*

# 3.3 SDK și headless

**Cum scoți Claude din panoul VS Code și îl pui să lucreze singur, în scripturi care rulează automat, la 3 noaptea sau integrate în produsul tău. Ultima frontieră: Claude ca angajat programatic, nu ca asistent la cerere.**

| Meta | |
|---|---|
| Timp de setup | 4+ ore pentru primul pipeline funcțional |
| Timp de învățare până la fluență | 1-3 luni, în funcție de cazul de producție |
| Valoare pentru solopreneur | Medie-Mare dacă ai un caz de automatizare reală, altfel Low. Se justifică când economisești sistematic ore pe săptămână sau creezi produs care folosește Claude în fundal |
| Dependențe | 1.1 până la 1.5 complete, noțiuni de TypeScript sau Python pentru SDK. Pentru CLI headless, noțiuni de scripting shell |

**Acoperă:** diferența între CLI-ul `claude` headless (`-p` sau `--print`) și Agent SDK (Python și TypeScript), cazuri tipice de folosire pentru solopreneur (pipeline cron, integrare în aplicație proprie, sistem batch de procesare), exemplu Python cu `query()` și `ClaudeAgentOptions`, flag-uri esențiale (`--output-format json`, `--allowedTools`, `--bare`), contextul în care plug-in-urile din 3.2 se aplică și în SDK.
**Nu acoperă:** deployment specific platformă cloud (AWS, GCP, Azure), dezvoltare MCP server propriu, SDK patterns advanced (custom tools, multi-agent orchestration complexă), optimizări de cost la scale.

---

## TL;DR

Până la 3.2, tot ce am construit a trăit în panoul Claude Code din VS Code. Stratul ăsta îl scoate din panou. CLI-ul `claude -p "prompt"` rulează Claude în terminal fără interfață, util pentru scripturi rapide și automatizări simple. Agent SDK (Python sau TypeScript) îl pune în programul tău ca bibliotecă, util pentru aplicații proprii sau pipeline-uri complexe. Pentru solopreneurul mediu, stratul ăsta e opțional. Devine relevant când identifici un caz de automatizare repetitivă (procesare emailuri zilnice, raport săptămânal, curățare date lunar) sau când construiești un produs care integrează Claude intern.

---

## De ce contează

Până aici, dialogul cu Claude a fost sincron. Tu scrii în panou, el răspunde. Tu vezi diff-ul, aprobi. Tu ești în fața calculatorului. Pentru munca zilnică de solopreneur, modelul ăsta e suficient.

Sunt două situații în care devine limitare.

**Task-uri repetitive care nu merită atenția ta.** Procesezi în fiecare dimineață 30 de emailuri (triaj, răspunsuri simple, marcare). În fiecare luni, faci un raport cu ce s-a lucrat săptămâna trecută. În fiecare zi de 1, curăți baza de date de duplicate. Toate astea sunt munca pe care nici ție nu-ți place să o faci, dar nici nu o poți delega la altcineva. Un pipeline automat care rulează noaptea și îți lasă rezultatul pe birou dimineața e răspunsul.

**Produs care folosește Claude intern.** Vinzi o aplicație sau un serviciu care, ca parte din funcționalitate, cere ca Claude să facă o muncă (rezumă text, analizează fișier, generează conținut). Nu poți cere clientului să deschidă VS Code și să interacționeze cu panoul. Ai nevoie ca Claude să fie integrat în aplicația ta, accesibil programatic.

Pentru primul scenariu, CLI-ul `claude -p` e de obicei suficient. Un script bash care cheamă `claude -p` cu prompt-ul tău, parsează rezultatul, face ce trebuie. Ușor de scris, ușor de menținut.

Pentru al doilea scenariu, ai nevoie de SDK. Python sau TypeScript, scris ca orice bibliotecă, integrat în aplicația ta. Mai multă putere, mai multă complexitate.

Avertisment onest. Stratul ăsta are un prag de intrare mai ridicat decât restul ghidului. Cere familiaritate cu terminalul și, pentru SDK, cu un limbaj de programare. Dacă nu ești acolo, sari peste. Capabilitățile din straturile 1-3.2 sunt deja semnificative pentru un solopreneur nontehnic.

---

## Ce vei putea face după

- Înțelegi diferența dintre CLI headless și SDK, și când alegi pe care
- Rulezi Claude dintr-un script shell cu `claude -p "prompt"`
- Extragi rezultatul structurat cu `--output-format json` pentru parsare ulterioară
- Limitezi tool-urile disponibile pentru rulări automate cu `--allowedTools`
- Scrii un pipeline minim în Python cu Agent SDK pentru un task recurent
- Decizi când merită efortul de a muta un workflow în producție

---

## Modelul mental: panoul e cabina pilotului, headless e autopilotul

În panoul VS Code, tu ești pilotul. Decizi fiecare manevră, vezi fiecare ajustare, aprobi fiecare edit. E mod manual cu asistență bună. Necesar când sarcina e creativă, ambiguă sau cere intervenție constantă.

Headless e autopilot. Tu programezi traiectoria înainte, aparatul o execută singur. Ideal când zborul e rutinier, bine definit, nu cere decizii creative pe parcurs. Pentru asta plătești un cost la început (scrierea scriptului) și primești în schimb rulări repetate fără atenția ta.

Consecința modelului pentru solopreneur. Nu muta în headless orice task. Muți doar task-urile cu aceste trei calități: se repetă des (ideal săptămânal sau zilnic), au structură clară (input previzibil, output măsurabil), nu cer decizii subiective pe loc. Task-urile creative rămân în panou. Task-urile rutiniere devin pipeline-uri.

---

## CLI headless: `claude -p`

Cel mai simplu mod de a folosi Claude în afara panoului e CLI-ul, cu flag-ul `-p` (sau `--print`, echivalent). Primește un prompt, întoarce răspunsul, iese. Fără REPL, fără interacțiune.

### Instalare (o dată)

Deschizi terminalul (în VS Code `View > Terminal`, sau aplicația Terminal dedicată). Instalezi Claude Code global cu:

```bash
npm install -g @anthropic-ai/claude-code
```

După instalare, comanda `claude` e disponibilă oriunde. Verifici cu:

```bash
claude --version
```

### Primul script

Un caz concret. Vrei să rezumi zilnic, la 7 dimineața, un fișier cu note pe care le-ai scris ziua anterioară.

Script `rezumat-zilnic.sh`:

```bash
#!/bin/bash

NOTE_FILE="$HOME/notite/$(date -v-1d +%Y-%m-%d).md"
OUTPUT="$HOME/notite/rezumate/$(date +%Y-%m-%d)-rezumat.md"

claude -p "Rezumă notițele din $NOTE_FILE în 5 bullet-uri scurte, în română. Salvează în $OUTPUT." \
  --allowedTools "Read,Write" \
  --bare
```

Ce face. Construiește numele fișierului de ieri, numele fișierului de output pentru azi. Cheamă Claude cu un prompt concret. Îi permite doar Read și Write (nu are voie la Bash, Edit, alte tool-uri). Flag-ul `--bare` îi spune să nu încarce configurul local interactiv (util în scripturi, evită surprize).

Îl programezi să ruleze zilnic la 7 cu `cron` (pe Mac/Linux) sau Task Scheduler (pe Windows). Dimineața găsești rezumatul făcut.

### Flag-urile importante

| Flag | Ce face |
|---|---|
| `-p` sau `--print` | Rulează headless, fără REPL. Intră în script cu prompt, iese cu răspuns |
| `--output-format json` | Răspunsul iese în JSON structurat, ușor de parsat în alt script |
| `--output-format stream-json` | JSON pe linii separate, util pentru streaming în timp real |
| `--allowedTools "Tool1,Tool2"` | Limitezi tool-urile permise, util pentru siguranță în automat |
| `--bare` | Nu încarcă configurul interactiv, recomandat în scripturi și CI |
| `--system-prompt "text"` | Suprascrie system prompt-ul cu text custom (pentru personalizare fină) |

### Parsare cu `--output-format json`

Dacă vrei să folosești rezultatul în alt script (ex. să îl trimiți prin email, să îl pui în Notion), cere JSON:

```bash
claude -p "Rezumă fișierul $FILE în trei propoziții." \
  --output-format json \
  --allowedTools "Read" \
  --bare > rezultat.json
```

Rezultatul e un obiect JSON cu câmpuri (mesajul, metadata, cost, etc.). Îl parsezi cu `jq`:

```bash
jq -r '.result' rezultat.json
```

Sau în Python, TypeScript, altceva, după nevoie.

---

## Agent SDK: Claude ca bibliotecă în programul tău

CLI-ul acoperă scripturi simple. Când ai ceva mai complex (aplicație web, integrare cu mai multe sisteme, logică pe care vrei să o controlezi strict), Agent SDK e soluția. Există două versiuni oficiale: Python și TypeScript.

### Instalare SDK Python

```bash
pip install claude-agent-sdk --break-system-packages
```

(Folosești `--break-system-packages` dacă ai Python gestionat de sistemul de operare. Altfel, folosește `venv` pentru izolare.)

### Primul exemplu Python

Un caz simplu. Vrei o funcție care primește un text și întoarce un rezumat în română, folosind skills-urile tale de stil.

```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def rezuma_text(text: str) -> str:
    options = ClaudeAgentOptions(
        system_prompt="Rezumă textul primit în 3 bullet-uri scurte, în română cu diacritice.",
        allowed_tools=[],
    )
    rezultat_final = ""
    async for message in query(
        prompt=f"Text de rezumat:\n{text}",
        options=options,
    ):
        if hasattr(message, "result"):
            rezultat_final = message.result
    return rezultat_final

async def main():
    text = "Conținut lung pe care vreau să-l rezum..."
    rezumat = await rezuma_text(text)
    print(rezumat)

asyncio.run(main())
```

Ce face. Definește o funcție `rezuma_text` care primește text. Construiește opțiuni (system prompt personalizat, zero tool-uri, nu are nevoie). Lansează `query()` care e iterator asincron, primește mesaje pe măsură ce Claude lucrează. Când vine mesajul final (cel cu `.result`), îl salvează și îl întoarce. Main-ul îl cheamă, afișează rezumatul.

Fiecare apel `query()` pornește proaspăt, fără memoria apelurilor anterioare. E izolat default.

### Primul exemplu TypeScript

Echivalent în TypeScript, pentru cei care preferă Node.js:

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

async function rezumaText(text: string): Promise<string> {
  let rezultatFinal = "";
  for await (const message of query({
    prompt: `Text de rezumat:\n${text}`,
    options: {
      systemPrompt: "Rezumă textul primit în 3 bullet-uri scurte, în română cu diacritice.",
      allowedTools: [],
    },
  })) {
    if ("result" in message) {
      rezultatFinal = message.result as string;
    }
  }
  return rezultatFinal;
}

(async () => {
  const text = "Conținut lung pe care vreau să-l rezum...";
  const rezumat = await rezumaText(text);
  console.log(rezumat);
})();
```

Structura logică e aceeași. Diferența e doar sintactică.

### Când alegi CLI și când alegi SDK

| Situație | Alegere |
|---|---|
| Script bash simplu, rulat cu cron, un singur pas | CLI (`claude -p`) |
| Logică complexă cu multiple apeluri Claude într-un flow | SDK |
| Integrezi Claude într-o aplicație web pe care o construiești | SDK |
| Ai nevoie de control fin asupra fiecărui mesaj, tool call, răspuns | SDK |
| Vrei rapiditate, nu vrei să scrii cod | CLI |
| Ai nevoie de memorie peste apeluri (sesiuni, threads) | SDK |

Regula de pornire. Începe cu CLI. Dacă ajungi la limita lui (logică ramificată, multiple pași condiționați, integrare în aplicație), treci la SDK.

---

## Scenarii concrete pentru solopreneur

### Scenariu 1: rezumatul săptămânal automat

În fiecare duminică seară, ai nevoie de un rezumat al săptămânii: ce ai lucrat, ce e pe stiva pentru săptămâna viitoare. Ai notele în fișiere Markdown într-un dosar.

Cron job duminică 19:00: rulează script care cheamă `claude -p` cu prompt "citește toate notele din săptămâna asta și produ un raport cu trei secțiuni: ce s-a livrat, ce e în lucru, ce e pe stivă pentru săptămâna viitoare". Rezultatul ajunge într-un fișier numit `raport-sapt-YYYY-MM-DD.md`.

Timp de implementare: o seară. Timp economisit: 30 minute săptămânal pe perpetuitate.

### Scenariu 2: triaj de emailuri dimineața

În fiecare dimineață, Gmail-ul tău are 20-40 emailuri noi. Vrei triaj rapid: care sunt urgente, care cer răspuns personal, care pot fi arhivate automat.

CLI sau SDK (depinde de complexitate). Un script care citește emailurile prin MCP Gmail server, le trimite lui Claude cu un prompt de clasificare, primește o listă cu verdicte. Pentru fiecare verdict, aplică acțiunea (arhivare automată, mutare în folder prioritar, draft de răspuns pentru cele care cer personal).

Timp de implementare: un weekend. Timp economisit: 20-30 minute zilnic.

### Scenariu 3: produs care include Claude

Ai construit o aplicație web pentru coaching pe care o vinzi clienților. Una dintre funcționalități: clientul își uploadează un jurnal săptămânal, aplicația produce un feedback structurat.

SDK obligatoriu. Integrezi `claude_agent_sdk` în backend-ul aplicației (Node.js sau Python). Când vine un upload, îl trimiți la Claude cu un prompt personalizat (folosind skills-uri de analiză), primești rezultatul, îl returnezi clientului.

Aici nu mai e vorba de automatizare internă. E parte din produsul pe care îl livrezi. Investiția merită proporțional cu valoarea produsului.

---

## Referință

### Comenzi CLI esențiale

| Comandă | Ce face |
|---|---|
| `claude` | Pornește REPL interactiv în terminal (alternativă la panoul VS Code) |
| `claude "prompt"` | Pornește REPL cu un prompt inițial |
| `claude -p "prompt"` | Rulează headless, primește răspuns, iese |
| `claude -p "prompt" --output-format json` | Răspuns în JSON pentru parsare |
| `claude --continue` | Continuă ultima conversație |
| `claude --resume` | Alege o conversație anterioară pentru reluare |

### Agent SDK, funcția principală

| Limbaj | Import | Funcție |
|---|---|---|
| Python | `from claude_agent_sdk import query, ClaudeAgentOptions` | `query(prompt, options)` iterator asincron |
| TypeScript | `import { query } from "@anthropic-ai/claude-agent-sdk"` | `query({ prompt, options })` iterator asincron |

### Opțiuni comune SDK

| Opțiune | Ce face |
|---|---|
| `system_prompt` | System prompt custom |
| `allowed_tools` | Lista de tool-uri permise |
| `permission_mode` | `default`, `acceptEdits`, `plan`, `bypassPermissions` |
| `cwd` | Directorul de lucru |
| `max_turns` | Limită maximă de ture în conversație |

---

## Capcane frecvente

Capcane acoperite: automatizare prematură fără caz real, permisiuni prea largi în automat, credențiale hard-codate în script, ne-verificarea rezultatului înainte să aibă efecte, costuri neașteptate la scale, pipeline care rulează chiar și când nu e nevoie.

### Automatizare prematură fără caz de utilizare real

**Simptom:** Ai construit un pipeline complex, rulează frumos, dar îți dai seama după două săptămâni că rezultatul nu îți folosește la nimic.
**Cauză:** Te-ai grăbit să automatizezi fără să identifici clar ce economisești și cine folosește rezultatul.
**Fix:** Înainte de orice pipeline, răspunde. Care e task-ul manual pe care îl repeți? De câte ori pe săptămână? Cât durează? Dacă nu e sub 30 minute pe săptămână economisite, probabil nu justifică efortul. Dacă e peste 2 ore pe săptămână, e candidat evident. Sub 30 minute, continuă manual.

### Permisiuni prea largi în mod automat

**Simptom:** Script-ul automat a șters fișiere importante sau a făcut schimbări nedorite.
**Cauză:** Ai rulat `claude -p` fără `--allowedTools`, sau cu toate tool-urile permise. Când rulează automat, fără supervizare, Claude poate interpreta ceva greșit.
**Fix:** Principiul privilegiilor minime. Dă exact tool-urile necesare pentru task. Un script de rezumat are nevoie doar de Read, nu de Edit sau Bash. Un script de triaj email are nevoie de tool-urile MCP Gmail, nu de Write. Folosește `--allowedTools "Read,Grep"` și atât. Dacă scriptul îți cere mai multe, refuză și evaluează de ce.

### Credențiale în cleartext în scripturi

**Simptom:** Ai API key, Notion token, Gmail credentials direct în script. Scriptul ajunge pe GitHub sau pe un calculator partajat.
**Cauză:** Lene sau neatenție. Credențialele par convenabile în cod.
**Fix:** Folosește variabile de mediu. În script:
```bash
export NOTION_API_KEY="${NOTION_API_KEY}"
```
Păstrează credențialele într-un fișier `.env` local, adăugat la `.gitignore`. Pentru producție, folosește sisteme de secrets management (Vault, AWS Secrets Manager, 1Password CLI).

### Rezultat neverificat care declanșează acțiuni

**Simptom:** Pipeline-ul procesează, output-ul e aplicat direct (email trimis, fișier șters, Notion actualizat). Uneori, Claude interpretează greșit și aplici ceva nedorit.
**Cauză:** Ai sărit validarea între răspunsul lui Claude și acțiunea finală.
**Fix:** Separă producția răspunsului de aplicarea lui. Claude produce rezultatul, scriptul îl validează (conține cuvintele cheie așteptate? e într-un format valid? are lungimea rezonabilă?), și doar după validare aplică. Pentru acțiuni ireversibile (trimitere email, ștergere), ia în calcul un pas de review uman, cel puțin la început.

### Costuri neașteptate la scale

**Simptom:** Ai construit un pipeline, rulează de 10 ori pe zi pe texte lungi. La sfârșitul lunii, vezi factura mai mare decât te-ai aștepta.
**Cauză:** Fiecare apel Claude consumă tokeni. Pipeline-urile automate care procesează volume pot ajunge rapid la sume semnificative.
**Fix:** Monitorizare. Urmărește câți tokeni consumă un apel tipic. Înmulțește cu frecvența lunară. Dacă ai un pipeline care rulează de 100 de ori pe lună cu 10,000 tokeni per apel, costul e previzibil. Pentru volume mari, alege modele mai mici (Haiku în loc de Sonnet) când complexitatea task-ului permite. Setează limite de buget în dashboard-ul Anthropic și alerte dacă sunt depășite.

### Pipeline care rulează chiar și când nu e nevoie

**Simptom:** Ai un cron job zilnic. Uneori nu ai input nou (n-ai scris note ieri). Pipeline-ul rulează oricum, generează output gol sau confuz.
**Cauză:** Nu ai condiții de oprire. Scriptul rulează orb.
**Fix:** Verifică prerequisitele înainte de a cheama Claude. Dacă input-ul lipsește sau e gol, exit timpuriu fără să consumi tokeni. Exemplu:
```bash
if [ ! -s "$INPUT_FILE" ]; then
  echo "Nu sunt note de procesat. Exit."
  exit 0
fi
```

---

## Următorul pas

Felicitări. Ai parcurs toate cele trei părți ale ghidului. De aici înainte, urmează consolidare prin practică. Ghidul continuă cu trei anexe utile pentru referință rapidă.

→ [Anexa A, Glosar](../anexe/glosar.md). Definiții scurte pentru toți termenii tehnici apăruți în ghid.
→ [Anexa B, Prompturi esențiale](../anexe/prompturi-esentiale.md). Colecție de prompturi testate pentru scenarii tipice ale solopreneurului.
→ [Anexa C, Troubleshooting](../anexe/troubleshooting.md). Probleme comune și soluții rapide, organizate per strat.

---

## Surse

- [Run Claude Code programmatically](https://docs.claude.com/en/docs/claude-code/headless)
- [Agent SDK overview](https://docs.claude.com/en/api/agent-sdk/overview)
- [Agent SDK reference, Python](https://docs.claude.com/en/api/agent-sdk/python)
- [Agent SDK reference, TypeScript](https://docs.claude.com/en/api/agent-sdk/typescript)
- [Migrate to Claude Agent SDK](https://docs.claude.com/en/docs/claude-code/sdk/migration-guide)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
