*Partea III, Construcție > 3.2 Plugins*

# 3.2 Plugins

**Cum împachetezi skills, subagenți, comenzi custom, hooks și MCP servers într-un singur pachet pe care tu sau alții îl instalează cu un click, primind toată configurația ta deodată.**

| Meta | |
|---|---|
| Timp de setup | 4-8 ore pentru primul plugin solid, apoi mai repede |
| Timp de învățare până la fluență | 2-3 luni, după ce ai distribuit cel puțin un plugin real |
| Valoare pentru solopreneur | Variabilă. Mare dacă ai produs (curs, comunitate, consultanță) unde vrei să livrezi workflow-ul tău clienților. Medie dacă vrei doar să-ți replici setup-ul între calculatoare |
| Dependențe | 3.1 Skills completă. Ideal și 2.1-2.3 pentru a împacheta plugin-uri complete cu toate componentele |

**Acoperă:** ce e un plugin și când merită efortul, structura unui dosar de plugin (manifestul `.claude-plugin/plugin.json`, dosarele `skills/`, `agents/`, `commands/`, `hooks/`, `.mcp.json`), distribuție prin marketplace propriu (`marketplace.json`), instalare prin `/plugin` în panou, restricții de nume rezervate și impersonare, variabila `${CLAUDE_PLUGIN_ROOT}` pentru referințe interne la scripturi.
**Nu acoperă:** LSP servers (depășește scopul solopreneurului), plugin advanced patterns cu dependențe complexe, scoaterea Claude din VS Code în pipeline-uri automatizate (vezi 3.3).

---

## TL;DR

Un plugin e un pachet care conține mai multe componente Claude Code (skills, subagenți, comenzi custom, hooks, MCP servers) într-un singur dosar cu structură standard. Tu (sau altcineva) instalezi pachetul cu `/plugin install` din panou, iar toată configurația apare deodată. Util pentru două scenarii. Primul, replici setup-ul tău între calculatoare sau proiecte. Al doilea, îl distribui către alții (clienți, studenți, comunitate) ca produs sau bonus. Marketplace-ul propriu e cum faci distribuția accesibilă la un click.

---

## De ce contează

Ai construit skills-uri, ai configurat subagenți, ai scris câteva hooks, ai conectat MCP servers la Gmail și Notion. Totul merge perfect pe calculatorul tău. Acum vrei două lucruri.

Primul, să rulezi același setup pe al doilea calculator (laptop de călătorie), pe un proiect nou, sau după o reinstalare. Fără să reconfigurezi manual totul de la zero.

Al doilea, și aici e partea interesantă pentru solopreneurul care educă sau vinde, vrei ca cineva care îți cumpără cursul sau intră în comunitatea ta să primească workflow-ul tău gata de folosit. Nu o listă de instrucțiuni de 20 de pagini. O instalare rapidă, un click, toate skills-urile și subagenții tăi sunt acolo.

Plugin-urile fac asta posibil. Împachetezi tot într-un singur loc, livrezi ca fișier (sau prin Git, sau printr-un marketplace), primitorul instalează cu o comandă. Sfârșitul configurărilor manuale repetitive.

Pentru solopreneur, valoarea strategică e semnificativă. Workflow-ul tău, pe care l-ai rafinat luni de zile, devine un livrabil digital. Poate fi bonus la curs, parte din ofertă de consultanță, produs vândut separat pe platformă. Un plugin bine construit e investiție cu rezultat compus.

Onest, e strat opțional. Dacă ești solo și rămâi solo, fără plan de distribuție, plugin-urile sunt lux. Valoarea lor se vede când apare un al doilea utilizator (tu pe alt dispozitiv, un coleg, un client, 500 de cursanți).

---

## Ce vei putea face după

- Înțelegi ce intră într-un plugin și ce nu
- Construiești structura de bază a unui plugin nou pornind de la skills-urile pe care le ai deja
- Creezi manifestul `.claude-plugin/plugin.json` cu metadatele esențiale
- Instalezi un plugin local (dezvoltare) și unul de la distanță (testare)
- Creezi propriul marketplace pentru a distribui plugin-uri multiple
- Gestionezi versiuni și update-uri fără să rupi configurațiile utilizatorilor

---

## Modelul mental: setul de unelte portabil

Până aici ai construit componente individuale: un skill de scriere, un subagent de code review, un hook de log, un MCP server de Notion. Fiecare stă în locul lui (unele la user, altele la proiect, altele în config separat). Când vrei să dai cuiva "tot setup-ul tău", durează pentru că informația e împrăștiată.

Plugin-ul e cutia în care pui toate uneltele și o închizi. O etichetezi (manifest), o împachetezi, o dai mai departe. Cineva o primește, o deschide, toate uneltele apar instalate.

Trei consecințe ale modelului.

**Plugin-ul e agnostic față de conținut.** Nu contează dacă pui în el 10 skills sau unul singur. Nu contează dacă are sau nu subagenți. Pachetul funcționează cu ce pui în el. Plec de la setup-ul tău existent, îl împachetez, gata.

**Plugin-ul adaugă mentenanță.** Când rulezi skills-urile tale local, le modifici direct și merge. Într-un plugin distribuit, orice modificare trebuie versionată și propagată la utilizatori. Asta e cost. Merită plătit doar dacă ai mai mulți utilizatori decât tine.

**Marketplace-ul e vitrina.** Un plugin singur e un dosar. Un marketplace e un catalog unde listezi mai multe plugins și le expui cu instalare ușoară. Pentru distribuție către comunitate, ai nevoie de marketplace.

---

## Cum folosești

### Structura unui plugin

Un plugin e un dosar. Structura minimă:

```
nume-plugin/
  ├── .claude-plugin/
  │   └── plugin.json
  ├── skills/
  │   ├── skill-1/
  │   │   └── SKILL.md
  │   └── skill-2/
  │       └── SKILL.md
  ├── agents/
  │   └── code-reviewer.md
  ├── commands/
  │   └── follow-up.md
  ├── hooks/
  │   └── hooks.json
  └── .mcp.json
```

Regulă importantă de ținut minte. Doar `plugin.json` stă în dosarul `.claude-plugin/`. Toate celelalte dosare (`skills/`, `agents/`, `commands/`, `hooks/`) stau la rădăcina plugin-ului, direct sub numele lui. E o greșeală comună să le pui toate sub `.claude-plugin/`, iar Claude Code nu le recunoaște atunci.

Niciuna dintre secțiuni nu e obligatorie per se. Un plugin poate avea doar skills, sau doar un subagent, sau doar un MCP server. Pui doar ce ai.

### Manifestul `plugin.json`

Fișierul de identitate al plugin-ului. Exemplu:

```json
{
  "name": "workflow-solopreneur-dm",
  "version": "1.0.0",
  "description": "Setul meu de skills și subagenți pentru scriere, review și procesare feedback client. Stil Dănuț Mitrut.",
  "author": {
    "name": "Dănuț Mitrut",
    "email": "contact@exemplu.ro"
  },
  "homepage": "https://exemplu.ro"
}
```

Câmpurile principale:

| Câmp | Ce face |
|---|---|
| `name` | Numele intern al plugin-ului, fără spații. Apare în manager-ul de plugins |
| `version` | Versiunea (semantic versioning: major.minor.patch). Schimbă-l la fiecare release semnificativ |
| `description` | Fraza care apare în listă când cineva navighează marketplace-ul |
| `author` | Cine l-a scris. Util pentru atribuire |
| `homepage` | Link către site-ul tău unde explici mai mult |

Nume rezervate. Numele de tip `claude-code-marketplace`, `claude-code-plugins` și alte nume oficiale sunt blocate. Nu poți impersona marketplace-ul oficial. Alege un nume propriu, descriptiv.

### Construiești plugin-ul pornind de la skills existente

Cea mai ușoară cale e să iei skills-urile pe care le ai deja și să le copiezi în structura unui plugin.

Pas 1. Creezi dosarul `workflow-solopreneur-dm/` undeva în calculatorul tău (pe desktop, într-un `~/plugins/` dedicat, oriunde).

Pas 2. Creezi subfolder-ul `.claude-plugin/` și pui în el `plugin.json` cu metadatele de mai sus.

Pas 3. Creezi subfolder-ul `skills/` și copiezi fiecare skill existent (cu tot conținutul dosarului, `SKILL.md` plus orice fișiere auxiliare).

Pas 4. Dacă ai subagenți pe care vrei să-i incluzi, copiezi fișierele din `~/.claude/agents/` sau din `.claude/agents/<proiect>/` în dosarul `agents/` al plugin-ului.

Pas 5. Pentru comenzi custom (slash commands din 1.4), copiezi în `commands/`.

Pas 6. Pentru hooks, copiezi fișierul JSON în `hooks/hooks.json`.

Pas 7. Pentru MCP servers, pui un `.mcp.json` la rădăcina plugin-ului.

### Instalezi un plugin local

Înainte să distribui, testezi local. În panoul Claude, tastezi:

```
/plugin install ~/plugins/workflow-solopreneur-dm
```

Claude Code recunoaște dosarul, citește manifestul, instalează toate componentele. De acum, skills-urile, subagenții, hooks-urile, MCP server-ii din plugin sunt activi.

Pentru a dezinstala:

```
/plugin remove workflow-solopreneur-dm
```

Instalarea locală e utilă în timpul dezvoltării. Modifici un skill în dosarul plugin-ului, rulezi `/plugin` pentru verificare, schimbarea e vizibilă.

### Distribuție prin marketplace

Marketplace-ul e fișierul `marketplace.json` care listează plugin-urile tale și unde stau fizic (repo Git, URL, cale locală).

Exemplu minim:

```json
{
  "name": "marketplace-solopreneur-dm",
  "description": "Plugin-uri pentru solopreneurul care lucrează cu Claude Code",
  "plugins": [
    {
      "name": "workflow-solopreneur-dm",
      "description": "Skills și subagenți pentru scriere și review",
      "version": "1.0.0",
      "source": {
        "type": "git",
        "url": "https://github.com/utilizator/workflow-solopreneur-dm"
      }
    },
    {
      "name": "brand-voice-dm",
      "description": "Skill dedicat stilului meu de scriere",
      "version": "1.2.0",
      "source": {
        "type": "git",
        "url": "https://github.com/utilizator/brand-voice-dm"
      }
    }
  ]
}
```

Marketplace-ul stă fie pe GitHub (ca un repo separat, public), fie pe serverul tău, fie local pentru testare.

Un utilizator care vrea să se conecteze la marketplace-ul tău face o dată:

```
/plugin marketplace add https://github.com/utilizator/marketplace-solopreneur-dm
```

De atunci, vede toate plugin-urile tale și poate instala individual:

```
/plugin install workflow-solopreneur-dm
```

### Versiuni și update-uri

Când modifici un plugin, crești versiunea în `plugin.json` (ex. de la `1.0.0` la `1.0.1` pentru fix, `1.1.0` pentru feature nou, `2.0.0` pentru breaking change).

Utilizatorii verifică update-urile cu:

```
/plugin update workflow-solopreneur-dm
```

Sau toate deodată:

```
/plugin update
```

Regulile de versionare semantică (semver):
- **Patch (1.0.0 → 1.0.1)**: fix-uri mici, nu strică nimic existent
- **Minor (1.0.0 → 1.1.0)**: feature-uri noi, compatibil cu versiunile anterioare
- **Major (1.0.0 → 2.0.0)**: breaking change, utilizatorii pot fi afectați

---

## Scenarii concrete pentru solopreneur

### Scenariu 1: replicare setup între calculatoare

Ai setup-ul complet pe MacBook. Cumperi un laptop Windows sau reinstalezi macOS. Vrei să reproduci totul rapid.

Împachetezi ce ai într-un plugin `my-claude-setup`, îl urci pe GitHub într-un repo privat, pe noul calculator rulezi `/plugin marketplace add` și `/plugin install`. În câteva minute ești la paritate.

### Scenariu 2: bonus la un curs sau produs digital

Ai un curs despre folosirea Claude Code pentru solopreneuri. Oferi ca bonus un plugin cu skills-urile tale cele mai folosite. Cumpărătorul cursului primește acces la marketplace-ul tău (repo GitHub public sau privat, cu invitație), instalează plugin-ul, începe să lucreze cu metodologia ta fără configurare.

Valoarea pentru el e enormă. Economisește săptămâni de experimentare. Pentru tine, e bonus care crește perceput valoarea ofertei fără muncă suplimentară.

### Scenariu 3: comunitate plătită cu plugin în evoluție

Ai o comunitate plătită de solopreneuri. Menții un plugin pe care îl actualizezi lunar cu skills noi, pattern-uri noi, subagenți noi. Membrii comunității rulează `/plugin update` săptămânal și primesc îmbunătățirile fără efort.

Modelul e recurent: plugin-ul devine unul dintre pilonii ofertei, nu produs one-off. Utilizatorii rămân membri parțial ca să primească update-urile.

---

## Referință

### Structura minimă de plugin

```
nume-plugin/
  ├── .claude-plugin/
  │   └── plugin.json
  └── skills/ (sau agents/, commands/, hooks/, .mcp.json)
```

### Componente posibile într-un plugin

| Componentă | Locație în plugin | Corespunde cu |
|---|---|---|
| Skills | `skills/<nume>/SKILL.md` | 3.1 Skills |
| Subagenți | `agents/<nume>.md` | 2.1 Subagenți |
| Slash commands custom | `commands/<nume>.md` | 1.5 Memorie, comenzi custom |
| Hooks | `hooks/hooks.json` | 2.3 Hooks |
| MCP servers | `.mcp.json` la rădăcina plugin-ului | 2.2 MCP servers |

### Comenzi principale în panou

| Comandă | Ce face |
|---|---|
| `/plugin install <cale sau nume>` | Instalează plugin local sau din marketplace |
| `/plugin remove <nume>` | Dezinstalează plugin |
| `/plugin list` | Vezi ce plugins sunt active |
| `/plugin update` | Verifică și aplică update-uri |
| `/plugin marketplace add <url>` | Adaugi marketplace nou ca sursă |

### Variabila `${CLAUDE_PLUGIN_ROOT}`

În interiorul plugin-ului, dacă ai scripturi care trebuie să referiască alte fișiere din plugin, folosește `${CLAUDE_PLUGIN_ROOT}`. Se rezolvă la calea absolută unde plugin-ul e instalat la user.

Exemplu, într-un hook:

```json
{
  "type": "command",
  "command": "bash ${CLAUDE_PLUGIN_ROOT}/scripts/format.sh"
}
```

---

## Capcane frecvente

Capcane acoperite: `plugin.json` pus în dosarul greșit, versiune neactualizată la release, marketplace fără teste înainte de publicare, credențiale hard-codate în plugin distribuit, efort disproporționat față de audiență, dependențe între componente nedocumentate.

### `plugin.json` pus în dosarul greșit

**Simptom:** Instalezi plugin-ul, Claude Code dă eroare "manifest not found" sau plugin-ul apare gol.
**Cauză:** Ai pus `plugin.json` direct la rădăcina plugin-ului, nu în `.claude-plugin/`. Sau invers, ai pus toate dosarele (`skills/`, `agents/`) sub `.claude-plugin/`.
**Fix:** Structura corectă. `.claude-plugin/plugin.json` și doar atât în `.claude-plugin/`. `skills/`, `agents/`, `commands/`, `hooks/`, `.mcp.json` stau la rădăcina plugin-ului, alături de `.claude-plugin/`.

### Versiune neactualizată la release

**Simptom:** Distribui o versiune nouă, utilizatorii rulează `/plugin update`, nu primesc nimic. Sau primesc versiunea nouă dar `/plugin list` arată versiunea veche.
**Cauză:** Ai modificat conținutul plugin-ului fără să crești versiunea în `plugin.json`.
**Fix:** De fiecare dată când distribui modificări, crește versiunea conform semver. Patch pentru fix-uri mici, minor pentru adăugiri, major pentru breaking changes. Fără versiune nouă, clientul nu știe că există update.

### Marketplace fără teste înainte de publicare

**Simptom:** Publici marketplace-ul, utilizatorii încearcă să instaleze, primesc erori. Reputația ta ca distribuitor e afectată.
**Cauză:** Nu ai testat integrarea. Plug-in-ul tău merge pe calculatorul tău (unde e deja configurat parțial), dar nu pornește curat pe un calculator virgin.
**Fix:** Înainte de publicare, testează pe un calculator separat sau o mașină virtuală cu Claude Code proaspăt instalat. Fă setup-ul complet doar prin plugin. Vezi ce lipsește. Documentează prerequisitele (Node.js, credențiale care trebuie completate, variabile de mediu).

### Credențiale hard-codate în plugin distribuit

**Simptom:** Ai distribuit plugin-ul, utilizatorii raportează că l-au instalat și Claude începe să facă cereri la Gmail-ul tău sau la Notion-ul tău.
**Cauză:** Ai pus credențialele reale în `.mcp.json` sau într-un skill. Distribuirea le-a împrăștiat.
**Fix:** Înainte de distribuție, scoate orice credențial din fișiere. Folosește variabile de mediu (ex. `${NOTION_API_KEY}`) și documentează în README-ul plugin-ului că utilizatorii trebuie să le seteze local. Niciun token real nu ar trebui să ajungă pe GitHub public sau în marketplace distribuit.

### Efort disproporționat față de audiență

**Simptom:** Ai pus 20 de ore să construiești un plugin. Ai trei utilizatori, dintre care doi sunt tu pe alt calculator. Nu pare că justifică efortul.
**Cauză:** Ai sărit la plugin înainte să valorizezi oferta. Plugin-urile sunt utile când ai audiență reală sau nevoie reală de distribuție.
**Fix:** Verifică înainte. Ai minim 10 oameni care chiar folosesc ceea ce faci (studenți, clienți, comunitate)? Da, plugin merită. Nu, păstrează skills-urile locale, economisești timp. Poți împacheta oricând mai târziu când cererea apare.

### Dependențe între componente nedocumentate

**Simptom:** Instalezi plugin-ul, skills-urile par să meargă, dar unul dintre ele încearcă să folosească un MCP server care nu pornește. Sau un hook referă un script care nu există.
**Cauză:** Componentele plugin-ului depind una de alta (skill-ul presupune că MCP-ul e activ), dar nu ai documentat asta sau nu ai testat fluxul complet.
**Fix:** Creează o secțiune "Prerequisite" în README-ul plugin-ului. Listează explicit ce trebuie să fie instalat înainte (Node.js, Python, credențiale, alte plugin-uri). Testează scenariul "primul install" înainte de publicare.

---

## Următorul pas

→ [3.3 SDK și headless](sdk-headless.md). Până acum, tot ce am construit trăiește în panoul Claude Code din VS Code. Stratul final scoate Claude din VS Code și îl pune în pipeline-uri automate: scripturi care rulează la 3 noaptea, integrări în sisteme business, agenți care fac munca fără ca tu să fii pe calculator. Aici intră CLI-ul `claude` și Agent SDK.

---

## Surse

- [Create plugins](https://docs.claude.com/en/docs/claude-code/plugins)
- [Plugins reference](https://docs.claude.com/en/docs/claude-code/plugins-reference)
- [Create and distribute a plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
