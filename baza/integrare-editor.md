*Partea I, Bază > 1.3 Integrare cu editorul*

# 1.3 Integrare cu editorul

**Cum îi arăți lui Claude exact ce fișier, ce linii sau ce eroare din terminal te interesează, fără să copiezi și lipești.**

| Meta | |
|---|---|
| Timp de setup | 5 minute |
| Timp de învățare până la fluență | 1 oră de practică |
| Valoare pentru solopreneur | Mare, elimină 80% din copy-paste |
| Dependențe | 1.1 Instalare și 1.2 Primul contact complete |

**Acoperă:** `@file` pentru fișier întreg, `@file#linii` pentru linii specifice, `@directory` pentru tot dosarul, `@terminal` pentru output-ul terminalului, shortcut `Option+K` (macOS) sau `Alt+K` (Windows/Linux) pentru inserare rapidă din selecție, context automat din fișierul activ, ascundere selecție prin eye-slash.
**Nu acoperă:** plan mode și auto-accept (vezi 1.4), memorie persistentă între sesiuni (vezi 1.5), conectare la sisteme externe ca Gmail sau Notion (vezi 2.2).

---

## TL;DR

Claude nu vede tot workspace-ul în mod implicit. Vede ce îi arăți. @-mentions sunt modul prin care îi arăți: `@numefisier.ts`, `@utils.py#20-45`, `@terminal` pentru erori. Shortcut-ul `Option+K` sau `Alt+K` inserează rapid un mention pentru liniile selectate în editor. Când înveți asta, nu mai deschizi al doilea tab ca să copiezi text în prompt.

---

## De ce contează

Când lucrezi cu un coleg uman la un fișier, îi spui "uite, linia aia din utils.ts îmi dă eroare" și amândoi vedeți același ecran. Când lucrezi cu Claude, problema e că Claude nu are ochi pe ecranul tău. Vede doar ce i se trimite explicit.

Soluția evidentă e copy-paste. Selectezi codul, copiezi, lipești în panel, scrii întrebarea. Funcționează, dar e lent și fragil. Dacă te răzgândești și vrei să întrebi despre alt bloc, reîncepi.

@-mentions rezolvă asta. În loc să copiezi conținutul, referențiezi locul. Claude deschide fișierul, citește exact ce i-ai arătat, răspunde cu acel context în minte. Workflow-ul devine conversațional, nu administrativ.

Termenul "@-mention" vine din Slack, Twitter, Discord, unde pomenești pe cineva cu `@nume`. Aici pomenești un fișier sau o resursă în loc de o persoană.

---

## Ce vei putea face după

- Referențiezi un fișier întreg cu `@file` și o zonă precisă cu `@file#linii`
- Inserezi rapid mention-uri din editor folosind `Option+K` sau `Alt+K`
- Aduci output-ul unui terminal în conversație fără copy-paste
- Înțelegi ce vede Claude automat și ce trebuie să-i arăți explicit
- Ascunzi selecția când nu vrei să o trimiți (eye-slash)

---

## Modelul mental: Claude vede doar ce-i arăți

E ușor să crezi că Claude "are acces la workspace" și vede tot. Pe jumătate adevărat. Claude poate citi orice fișier din workspace atunci când îi ceri sau când decide singur că are nevoie. Diferența e că, în conversația curentă, contextul pe care îl ține în minte e limitat la:

1. **Fișierul activ în editor.** Claude Code știe ce tab ai deschis. Dacă întrebi ceva fără să specifici fișierul, presupune că e cel activ.
2. **Selecția curentă.** Dacă ai selectat câteva linii în editor, ele apar ca atașament în prompt (cu eye-slash pe ele ca să le vezi).
3. **Mențiunile explicite.** Fiecare `@ceva` pe care îl scrii aduce acea resursă în context.
4. **Istoricul conversației.** Ce ai discutat mai devreme în sesiunea curentă.

Restul workspace-ului există, dar nu e "în minte". Dacă întrebi "ce face funcția X din utils.ts?", Claude va deschide utils.ts (tool call vizibil), va citi, va răspunde. Dar dacă întrebi "crezi că e o problemă cu proiectul?", Claude nu se uită prin tot proiectul singur, ci folosește doar ce ai adus explicit.

> ⚠️ Implicația practică: dacă vrei răspuns bun, adu contextul potrivit. Nu prea mult (umple fereastra și Claude devine lent și confuz), nu prea puțin (ghicește). @-mentions sunt butonul cu care reglezi asta.

---

## Cum folosești @-mentions

### Fișier întreg: `@file`

Scrii `@` în câmpul de input. Apare o listă filtrantă cu fișierele din workspace. Începi să tastezi numele, se restrânge. Alegi cu Enter, se inserează ca chip în prompt.

> **Prompt:**
> "În @utils.ts ai vreo funcție care deja calculează TVA? Dacă da, arată-mi-o. Dacă nu, spune-mi unde ai pune-o."
>
> **Ce ar trebui să vezi:**
> Claude deschide utils.ts (tool call `Read`), răspunde pe baza conținutului real, nu după ce a ghicit după nume.

### Linii specifice: `@file#start-end`

Când fișierul e mare și vrei doar o zonă, adaugi numere de linie după `#`.

> **Prompt:**
> "Explică-mi ce face bucla din @server.py#45-62. Nu modifica nimic, vreau doar înțelegere."
>
> **Ce ar trebui să vezi:**
> Claude citește strict acele linii, răspunde narativ despre ele. Economisești context, răspunsul e mai precis.

### Dosar întreg: `@directory`

Uneori vrei să ceri ceva la nivel de dosar (ex. "găsește toate fișierele care...").

> **Prompt:**
> "Uită-te în @src/components/ și spune-mi care componente au state local și care sunt pure."
>
> **Ce ar trebui să vezi:**
> Claude listează fișierele din dosar, le citește pe rând, trage concluzia. E mai scump ca tokens decât `@file`, folosește-l când chiar ai nevoie de privire de ansamblu.

### Output terminal: `@terminal`

Aici e schimbarea mare pentru nontehnici. Dacă rulezi ceva în terminalul VS Code și primești o eroare lungă, în loc să selectezi, copiezi, lipești, poți referenția terminalul direct.

> **Prompt:**
> "Uite eroarea din @terminal. Explică-mi în română ce înseamnă și ce ar trebui să schimb."
>
> **Ce ar trebui să vezi:**
> Claude ia output-ul terminalului ca și cum l-ai fi lipit, răspunde pe el. Mention-ul funcționează și pentru terminalele pe care le-ai numit (ex. `@terminal:build`).

### Shortcut rapid: `Option+K` sau `Alt+K`

Până acum ai tastat `@` și ai ales din listă. Există un drum și mai scurt. Selectezi câteva linii direct în editor (cursor pe cod), apeși `Option+K` pe macOS sau `Alt+K` pe Windows/Linux, și selecția apare automat ca mention în panel.

> **Flux complet:**
> 1. Ești în editor, vezi o funcție problematică. Selectezi cu mouse-ul sau `Shift+săgeți`.
> 2. Apeși `Option+K` (sau `Alt+K`). Focus-ul se mută în panelul Claude, mention-ul e inserat.
> 3. Scrii întrebarea: "de ce asta ar putea arunca excepție când input-ul e gol?"
> 4. Enter.

Această combinație (selecție în editor plus shortcut) e cea mai rapidă formă de context pentru Claude. O să o folosești zeci de ori pe zi.

### Ascunde selecția: eye-slash

Uneori ai selectat ceva în editor din motive proprii (evidențiere vizuală, urmărire) și nu vrei ca Claude să vadă. Lângă câmpul de input apare un mic chip cu selecția, cu o iconiță `eye-slash`. Click pe ea și selecția e exclusă din context până deselectezi și reselectezi.

La fel, fiecare atașament (fișier, imagine, terminal) are un `X` prin care îl scoți din prompt fără să rescrii textul.

---

## Referință

### Tipuri de @-mentions

| Sintaxă | Ce aduce | Când folosești |
|---|---|---|
| `@nume-fisier.ext` | Fișierul întreg | Fișier mic sau mediu, întrebare despre tot conținutul |
| `@fisier.ext#20-45` | Doar liniile specificate | Fișier mare, întrebare punctuală pe o zonă |
| `@nume-dosar/` | Listă plus citire pe rând | Privire de ansamblu pe un dosar |
| `@terminal` | Output-ul terminalului activ | Erori, log-uri, rezultate de rulare |
| `@terminal:nume` | Output-ul unui terminal numit | Ai mai multe terminale deschise |

### Shortcut-uri esențiale

| Scop | Shortcut | Context |
|---|---|---|
| Inserează mention din selecția editor | `Option+K` (macOS) / `Alt+K` (Win/Linux) | Cursor în editor, ceva selectat |
| Deschide lista de mențiuni | `@` | În câmpul de input al panelului |
| Trimite prompt | `Enter` | În panel |
| Linie nouă în prompt | `Shift+Enter` | În panel |

### Ce vede Claude automat (fără @-mention)

- Fișierul activ în tab-ul de editor (știe numele și poate citi la cerere)
- Selecția curentă din editor (apare ca atașament)
- Istoricul conversației curente
- Orice fișier la care s-a uitat în tool call-uri anterioare din aceeași sesiune

Restul, inclusiv alte tab-uri deschise, alte fișiere din workspace, nu există pentru Claude decât dacă le mentionezi sau dacă el decide să le citească singur.

---

## Capcane frecvente

Capcane acoperite: fișier prea mare, director mentionat nepotrivit, așteptare implicită că Claude vede tot, eye-slash activ fără să știi, `Option+K` apăsat fără selecție.

### Aduci un fișier prea mare și Claude "uită"

**Simptom:** Mentionezi un fișier de 2000 de linii, ceri să-l analizeze, răspunsul e vag sau Claude se scuză că nu vede tot.
**Cauză:** Fereastra de context are limite. Un fișier mare ocupă mult din ea și lasă puțin loc pentru conversație și gândire.
**Fix:** În loc de `@fisier-mare.ts`, folosește `@fisier-mare.ts#100-200` doar pentru zona care te interesează. Sau cere mai întâi un rezumat ("dă-mi structura la nivel de funcții") și abia apoi intri în detalii.

### Mentionezi un dosar întreg și Claude o ia razna

**Simptom:** Scrii `@src/` și aștepți răspuns concis. Claude citește 40 de fișiere, răspunsul e un eseu.
**Cauză:** `@directory` face Claude să citească fiecare fișier pe rând. Când sunt multe, răspunsul devine exhaustiv și contextul se umple.
**Fix:** Rafinează întrebarea. "Uită-te doar la fișierele care folosesc hook-ul X din @src/components/" îi dă un criteriu de selecție. Sau restrânge dosarul (`@src/components/forms/` în loc de `@src/`).

### Crezi că Claude a văzut tot workspace-ul

**Simptom:** Întrebi "ai observat probleme în proiect?" și Claude răspunde general. Te aștepți să fi citit totul.
**Cauză:** Claude nu scanează proactiv. În conversația curentă, el "știe" doar ce ai mentionat sau ce s-a deschis prin tool call-uri anterioare.
**Fix:** Ori îi ceri explicit să exploreze ("listează fișierele din @src/ și identifică pe cele fără teste"), ori folosești CLAUDE.md (vezi 1.4) ca să pui în memorie persistentă ce ar trebui să știe despre proiect.

### Eye-slash activ și Claude pare că nu vede selecția

**Simptom:** Ai selectat niște cod, întrebi Claude despre el, răspunde ca și cum nu l-ar fi văzut.
**Cauză:** Iconița eye-slash de lângă selecție e activată, selecția e ascunsă din context.
**Fix:** Click pe iconiță să o dezactivezi. Sau folosește `Option+K` / `Alt+K` care forțează inserarea ca mention vizibil.

### Apeși `Option+K` fără selecție activă

**Simptom:** Scurtătura nu face nimic. Sau mutați focus-ul în panel fără să se întâmple ceva util.
**Cauză:** Shortcut-ul funcționează cu o selecție activă în editor. Fără text selectat, nu are ce să insereze.
**Fix:** Selectează întâi (`Shift+săgeți` sau mouse), apoi `Option+K`. Pentru prompt gol fără context, click pur și simplu în panel și scrie `@` să alegi manual.

---

## Următorul pas

→ [1.4 Workflow de editare](workflow-editare.md). Până acum i-ai arătat lui Claude contextul. Acum vezi cum controlezi ce propune și când acceptă. Diff side-by-side, plan mode prin `/plan`, auto-accept cu `Shift+Tab`, conversation history.

---

## Surse

- [Use Claude Code in VS Code (ide-integrations)](https://docs.anthropic.com/en/docs/claude-code/ide-integrations)
- [Common workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
- [Interactive mode](https://docs.anthropic.com/en/docs/claude-code/interactive-mode)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
