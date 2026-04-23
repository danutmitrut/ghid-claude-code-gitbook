*Partea II, Extensii > 2.2 MCP servers*

# 2.2 MCP servers

**Cum îi dai lui Claude acces la sistemele tale externe (Gmail, Notion, GitHub, baze de date, API-uri) printr-un fișier de configurare și câteva click-uri. Fără să scrii cod de integrare.**

| Meta | |
|---|---|
| Timp de setup | 1-2 ore pentru primul server, 10-20 minute pentru următoarele |
| Timp de învățare până la fluență | 2-3 săptămâni |
| Valoare pentru solopreneur | Mare. Aici Claude iese din workspace și atinge restul sistemelor tale |
| Dependențe | 1.1 până la 1.5 complete, Node.js instalat (explicat mai jos) |

**Acoperă:** ce înseamnă un MCP server și de ce există, adăugare prin editarea fișierului `.mcp.json` din rădăcina proiectului, trei scope-uri (project partajabil cu echipa, user global peste toate proiectele, local privat), verificare status cu `/mcp` în panou, prompturi MCP care apar ca slash commands în panou, exemple de servere uzuale (Notion, Gmail, GitHub, Postgres).
**Nu acoperă:** construcție de MCP server propriu (depășește scopul ghidului pentru solopreneur), automatizare pe evenimente cu hooks (vezi 2.3), cum transformi un workflow în skill reutilizabil (vezi 3.1).

---

## TL;DR

Un MCP server e un program mic care stă între Claude și un sistem extern (Gmail, Notion, GitHub, baza ta de date). Tu îi spui lui Claude "conectează-te la Gmail" printr-un fișier de configurare, iar de atunci Claude poate citi emailurile, poate căuta în Notion, poate crea issue-uri în GitHub, poate interoga baza de date, fără să ieși din panoul extensiei. Setup-ul e o dată per server (editezi `.mcp.json`, introduci credențiale). De atunci, sistemul apare în context ca orice alt instrument al lui Claude.

---

## De ce contează

Până aici, Claude a lucrat în workspace-ul tău. Citește fișiere, scrie cod, reține context despre proiect. E util, dar lumea ta reală nu stă doar în fișierele locale. Clienții scriu pe email. Note de ședință stau în Notion. Task-urile sunt în GitHub sau Linear. Invoice-urile în Stripe. Baza de date cu clienți e într-un Postgres separat.

Fără MCP, rutina e așa: deschizi Gmail în browser, copiezi emailul de la client, te întorci în VS Code, pastezi în panoul cu Claude ca să-i dai context, îi ceri să-ți scrie răspunsul, iei răspunsul, te întorci în Gmail, îl trimiți. Cu cât ai mai multe sisteme, cu atât devine mai obositoare cărățile astea de informație dintr-o parte în alta.

MCP (Model Context Protocol) închide cercul. E o convenție prin care Claude poate vorbi direct cu sistemele externe. Nu mai copiezi emailul manual, îi spui lui Claude "uită-te la ultimele 5 emailuri de la X, scrie un răspuns la cel mai recent". El citește direct din Gmail, îți prezintă răspunsul, îl trimite dacă îi dai ok.

Beneficiul pentru solopreneur e disproporționat. Tu ești singura persoană care face tot. Economia de timp din integrarea sistemelor e ore pe săptămână, nu minute.

---

## Ce vei putea face după

- Înțelegi ce e un MCP server și când să adaugi unul
- Editezi `.mcp.json` la nivel de proiect pentru a conecta primul server
- Alegi între scope project (partajabil), user (peste toate proiectele tale) și local (privat)
- Verifici cu `/mcp` dacă un server e conectat corect
- Recunoști și folosești slash commands generate de MCP servers
- Decizi ce credențiale păstrezi în proiect și care rămân în configul tău personal

---

## Modelul mental: adaptor universal între Claude și lumea ta

Gândește-te la MCP ca la un adaptor universal, ca USB-C pentru Claude. Înainte de USB-C, fiecare dispozitiv avea mufa lui (un cablu pentru imprimantă, altul pentru mouse, altul pentru telefon). Dacă Claude ar fi vrut să vorbească cu Gmail, cineva trebuia să scrie cod dedicat Gmail-Claude. Pentru Notion, cod dedicat Notion-Claude. Pentru fiecare combinație, muncă nouă.

MCP standardizează conectorul. Oricine face un MCP server (și există deja zeci pentru serviciile mari) expune aceleași tipuri de "prize" (tool-uri, resurse, prompturi). Claude știe să le recunoască indiferent de ce sistem e în spate. Tu doar îl conectezi și funcționează.

Un MCP server e, tehnic, un program care rulează local (sau remote) și face trei lucruri. Oferă tool-uri (acțiuni pe care Claude le poate executa, cum ar fi "trimite email"). Oferă resurse (date pe care Claude le poate citi, cum ar fi "lista ultimelor 10 emailuri"). Oferă prompturi (scurtături predefinite pe care tu le invoci ca slash commands).

Pentru tine, ca solopreneur, modelul de gândit e simplu. Fiecare sistem cu care lucrezi zilnic are, cel mai probabil, un MCP server gata construit. Tu îl instalezi o dată, îl configurezi cu credențialele tale, de atunci Claude vorbește cu sistemul ăla ca și cum ar fi parte din workspace-ul tău.

---

## Ce e nevoie înainte de primul server

Majoritatea MCP servers sunt programe Node.js (cele care se instalează cu `npm`) sau Python (cu `pip` sau `uvx`). Înseamnă că pe calculatorul tău trebuie să existe mediul respectiv instalat o dată. Pentru mulți solopreneuri care nu sunt developeri, asta e pasul cel mai nefamiliar, așa că merită explicat curat.

**Node.js** e un mediu de rulare pentru programe JavaScript. Descarci instalatorul de pe [nodejs.org](https://nodejs.org), îl rulezi ca pe orice alt program. După instalare, poți folosi comenzi `npx` (în câteva cazuri când un MCP server cere asta explicit, de obicei configurat automat). Nu trebuie să înveți să programezi în JavaScript. Node.js e doar motorul care rulează serverele în fundal.

**Python** e la fel, un mediu de rulare pentru programe Python. Descarci de pe [python.org](https://python.org) sau (pe Mac) îl ai deja instalat. Unele MCP servers cer Python în loc de Node.

Regula e așa. Când alegi un MCP server, pagina lui îți zice clar "necesită Node" sau "necesită Python". Instalezi ce lipsește o singură dată, de atunci nu te mai întorci la pasul ăsta.

> ⚠️ Pentru strict scope: documentația unor servere îți zice să rulezi comenzi de tip `npm install -g <nume-server>` în terminal. În VS Code ai un terminal integrat la `View > Terminal` (sau `Ctrl+backtick`). Deschizi terminalul acolo, pastezi comanda exactă din documentația MCP server-ului, apeși Enter. E pur și simplu rularea unui instalator standard, ca atunci când instalezi o aplicație din Mac App Store.

---

## Cum folosești

### Adaugi primul MCP server prin `.mcp.json`

Mergi în rădăcina proiectului tău (în VS Code, file explorer din stânga). Creezi un fișier numit `.mcp.json` dacă nu există deja. Structura de bază arată așa:

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_API_KEY": "secret_xxxxxxxxxxxx"
      }
    }
  }
}
```

Să desfacem câmpurile.

**`mcpServers`** e containerul. Toate serverele pe care le adaugi la proiect apar aici.

**`"notion"`** e numele pe care îl alegi tu pentru server. Poți avea mai multe instanțe (ex. `"notion-personal"` și `"notion-client"`), dar cel mai des folosești numele simplu al sistemului.

**`command`** e programul care pornește serverul. Majoritatea exemplelor folosesc `npx` (rulează un pachet Node fără instalare globală) sau `uvx` (echivalentul pentru Python).

**`args`** sunt argumentele transmise la pornire. De obicei e numele pachetului NPM sau Python.

**`env`** conține variabilele de mediu, cel mai des credențialele (token API, password). Se obțin din panoul sistemului extern (Notion îți dă un token de Integration, GitHub îți dă un Personal Access Token, Gmail folosește OAuth și tokenul apare în flow-ul de autentificare).

După ce salvezi fișierul, restartezi panoul Claude (închide panoul și redeschide-l) sau rulezi `/mcp` ca să vezi statusul. Dacă serverul e listat și marcat "connected", ești bine.

> **Prompt de verificare (după setup):**
> "Listează-mi ultimele 5 pagini pe care le-am creat în Notion."
>
> **Ce ar trebui să vezi:**
> Claude invocă un tool de tip `mcp__notion__list_pages` (sau similar, numele variază după server). Îți întoarce lista. Dacă eșuează, vezi în panou mesajul de eroare (de cele mai multe ori, token greșit sau permisiuni lipsă).

### Trei scope-uri, trei fișiere diferite

`.mcp.json` la nivel de proiect e doar una dintre opțiuni. În total ai trei scope-uri, cu locații diferite.

**Project scope** (`.mcp.json` în rădăcina proiectului): partajabil. Dacă pui proiectul pe GitHub sau îl trimiți unui colaborator, config-ul MCP merge cu el. Util pentru servere care țin de proiectul concret (baza de date specifică, API-ul clientului). Atenție: nu pune credențiale reale aici dacă proiectul ajunge pe GitHub public. Vezi secțiunea de capcane.

**User scope**: disponibil peste toate proiectele tale. Configurezi Gmail o singură dată la user, ai acces la el indiferent de proiectul în care lucrezi. Aici pui serverele pe care le folosești constant (Gmail, Notion personal, calendar). Claude Code gestionează asta prin configul intern al user-ului (fișierul `~/.claude.json`). Nu-l editezi direct de obicei, adaugi serverele în config local/user și Claude le notează singur.

**Local scope** (default): privat la tine, doar pe proiectul curent. Dacă experimentezi cu un server nou și nu ești sigur că vrei să-l salvezi peste tot, local scope e opțiunea conservativă.

Pentru început, recomandarea e: Notion, Gmail, alte tool-uri personale intră la **user scope**. Servere specifice proiectului (baza de date a clientului X) intră la **project scope**, cu credențiale păstrate separat.

### `/mcp` ca dashboard în panou

În panoul Claude, tastezi `/mcp`. Apare o listă cu toate serverele configurate, statusul lor (connected, error, disabled), ce tool-uri expun, ce prompturi oferă. E dashboard-ul tău de control.

De aici poți:
- Verifica dacă un server s-a conectat corect
- Vedea ce tool-uri sunt expuse (ca să știi ce poți cere lui Claude)
- Dezactiva temporar un server (dacă îți încarcă contextul inutil pentru task-ul curent)

Nu trebuie să memorezi lista. Tastezi `/mcp` când ai dubii.

### Prompturi MCP ca slash commands

Pe lângă tool-uri, un MCP server poate oferi prompturi predefinite. Tastezi `/` în panou, vezi în listă intrări de forma `/mcp__notion__summarize_page`. Acelea sunt prompturi pre-scrise de autorul serverului. Le apelezi, faci munca fără să mai formulezi tu promptul de la zero.

Format: `/mcp__<nume-server>__<nume-prompt>`.

Util când un workflow e repetitiv și autorul MCP l-a împachetat pentru tine. Nu toate serverele oferă prompturi, depinde de autorul pachetului.

---

## Exemple de servere uzuale pentru solopreneur

Scopul aici e orientarea, nu tutorialul complet. Vezi că există și ce fac.

**Notion.** Server oficial de la Notion. Expune tool-uri pentru căutare în workspace, citire de pagini, creare/editare. Util pentru solopreneuri care țin toate notele, documentația și knowledge base-ul în Notion. Claude poate referi direct la paginile tale fără să copiezi conținut.

**Gmail.** Server oficial sau community. Citire emailuri, căutare, trimitere, draft. Util pentru procesarea emailurilor clienților, răspunsuri cu context automat din workspace (Claude vede emailul și fișierul de proiect simultan, răspunde cu referințe concrete).

**GitHub.** Server oficial. Issues, pull requests, discuțiuni, cod. Util pentru solopreneuri care livrează produse open source sau care lucrează cu clienți pe GitHub. Claude poate crea issue-uri, comenta pe PR, căuta în codebase-ul altor repos.

**Postgres (sau MySQL, SQLite).** Server pentru baze de date. Claude interoghează direct baza, fără să ieși din panou. Util pentru analize rapide ("câți clienți am activi în luna asta?", "care sunt cele mai vândute produse?").

**Slack.** Dacă ai un workspace Slack unde ești în contact cu clienți. Căutare în conversații, trimitere mesaje, rezumate de canale.

**Browser (Playwright sau Puppeteer MCP).** Dă-i lui Claude un browser. Poate deschide pagini, completa formulare, face scraping, testa un site. Util pentru QA pe propriul produs sau automatizare de task-uri repetitive online.

Majoritatea acestor servere au pagini de setup clare cu exact blocul JSON pe care îl copiezi în `.mcp.json`. Nu îl scrii de la zero.

---

## Referință

### Cele trei scope-uri

| Scope | Locație | Cine vede | Când folosești |
|---|---|---|---|
| Project | `.mcp.json` în rădăcina proiectului | Toți colaboratorii (dacă e partajat) | Servere specifice proiectului (baza de date a clientului, API dedicat) |
| User | Config intern al user-ului | Doar tu, peste toate proiectele | Tool-uri personale pe care le folosești constant (Gmail, Notion personal, calendar) |
| Local | Config intern, doar proiect curent | Doar tu, doar aici | Experimentare, servere temporare |

### Structura unui server în `.mcp.json`

| Câmp | Obligatoriu | Ce face |
|---|---|---|
| `command` | Da | Programul care pornește serverul (de obicei `npx` sau `uvx`) |
| `args` | Da (în general) | Argumentele comenzii, inclusiv numele pachetului |
| `env` | Depinde | Variabile de mediu, cel mai des credențialele API |

### Cum apar tool-urile MCP în Claude

Format intern: `mcp__<nume-server>__<nume-tool>`. Nu trebuie să le scrii tu, Claude le invocă singur când contextul cere. Dar dacă vezi un tool call cu prefixul `mcp__`, știi că vine de la un MCP server.

---

## Capcane frecvente

Capcane acoperite: credențiale în proiect partajat, server care nu pornește din lipsa mediului, prea multe servere active simultan, credențiale expirate, confuzie cu tool-urile native, diferența față de subagenți.

### Credențiale puse în `.mcp.json` ajung pe GitHub

**Simptom:** Ți-ai configurat Notion în `.mcp.json` cu token-ul real, apoi ai făcut commit al proiectului pe GitHub. Token-ul e acum vizibil pentru oricine îți vede repository-ul.
**Cauză:** `.mcp.json` la scope project e partajabil. Conținutul lui ajunge în istoric Git dacă nu excluzi explicit.
**Fix:** Două opțiuni. Fie pui credențialele prin variabile de mediu pe sistemul tău și referi `"${NOTION_API_KEY}"` în `env` (Claude rezolvă variabila local, nu apare scrisă în fișier). Fie muți serverul la user scope, unde credențialele stau în configul tău personal. Pentru servere cu date sensibile (Gmail, banking, clienți), întotdeauna user scope. Pentru servere legate de proiect (o bază de date de test fără date reale), project scope e ok, dar verifică `.gitignore`.

### Server marcat "error" la `/mcp`

**Simptom:** Ai configurat un server, tastezi `/mcp`, vezi status "error" sau "failed to start".
**Cauză:** Cel mai des, lipsă de mediu (Node.js sau Python neinstalat), token API greșit, sau pachet instalat incorect.
**Fix:** Verifică mesajul de eroare afișat la `/mcp` sau în panoul de logs al extensiei. În ordine: 1) Node/Python instalat? 2) Token-ul e copiat corect (fără spații la final)? 3) Pachetul exact cum e scris în documentație? După corecție, restart panou.

### Prea multe servere, Claude încărcat

**Simptom:** Ai conectat 8 MCP servers, contextul e plin de descrieri de tool-uri, Claude pare confuz sau alege tool greșit.
**Cauză:** Fiecare server adaugă descrieri de tool-uri în contextul fiecărei sesiuni. Peste un prag, Claude are prea multe opțiuni și alege suboptim.
**Fix:** Păstrează active doar serverele pe care le folosești în proiectul curent. Dacă ai un proiect unde lucrezi doar în cod, poate n-ai nevoie de Gmail activ. Dezactivează temporar prin `/mcp` sau mută la alt scope. Revino periodic la lista de servere și scoate ce nu mai folosești.

### Credențiale expirate fără avertisment

**Simptom:** Claude încearcă să acceseze Gmail, primește eroare de autentificare. Token-ul OAuth a expirat.
**Cauză:** Tokenii din majoritatea serviciilor au durată de viață limitată. Unele MCP servers gestionează refresh automat, altele nu.
**Fix:** Când vezi eroare de auth, mergi în sistemul sursă (ex. Google Cloud Console pentru Gmail OAuth), regenerezi token-ul, îl actualizezi în `env`. Pentru servere cu auth volatilă, setează-ți un reminder să verifici periodic.

### Confuzia între tool-urile native ale lui Claude și cele MCP

**Simptom:** Nu știi dacă un anumit tool vine de la Claude direct sau de la un MCP server. Nu înțelegi de ce "Read" merge mereu dar "gmail_search" doar uneori.
**Cauză:** Tool-urile native (Read, Write, Edit, Grep, Bash) sunt parte din Claude Code. Tool-urile MCP vin de la servere externe și au prefixul `mcp__<server>__`. Disponibilitatea lor depinde de statusul serverului.
**Fix:** Când dai peste o eroare pe un tool cu prefix `mcp__`, verifică statusul serverului la `/mcp`. Când nu e prefix, problema e cu Claude direct sau cu permisiunile. Reperul e prefixul.

### Subagent vs MCP server, când folosești ce

**Simptom:** Nu știi dacă pentru un task extern (ex. verifică Gmail și rezumă) folosești un subagent sau un MCP server.
**Cauză:** Ambele extind capabilități, dar fac lucruri diferite. Subagentul e un Claude secundar cu context izolat. MCP server-ul e un conector la un sistem extern.
**Fix:** Ai nevoie de MCP server când Claude trebuie să atingă un sistem extern (email, Notion, GitHub). Ai nevoie de subagent când vrei să delegi o bucată de muncă pentru context izolat. Se combină natural: un subagent poate folosi MCP servers. Nu alegi între ele, ci conform scopului.

---

## Următorul pas

→ [2.3 Hooks](hooks.md). Până acum, toate interacțiunile au fost declanșate de tine. Pasul următor e să automatizezi: Claude Code rulează acțiuni în fundal când se întâmplă evenimente specifice (înainte de fiecare edit, după un tool call, la începutul sesiunii). Util pentru verificări automate și logging.

---

## Surse

- [Connect Claude Code to tools via MCP](https://docs.claude.com/en/docs/claude-code/mcp)
- [Model Context Protocol overview](https://docs.claude.com/en/docs/mcp)
- [Claude Code settings](https://docs.claude.com/en/docs/claude-code/settings)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
