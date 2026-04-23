*Partea I, Bază > 1.4 Workflow de editare*

# 1.4 Workflow de editare

**Cum controlezi ritmul și riscul: diff side-by-side, plan mode prin `/plan`, auto-accept cu `Shift+Tab`, istoric de conversații pe care îl deschizi când ai nevoie.**

| Meta | |
|---|---|
| Timp de setup | 15 minute |
| Timp de învățare până la fluență | 2-3 ore de practică |
| Valoare pentru solopreneur | Mare, aici hotărăști cine face decizia |
| Dependențe | 1.1, 1.2 și 1.3 complete |

**Acoperă:** diff side-by-side cu cele trei opțiuni (accept, reject, instruiește), plan mode activat prin comanda `/plan` sau click pe mode indicator, ciclul de moduri prin `Shift+Tab` (Normal, Auto-Accept, Plan), celelalte moduri de permisiuni (`dontAsk`, `bypassPermissions`), dropdown-ul de conversation history, reluare sesiune prin slash command `/resume`.
**Nu acoperă:** memorie persistentă între sesiuni prin CLAUDE.md (vezi 1.5), delegare la subagenți cu context izolat (vezi 2.1).

---

## TL;DR

Claude Code îți dă trei moduri de a lucra: supraveghere strictă (default, confirmi fiecare pas), auto-pilot parțial (acceptă singur edit-urile de fișiere, cere confirmare pentru shell) și doar gândire fără execuție (plan mode, produce un plan scris pe care îl citești înainte să ruleze ceva). Treci între ele cu `Shift+Tab`. Fiecare conversație se salvează automat, o poți relua din dropdown-ul panelului sau din slash command-ul `/resume`.

---

## De ce contează

La primul contact cu Claude Code, ai o frică discretă. "Dacă îi cer ceva și strică fișierul? Dacă rulează o comandă pe care n-o înțeleg? Dacă mă pune în mocirlă și nu știu să ies?"

Frica e justificată. Claude rulează comenzi, modifică fișiere, poate face mult. Soluția pe care o aplică majoritatea oamenilor nontehnici e să stea în tensiune maximă permanent: citesc fiecare rând, aprobă fiecare pas, ezită, revin. Devine obositor în 30 de minute.

Claude Code oferă alt drum. Împarte riscul în niveluri și te lasă să alegi nivelul potrivit pentru task-ul curent. Sarcini sensibile (ai o bază de date, scrii un email, atingi un fișier de producție) cer supraveghere strictă. Sarcini rutiniere (refactoring minor pe fișiere temporare) pot merge mai rapid. Explorare, unde nu vrei nicio modificare, merge în plan mode.

Ce înveți în stratul ăsta e să nu stai mereu în același mod. Schimbi modul după task, nu invers.

---

## Ce vei putea face după

- Înțelegi ce vezi într-un diff side-by-side și îl folosești în loc să-l treci cu vederea
- Activezi plan mode când vrei explorare fără risc de modificare
- Pornești auto-accept când task-ul permite, oprești când devine delicat
- Reiei o conversație veche prin dropdown sau `/resume`
- Știi când deschizi o conversație nouă în alt tab pentru task paralel

---

## Modelul mental: trei moduri, un singur buton

În partea de jos a panelului Claude e un indicator care îți arată în ce mod ești. Cele trei moduri principale, în ordinea de strictețe, sunt:

1. **Normal.** Default. Claude propune diff-uri, tu le accepți sau respingi. Pentru comenzi shell (bash, npm, git) cere confirmare. Pentru citire, nu cere.
2. **Auto-Accept Edits.** Claude aplică singur edit-urile la fișiere. Comenzile shell încă cer confirmare. Indicator vizual: `⏵⏵ accept edits on`.
3. **Plan Mode.** Claude nu modifică nimic. Produce un plan scris (document Markdown) pe care îl citești, la care adaugi comentarii, apoi dai semnal să execute.

Butonul care ciclează între ele este `Shift+Tab`. O dată din Normal te duce în Auto-Accept. Încă o dată te duce în Plan Mode. A treia te aduce înapoi în Normal.

Pe lângă aceste trei, există și moduri avansate pe care le vezi mai rar:

- **dontAsk:** refuză tool-uri care n-au fost pre-aprobate. Util când partajezi Claude cu o echipă și vrei garanții.
- **bypassPermissions:** aprobă automat tot. Periculos, folosit de sdk-uri în condiții controlate.

În VS Code, modul inițial se setează în `settings.json` prin cheia `claudeCode.initialPermissionMode`.

> ⚠️ Implicația practică: începe în Normal. Când observi că răspunzi mecanic "accept" de 10 ori consecutiv pe același tip de edit, e semnal să treci pe Auto-Accept. Când vrei să explorezi un proiect nou fără risc, treci pe Plan.

---

## Cum folosești

### Diff side-by-side

Când Claude vrea să modifice un fișier, deschide un diff side-by-side în editor. Stânga e versiunea veche, dreapta e propunerea. Liniile noi sunt verzi, cele șterse sunt roșii. Sus apar trei opțiuni.

**Accept.** Aplici diff-ul, fișierul e actualizat pe disc.

**Reject.** Nu aplici, propunerea dispare.

**A treia opțiune, spune-i lui Claude ce să facă altfel.** În loc să respingi și să începi de la zero, scrii un mesaj nou în panel care rafinează propunerea. "Mută funcția în alt fișier." "Fără comentarii auto-generate." "Respectă convenția de naming pe care o folosesc în rest." Claude rescrie diff-ul cu feedback-ul tău, îl propune din nou.

> **Capcană frecventă:** Reject reflex. Te grăbești, nu-ți place un detaliu, apeși reject, repetă ciclul. A treia opțiune e aproape mereu mai rapidă.

### Plan mode prin `/plan` sau click pe indicator

Când vrei să înțelegi ce ar face Claude fără să se întâmple nimic, comanzi plan mode în două feluri:

- Din panel, click pe mode indicator (partea de jos) până apare "Plan Mode".
- Din prompt, scrii comanda `/plan` urmată de sarcină.

> **Notă scurtă despre slash commands.** Tot ce începe cu `/` în câmpul de input al panelului e un slash command. `/plan`, `/resume`, `/compact`, `/clear` sunt built-in. Le tastezi direct, se execută. În 1.4 vezi cum îți faci slash commands proprii pentru task-uri repetitive.

> **Prompt:**
> "/plan Refactorizează funcția calcTVA din @utils.ts să accepte și un parametru pentru țară (RO implicit, dar să poată fi UK, DE). Nu modifica acum, vreau doar plan."
>
> **Ce ar trebui să vezi:**
> Claude deschide un document Markdown cu planul detaliat: ce fișiere ar atinge, ce schimbări, ce teste, în ce ordine. Documentul e editabil, poți adăuga comentarii inline ("aici aș prefera enum în loc de string"). După ce dai acordul, Claude execută planul.

Plan mode e util în trei situații: nu cunoști bine proiectul și vrei să vezi cum ar aborda Claude; task-ul e complex cu multe atingeri și vrei vizibilitate înainte; lucrezi pe cod sensibil unde greșelile dor.

### Auto-Accept cu `Shift+Tab`

Sunt task-uri unde confirmi la fiecare diff și după a cincea oară îți dai seama că ai spus "accept" din inerție. Atunci e momentul pentru Auto-Accept.

Apeși `Shift+Tab` o dată. Indicator-ul se schimbă în `⏵⏵ accept edits on`. Claude aplică diff-urile de fișiere fără să întrebe. Pentru comenzi shell (instalare pachete, rulare teste, `git commit`) încă întreabă.

Când task-ul se termină sau devine sensibil, `Shift+Tab` din nou te duce în Plan Mode (care e "frână dublă"). Încă o dată, înapoi la Normal.

> ⚠️ Auto-Accept nu înseamnă "nu mai verific". Diff-urile aplicate sunt vizibile în istoric. La finalul sesiunii, un obicei util e să deschizi Source Control view din sidebar-ul VS Code (iconița cu ramificații) ca să vezi toate fișierele modificate. Dacă proiectul tău folosește Git, acolo vezi diff-ul cumulat.

### Conversation history

Toate conversațiile se salvează automat pe disc, per director de proiect. Nu trebuie să faci nimic să le păstrezi.

Ai două porți de intrare pentru reluare.

**Dropdown în panel.** Sus în panelul Claude e un selector cu conversațiile anterioare. Click pe una o deschide unde era. E cea mai vizuală cale, bună când vrei să răsfoiești și să recunoști sesiunea după titlu sau preview.

**Slash command `/resume`.** În câmpul de input scrii `/resume` și apasă Enter. Se deschide un picker interactiv cu sesiunile din proiectul curent, cu preview, căutare și opțiunea de redenumire. Dacă deja știi numele exact al sesiunii, scrii `/resume nume-sesiune` și sari direct la ea. Din interiorul unei sesiuni active, `/resume` e și modul prin care comuți pe altă conversație fără să închizi panelul.

**Conversații paralele.** "Open in New Tab" sau "Open in New Window" din Command Palette (`⌘+Shift+P` sau `Ctrl+Shift+P`) pornesc o conversație separată. Utilă când ai un task lung și vrei să întrebi ceva scurt fără să amesteci contextele.

> **Flux tipic:** Vineri lucrezi la un refactor, oprești. Luni dimineață deschizi VS Code, tastezi `/resume` în panel, alegi conversația de vineri din picker. Claude are tot contextul, continui de unde ai rămas.

---

## Referință

### Modurile de permisiuni

| Mod | Cum activezi | Ce aprobi automat | Ce cere confirmare |
|---|---|---|---|
| Normal | Default, sau `Shift+Tab` până ajungi aici | Citire fișiere | Edit-uri, comenzi shell |
| Auto-Accept | `Shift+Tab` o dată din Normal | Edit-uri de fișiere, citire | Comenzi shell |
| Plan Mode | `/plan` sau `Shift+Tab` de două ori | Nimic, doar produce plan | Totul (se execută după acord) |
| dontAsk | Setat în config | Doar tool-uri pre-aprobate | Restul refuzate |
| bypassPermissions | Setat în config, folosit în SDK | Tot | Nimic |

### Shortcut-uri și comenzi

| Scop | Cum | Context |
|---|---|---|
| Ciclează prin moduri | `Shift+Tab` | În panel |
| Intră în plan mode | `/plan <sarcină>` | Câmpul de input |
| Accept diff | Click "Accept" sau Enter pe diff focusat | Diff vizibil |
| Reject diff | Click "Reject" sau Escape | Diff vizibil |
| Reia conversație, vizual | Dropdown top panel | VS Code |
| Reia conversație, prin comandă | `/resume` sau `/resume nume-sesiune` | Câmpul de input |
| Conversație paralelă | Command Palette, "Open in New Tab" | VS Code |

### Ce se salvează în conversation history

- Toate mesajele tale și ale lui Claude
- Tool call-urile executate și output-urile lor
- Fișierele referențiate prin @-mention
- Diff-urile acceptate sau respinse, cu etichetă
- Nu se salvează selecțiile temporare din editor (efemere)

---

## Capcane frecvente

Capcane acoperite: reject reflex, auto-accept uitat deschis, plan mode confundat cu mod normal, conversații multiple amestecate, pierdere context după `/clear` accidental.

### Reject reflex în loc de rafinare

**Simptom:** Claude propune un diff, nu-ți place un detaliu, respingi fără comentariu. Reformulezi promptul, Claude propune aproape același lucru, respingi iar. Ciclu frustrant.
**Cauză:** Reject înseamnă "uită-l complet". Nu transmite ce anume te-a deranjat. Claude pornește de la zero, cu aceleași premise.
**Fix:** Folosește a treia opțiune. Scrie în panel ce te deranjează cu propunerea vizibilă ("pune-l în `helpers/tva.ts`, nu direct în utils", "fără semnătură exportată implicit"). Claude rescrie cu feedback punctual, mult mai rapid.

### Auto-Accept uitat activ

**Simptom:** Termini un refactor simplu în Auto-Accept, treci la un task sensibil, uiți să comuți. Claude aplică ceva ce n-ai vrut să aplice, îți iei un commit de regretat.
**Cauză:** Indicator-ul `⏵⏵ accept edits on` e discret. E ușor să-l ratezi.
**Fix:** După orice task în Auto-Accept, obișnuiește-te să apeși `Shift+Tab` până ajungi înapoi în Normal. Plus, o privire prin Source Control view din sidebar înainte să salvezi totul, să vezi ce s-a schimbat efectiv.

### Plan mode confundat cu mod normal

**Simptom:** Ești în plan mode, dai prompt, aștepți să se întâmple ceva. Claude produce un document cu plan, tu te întrebi "de ce n-a rulat nimic?".
**Cauză:** Plan mode nu execută. Produce plan ca document, așteaptă acordul tău să treacă la execuție.
**Fix:** Verifică indicator-ul înainte. Dacă vezi "Plan Mode", știi că rezultatul e un plan, nu o acțiune. Pentru execuție, citești planul, dai ok explicit, Claude trece la Normal și rulează.

### Conversații multiple amestecate

**Simptom:** Lucrezi la două task-uri diferite în aceeași conversație. Claude se încurcă, face presupuneri greșite între ele.
**Cauză:** Fiecare conversație are un fir. Dacă îi arunci două fire neconectate, încearcă să le lege (prost).
**Fix:** Pentru task-uri paralele, "Open in New Tab" din Command Palette. Fiecare conversație separată, fiecare cu contextul ei. Sau, dacă un task e terminat, `/clear` pornește sesiune nouă curată (dar pierzi istoricul sesiunii curente, nu al tuturor).

### `/clear` accidental și panică

**Simptom:** Rulezi `/clear`, panelul se curăță, crezi că ai pierdut totul.
**Cauză:** `/clear` resetează sesiunea curentă, dar nu șterge istoricul salvat. Conversațiile anterioare sunt în dropdown sau accesibile prin `/resume`.
**Fix:** Dropdown-ul top panel sau slash command `/resume`. Conversația "ștearsă" e încă acolo, o redeschizi.

---

## Următorul pas

→ [1.5 Memorie de proiect](memorie-proiect.md). Acum știi cum decizi în sesiunea curentă. Pasul următor e cum păstrezi context între sesiuni fără să reexplici proiectul de fiecare dată. CLAUDE.md la nivel de user și de proiect, ierarhia de încărcare, `/compact` și `/clear`, slash commands custom.

---

## Surse

- [Interactive mode](https://docs.anthropic.com/en/docs/claude-code/interactive-mode)
- [Configure permissions](https://docs.anthropic.com/en/docs/claude-code/sdk/sdk-permissions)
- [Use Claude Code in VS Code (ide-integrations)](https://docs.anthropic.com/en/docs/claude-code/ide-integrations)
- [Common workflows](https://docs.claude.com/en/docs/claude-code/common-workflows)

*Ultima revizuire: 2026-04-17. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
