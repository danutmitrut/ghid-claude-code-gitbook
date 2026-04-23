*Partea I, Bază > 1.1 Instalare*

# 1.1 Instalare

**Cum instalezi de la zero componentele necesare pentru Claude Code ca extensie VS Code, cu ramuri clare pentru macOS și Windows, plus verificarea că totul funcționează înainte să treci mai departe.**

| Meta | |
|---|---|
| Timp de setup (macOS) | 15-20 minute |
| Timp de setup (Windows) | 30-45 minute |
| Timp de învățare | Setup one-time, nu necesită fluență |
| Valoare pentru solopreneur | Necesară. Fără ea, restul ghidului nu funcționează |
| Dependențe | Acces administrator pe calculator (pentru instalare), conexiune internet, cont Anthropic sau willingness să îl creezi |

**Acoperă:** instalare VS Code pe macOS și Windows, instalare Node.js 18+ pe ambele platforme, instalare Git for Windows cu Git Bash (doar Windows) și setarea ca default terminal profile în VS Code, instalare extensia Claude Code din marketplace-ul VS Code, signin cu cont Anthropic prin OAuth, primul prompt de verificare, capcane specifice per platformă.
**Nu acoperă:** configurare avansată (vezi straturile următoare), instalare CLI-ul `claude` standalone (vezi 3.3 doar dacă îți trebuie), WSL ca alternativă la Git for Windows (menționată scurt la final pentru context), instalare pe Linux (similar cu macOS, te poți orienta după ramura aceea).

---

## TL;DR

Pentru Claude Code ca extensie VS Code ai nevoie de trei lucruri pe orice platformă: VS Code, Node.js 18 sau mai nou, și extensia Claude Code însăși. Pe Windows se adaugă un al patrulea: Git for Windows, care aduce Git Bash ca shell compatibil cu ce așteaptă Claude să găsească pe calculator. Ordinea normală e: instalezi prerequisitele pe rând, instalezi extensia din marketplace-ul VS Code, faci signin cu contul Anthropic, trimiți primul prompt ca să confirmi că totul răspunde. Pe macOS tot procesul durează sub 20 de minute. Pe Windows, aproape de 45 de minute, majoritatea din cauza setării Git Bash ca shell default.

---

## De ce contează

Stratul ăsta e fundația. Fără el, restul ghidului e literatură. Dacă instalarea eșuează în vreun punct, nu doar că nu ai Claude Code funcțional, dar pierzi și încrederea că merită să mergi mai departe. Pentru un solopreneur nontehnic care încearcă Claude Code prima dată, experiența primei instalări setează așteptarea pentru tot ce urmează.

Există două zone de risc la instalare. Prima, prerequisitele ascunse. Windows-ul nu are nativ un shell POSIX (genul de shell pe care Linux și macOS îl au din fabrică), iar Claude Code folosește intern un astfel de shell pentru anumite operațiuni. Soluția oficială pe Windows e Git for Windows, care instalează și Git Bash ca shell compatibil. Mulți cursanți nu știu că au nevoie de el, nu îl instalează, apoi se întreabă de ce extensia aruncă erori ciudate.

A doua, ordinea. Dacă instalezi extensia înainte de Node.js sau înainte de Git for Windows pe Windows, extensia se instalează dar nu funcționează complet. Restart-uri, reinstalări, confuzie. Ordinea corectă e prerequisitele întâi, extensia la final.

Acest strat îți dă ordinea corectă pe ambele platforme, cu explicații scurte la fiecare pas, ca să eviți amândouă zonele de risc.

---

## Ce vei putea face după

- Ai VS Code instalat și configurat cu shell-ul potrivit pentru platforma ta
- Ai Node.js 18 sau mai nou disponibil global (verificabil din terminal)
- Pe Windows, ai Git for Windows cu Git Bash funcțional și setat ca default în VS Code
- Ai extensia Claude Code instalată și logată cu contul Anthropic
- Ai trimis primul prompt și ai primit răspuns
- Știi ce să verifici dacă ceva nu merge din primul foc

---

## Modelul mental: trei componente, plus una pe Windows

VS Code e editorul în care scrii și lucrezi. Gândește-te la el ca la Microsoft Word pentru text, doar că adaptat pentru proiecte digitale (cod, scripturi, configurări, conținut Markdown). E programul pe care îl deschizi și în care trăiești când lucrezi cu Claude Code.

Node.js e un mediu de rulare pentru anumite programe tehnice. Nu îl deschizi direct și nu interacționezi cu el conștient. E motorul din spate pe care se bazează instalarea extensiei și unele componente ulterioare (în special MCP servers, vezi 2.2). Versiunea trebuie să fie 18 sau mai nouă.

Extensia Claude Code e interfața prin care vorbești cu Claude. Se instalează ca orice altă extensie VS Code, din marketplace-ul lui. Odată instalată, apare ca panou separat în VS Code unde scrii prompturi și vezi răspunsuri.

Pe Windows se adaugă Git for Windows. E un pachet care aduce comanda `git` (sistemul de versionare cod) plus Git Bash (un shell POSIX, adică un terminal care se comportă ca cel de pe Linux și macOS). Claude Code folosește intern un shell POSIX pentru anumite operațiuni. Pe Mac și Linux există din fabrică. Pe Windows, trebuie adus separat prin Git for Windows.

Ordinea de instalare și motivul fiecăreia:

1. VS Code primul, pentru că acolo vor trăi celelalte
2. Node.js al doilea, pentru că extensia și componente ulterioare depind de el
3. Pe Windows, Git for Windows al treilea, pentru că fără el extensia va avea probleme la anumite operațiuni
4. Extensia Claude Code la final, când toate prerequisitele sunt gata
5. Signin și primul prompt ca verificare

---

## Setup, pas cu pas pe macOS

### Pasul 1: Instalare VS Code

Deschide în browser [code.visualstudio.com](https://code.visualstudio.com) și descarcă versiunea pentru macOS. Primești un fișier `.zip` care la dublu-click se dezarhivează într-o aplicație `Visual Studio Code.app`.

Muți aplicația în folderul Applications (trage și plasează). Deschizi Launchpad sau folderul Applications, dai click pe iconița VS Code. Prima deschidere poate dura câteva secunde și îți va cere permisiune să deschizi o aplicație descărcată de pe internet. Dai Open.

Când vezi fereastra VS Code cu pagina de welcome, ești gata.

### Pasul 2: Instalare Node.js

Deschide în browser [nodejs.org](https://nodejs.org) și descarcă versiunea marcată LTS (Long Term Support). La momentul scrierii ghidului, asta e minim 18, cu versiuni mai noi disponibile. Oricare din LTS e bună.

Rulezi instalatorul `.pkg` descărcat, dai Next la pașii standard, accepți termenii, lași opțiunile default. Instalarea durează 1-2 minute.

Verifici că s-a instalat corect. Deschide Terminal (Cmd+Space, tastezi "Terminal", Enter). În terminal scrii:

```
node --version
```

Ar trebui să vezi ceva de tipul `v20.10.0` sau similar. Dacă vezi un număr care începe cu `v18`, `v20`, `v22` sau mai mare, ești bine.

### Pasul 3: Instalare extensia Claude Code

Revii în VS Code. Click pe iconița Extensions din sidebar-ul din stânga (patru pătrățele, sau `Cmd+Shift+X`). În căsuța de căutare tastezi "Claude Code". Primul rezultat, publicat de Anthropic, e cel corect. Click pe Install.

Instalarea durează câteva secunde. După ea, ar trebui să apară o iconiță cu scânteie (Spark) în sidebar. Dacă nu apare imediat, restartează VS Code (`Cmd+Shift+P` → "Developer: Reload Window").

### Pasul 4: Signin și primul prompt

Click pe iconița Spark. Se deschide panoul Claude. Primul ecran e pentru signin. Dai click pe "Sign in", browserul se deschide cu fluxul OAuth la Anthropic.

Ai două opțiuni: te loghezi cu contul Anthropic existent (dacă ai folosit Claude chat înainte) sau creezi unul nou. Flow-ul e standard, email plus parolă, eventual verificare prin email.

După ce confirmi, browserul te redirecționează înapoi la VS Code. Panoul Claude arată acum câmpul pentru primul tău prompt.

Pentru verificare, deschide un folder oarecare în VS Code (File > Open Folder, alegi un folder din `Documents` sau oriunde). Apoi scrie în panou:

> **Prompt:**
> "Listează fișierele din acest folder și spune-mi în câteva cuvinte ce conține."
>
> **Ce ar trebui să vezi:**
> Claude face un tool call (Read sau LS, vizibil ca bloc expandabil) și îți dă o listă narativă cu ce e acolo. Dacă primești răspuns, instalarea e completă.

---

## Setup, pas cu pas pe Windows

### Pasul 1: Instalare VS Code

Deschide în browser [code.visualstudio.com](https://code.visualstudio.com) și descarcă versiunea pentru Windows. Primești un fișier `.exe` (User Installer).

Rulezi instalatorul. La pagina "Select Additional Tasks", bifezi opțiunile:
- "Add 'Open with Code' action to Windows Explorer file context menu"
- "Add 'Open with Code' action to Windows Explorer directory context menu"
- "Add to PATH" (foarte important, bifat default)

Dai Next până la Install, aștepți finalizarea. Deschizi VS Code la sfârșit.

### Pasul 2: Instalare Node.js

Deschide în browser [nodejs.org](https://nodejs.org) și descarcă versiunea LTS pentru Windows (`.msi` pentru instalator standard).

Rulezi instalatorul. La pagina cu custom setup, lași opțiunile default. La pagina cu "Tools for Native Modules", nu bifezi nimic în plus (nu ai nevoie pentru Claude Code).

La final, restartează calculatorul. E important, pentru că Node.js adaugă path-uri în mediul de sistem care nu sunt vizibile până la restart.

După restart, verifici. Deschide PowerShell (Start > tastezi "PowerShell" > Enter) sau CMD. În fereastră scrii:

```
node --version
```

Ar trebui să vezi `v20.10.0` sau similar, orice cu `v18+`. Dacă vezi eroare "not recognized", Node.js nu e în PATH. Restart încă o dată, sau reinstalare cu atenție la opțiunea PATH.

### Pasul 3: Instalare Git for Windows

Deschide în browser [git-scm.com/download/win](https://git-scm.com/download/win). Descarcă cea mai nouă versiune standalone.

Rulezi instalatorul. E un instalator cu multe pagini de opțiuni, cele mai multe pot rămâne pe default. Trei puncte de atenție:

**"Select Components":** lasă default. Git Bash e inclus automat.

**"Choosing the default editor used by Git":** alege "Use Visual Studio Code as Git's default editor" (apare în lista dropdown).

**"Adjusting your PATH environment":** alege "Git from the command line and also from 3rd-party software" (opțiunea din mijloc, recomandată).

Dai Next la restul, lași opțiunile default. Instalarea durează 2-3 minute.

Verifici după instalare. Deschide Git Bash din meniul Start (Start > tastezi "Git Bash" > Enter). Se deschide o fereastră neagră cu prompt. Scrii:

```
git --version
bash --version
```

Ar trebui să vezi versiuni pentru amândouă. Închizi Git Bash, mergi mai departe.

### Pasul 4: Setare Git Bash ca default terminal profile în VS Code

Acest pas e specific Windows-ului și e cel mai important, pentru că asigură că extensia Claude Code găsește shell-ul compatibil.

Deschizi VS Code. Apeși `Ctrl+Shift+P` pentru a deschide Command Palette. Tastezi "Terminal: Select Default Profile" și apeși Enter.

Apare o listă cu shell-urile disponibile. Alegi "Git Bash".

Pentru verificare, deschizi un terminal în VS Code (`Ctrl+backtick`, adică `Ctrl+` sau prin meniu `View > Terminal`). Jos în fereastra de terminal ar trebui să apară un prompt care arată altfel decât PowerShell (de obicei include `MINGW64` sau un nume de user urmat de `@` și computer name).

Dacă vezi `PS C:\...` cu accent albastru, e PowerShell. Încearcă din nou pasul cu Select Default Profile și închide apoi redeschide terminalul.

### Pasul 5: Instalare extensia Claude Code

Click pe iconița Extensions din sidebar-ul stânga al VS Code (`Ctrl+Shift+X`). În căsuța de căutare tastezi "Claude Code". Primul rezultat, publicat de Anthropic, e cel corect. Click pe Install.

Instalarea durează câteva secunde. După ea, ar trebui să apară o iconiță cu scânteie (Spark) în sidebar. Dacă nu apare imediat, reload window (`Ctrl+Shift+P` → "Developer: Reload Window").

### Pasul 6: Signin și primul prompt

Click pe iconița Spark. Se deschide panoul Claude. Primul ecran e pentru signin. Dai click pe "Sign in", browserul se deschide cu fluxul OAuth la Anthropic.

Te loghezi cu contul Anthropic existent sau creezi unul nou. După confirmare, browserul te redirecționează înapoi la VS Code. Panoul Claude arată acum câmpul pentru primul prompt.

Deschide un folder oarecare în VS Code (`File > Open Folder`). Apoi în panou:

> **Prompt:**
> "Listează fișierele din acest folder și spune-mi în câteva cuvinte ce conține."
>
> **Ce ar trebui să vezi:**
> Claude face un tool call și îți dă o listă narativă. Dacă primești răspuns, instalarea e completă.

---

## Referință

### Prerequisite per platformă

| Component | macOS | Windows | De unde |
|---|---|---|---|
| VS Code | Obligatoriu | Obligatoriu | [code.visualstudio.com](https://code.visualstudio.com) |
| Node.js 18+ | Obligatoriu | Obligatoriu | [nodejs.org](https://nodejs.org) |
| Git for Windows | Nu | Obligatoriu | [git-scm.com/download/win](https://git-scm.com/download/win) |
| Git Bash ca default profile în VS Code | Nu | Obligatoriu | Setat din VS Code (`Ctrl+Shift+P`) |
| Extensia Claude Code | Obligatoriu | Obligatoriu | Din marketplace-ul VS Code |
| Cont Anthropic | Obligatoriu | Obligatoriu | Creat la signin |

### Timpi estimativi per pas

| Pas | macOS | Windows |
|---|---|---|
| VS Code | 3 min | 3 min |
| Node.js | 2 min | 5 min (cu restart) |
| Git for Windows | Skip | 5 min |
| Setare Git Bash default | Skip | 2 min |
| Extensia Claude Code | 2 min | 2 min |
| Signin și primul prompt | 5 min | 5 min |
| **Total realist** | 15-20 min | 30-45 min |

### Cum verifici că totul e în ordine

Deschide terminalul (Terminal pe Mac, Git Bash sau terminalul integrat VS Code pe Windows). Rulează pe rând:

```
node --version
```

Răspuns așteptat: `v18.x.x` sau mai mare.

Pe Windows, în plus:

```
git --version
bash --version
```

Ambele ar trebui să întoarcă versiuni fără eroare.

În VS Code, deschide panoul Claude și verifici că ești logat (ar trebui să vezi numele tău sau emailul sus în panou).

---

## Capcane frecvente

Capcane acoperite: Node.js negăsit după instalare pe Windows, Git Bash negăsit de Claude Code pe Windows, signin eșuează prin firewall corporate, extensia apare dar panoul nu se deschide, versiune Node sub 18, conflict între instalări multiple Node.js pe Mac.

### Windows: `node` negăsit după instalare

**Simptom:** Ai instalat Node.js, deschizi terminal, scrii `node --version`, primești "not recognized as an internal or external command".
**Cauză:** Instalatorul nu a adăugat Node la PATH, sau modificarea PATH nu s-a propagat fără restart.
**Fix:** Restart calculator. Dacă după restart tot nu merge, reinstalează Node.js și bifează explicit "Add to PATH" la pagina cu opțiuni. Ca ultimă rezolvare, adaugă manual `C:\Program Files\nodejs\` în variabila PATH din Environment Variables.

### Windows: Claude Code aruncă erori despre shell pe operațiuni normale

**Simptom:** Extensia e instalată, ai făcut signin, dar la anumite operațiuni (mai ales Bash tool calls) primești erori ciudate despre shell sau comandă negăsită.
**Cauză:** Claude Code nu găsește Git Bash automat, sau ai sărit pasul de setare Git Bash ca default terminal profile.
**Fix:** Verifică că Git for Windows e instalat (`git --version` trebuie să răspundă în orice terminal). Apoi `Ctrl+Shift+P` → "Terminal: Select Default Profile" → Git Bash. Restart panou Claude. Dacă nici atunci nu merge, poți seta calea manual în `settings.json` al VS Code, dar de obicei default profile rezolvă.

### Ambele platforme: signin eșuează prin firewall corporate

**Simptom:** Click pe Sign in, browserul se deschide dar fluxul OAuth nu se completează, sau returnează eroare.
**Cauză:** Firewall-ul de la birou blochează domeniile Anthropic, sau VPN-ul modifică DNS-ul în mod incompatibil.
**Fix:** Testează pe o conexiune personală (hotspot telefon, rețea acasă). Dacă merge acolo, problema e în rețeaua corporate și trebuie contactat IT-ul să adauge `anthropic.com` și subdomeniile pe whitelist. Dezactivează temporar VPN-ul ca test.

### Ambele platforme: extensia apare dar panoul nu se deschide

**Simptom:** Ai instalat extensia, vezi iconița Spark în sidebar, click pe ea nu deschide nimic sau deschide un panou gol.
**Cauză:** Conflict cu alt layout VS Code, sau extensia nu s-a inițializat complet.
**Fix:** În ordine. Reload window (`Ctrl+Shift+P` / `Cmd+Shift+P` → "Developer: Reload Window"). Dacă nu merge, "View: Reset View Locations" din Command Palette. Dacă tot nu merge, dezinstalare și reinstalare extensie. Ultima opțiune, restart VS Code complet.

### Ambele platforme: versiunea Node e sub 18

**Simptom:** `node --version` întoarce `v14.x.x` sau `v16.x.x`. Extensia se plânge la operațiuni.
**Cauză:** Ai Node instalat din surse vechi (un instalator mai vechi, un package manager care nu s-a actualizat).
**Fix:** Dezinstalează versiunea veche, instalează de la [nodejs.org](https://nodejs.org) versiunea LTS curentă. Pe Mac cu Homebrew: `brew upgrade node`. Pe Windows, reinstalator standard peste cel vechi.

### macOS: conflict între instalări multiple Node.js

**Simptom:** `node --version` răspunde o versiune, dar `which node` arată un path suspect. Sau o aplicație vede o versiune, alta vede alta.
**Cauză:** Ai instalat Node de mai multe ori din surse diferite (direct de pe nodejs.org, via Homebrew, via nvm). Fiecare păstrează propria copie și PATH-ul alege prima găsită.
**Fix:** Decide care instalare vrei să rămână. Recomandare: Homebrew dacă ești confortabil cu el (`brew install node`), altfel direct de pe nodejs.org. Dezinstalează celelalte. Verifică cu `which node` că path-ul returnat e cel așteptat.

---

## Alternativă pe Windows: WSL

Pentru cursanții mai avansați, documentația oficială Anthropic menționează WSL (Windows Subsystem for Linux) ca alternativă la Git for Windows. WSL e un Linux complet rulat în Windows, care aduce un shell POSIX real.

Recomandarea mea: nu începe cu WSL dacă ești nontehnic. Git for Windows e suficient pentru tot ce face acest ghid și e mai simplu de setat. WSL devine relevant doar când ajungi la operațiuni avansate (3.3 SDK, deployment) și ai deja confort cu Linux.

Dacă totuși vrei să încerci WSL, [documentația Microsoft](https://learn.microsoft.com/windows/wsl) e punctul de start. După ce ai WSL 2 instalat și o distribuție Linux activă (Ubuntu e default), Claude Code se comportă ca pe Linux nativ.

---

## Următorul pas

→ [1.2 Primul contact](primul-contact.md). Ai Claude Code instalat și funcțional. Pasul următor e să te familiarizezi cu interfața (Spark icon, panel, status bar), să trimiți primul prompt complet, să accepți primul diff și să înțelegi ce face Claude Code diferit de alte extensii AI populare.

---

## Surse

- [Set up Claude Code](https://docs.claude.com/en/docs/claude-code/setup)
- [Use Claude Code in VS Code](https://docs.claude.com/en/docs/claude-code/vs-code)
- [Quickstart](https://docs.claude.com/en/docs/claude-code/quickstart)
- [Troubleshooting](https://docs.claude.com/en/docs/claude-code/troubleshooting)

*Ultima revizuire: 2026-04-23. Verificat pe docs oficiale Anthropic, trimestrul 2 2026.*
