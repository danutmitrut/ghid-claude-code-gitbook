*Ghid Claude Code > Anexe > Glosar*

# Glosar

**Termeni tehnici folosiți în ghid, cu definiție scurtă. Ordine alfabetică.**

---

**Agent SDK.** Biblioteca oficială (Python și TypeScript) prin care poți folosi Claude programatic în aplicațiile tale. Aceleași tool-uri și aceeași buclă de gândire ca în Claude Code, dar accesibile din cod. Relevant pentru 3.3.

**Auto-Accept.** Mod de permisiuni în care Claude aplică edit-urile fără să mai ceară confirmare. Util pentru task-uri rutiniere, periculos pentru cele sensibile. Cycling prin `Shift+Tab` în panou.

**bypassPermissions.** Mod extrem de permisiuni prin care Claude rulează orice tool fără să întrebe. De folosit doar în medii izolate (container, worktree de test) pentru că poate face modificări distrugătoare.

**CLAUDE.md.** Fișier text cu instrucțiuni permanente pentru Claude. Există la trei niveluri: personal (`~/.claude/CLAUDE.md`, valabil peste toate proiectele), proiect (`CLAUDE.md` în rădăcina proiectului, partajabil cu echipa), pe subdosar (încărcat doar când Claude intră în dosarul respectiv). Relevant pentru 1.4.

**Claude Code.** Asistentul AI agentic de la Anthropic pentru programatori și workflow-uri tehnice. În ghid, se referă exclusiv la extensia pentru VS Code.

**CLI.** Command Line Interface, adică rularea lui `claude` direct din terminal, în afara VS Code-ului. Apare doar în 3.3.

**Command hook.** Tipul de hook care rulează o comandă shell la apariția unui eveniment. Cel mai comun tip. Relevant pentru 2.3.

**Context.** Fereastra de informații pe care Claude o vede în timpul conversației: promptul tău, fișierele citite, rezultatele tool call-urilor, CLAUDE.md-ul încărcat. Are limită de tokeni, se umple în timp.

**Diff side-by-side.** Panoul de comparație între cod vechi și cod nou pe care Claude vrea să-l aplice. Îl aprobi sau îl refuzi fragment cu fragment. Relevant pentru 1.3.

**dontAsk.** Setare care marchează anumite tool-uri ca pre-aprobate per proiect, fără confirmare de fiecare dată. Util pentru tool-uri sigure folosite des.

**Exit code.** Numărul returnat de un script când termină. `0` = succes, `2` = blocare în contextul hooks, alte valori = erori. Relevant pentru 2.3.

**Git worktree.** Copie izolată a unui repo Git, în care subagentul poate lucra fără să afecteze fișierele tale principale. Activat cu `isolation: worktree` în frontmatter-ul subagentului. Cere familiaritate de bază cu Git.

**Hook.** Script care se rulează automat când se întâmplă un eveniment în Claude Code (înainte de tool call, după tool call, la submit de prompt). Reflex, nu instrucțiune. Relevant pentru 2.3.

**Headless.** Mod de rulare a lui Claude fără interfață (fără REPL, fără panoul VS Code). Un singur prompt în intrare, un singur răspuns în ieșire. Util pentru scripturi și cron jobs. Activat cu `claude -p` în CLI. Relevant pentru 3.3.

**Manifest.** Fișierul `plugin.json` din `.claude-plugin/` care conține metadatele unui plugin (nume, versiune, descriere, autor). Relevant pentru 3.2.

**Marketplace.** Catalog de plugin-uri, definit într-un fișier `marketplace.json`. Permite distribuirea mai multor plugin-uri dintr-o singură sursă. Relevant pentru 3.2.

**Matcher.** Pattern care specifică pe ce tool se declanșează un hook. `"Bash"` = doar pe tool-ul Bash, `"*"` = pe orice, câmp absent pentru evenimente fără matcher (ex. `UserPromptSubmit`). Relevant pentru 2.3.

**MCP (Model Context Protocol).** Convenție deschisă prin care Claude vorbește cu sisteme externe (Gmail, Notion, GitHub, baze de date). Standardizează conectoarele între Claude și orice serviciu care oferă un MCP server. Relevant pentru 2.2.

**MCP server.** Program care expune un sistem extern prin MCP. Poate oferi tool-uri (acțiuni), resurse (date), prompturi (scurtături predefinite). Rulează local (stdio) sau remote (HTTP/SSE). Relevant pentru 2.2.

**Panel.** Panoul Claude Code integrat în VS Code (sidebar sau tab). Aici scrii prompturi și vezi răspunsurile. Echivalentul vizual al chat-ului în extensie.

**Permission mode.** Nivelul la care Claude cere aprobare pentru acțiuni. Normal (cere la fiecare), Auto-Accept (aprobă edit-uri), Plan Mode (doar planifică, nu execută), bypassPermissions (nu cere deloc). Cycling prin `Shift+Tab`. Relevant pentru 1.3.

**Plan Mode.** Mod în care Claude gândește, citește, propune un plan detaliat, dar nu execută nimic până nu îi dai ok. Util pentru task-uri complexe unde vrei să validezi abordarea înainte.

**Plugin.** Pachet care conține skills, subagenți, hooks, MCP servers și comenzi custom, distribuibil ca un singur bundle. Instalat prin `/plugin install` în panou. Relevant pentru 3.2.

**PreToolUse.** Hook care rulează înainte de fiecare tool call. Poate bloca acțiunea dacă returnează exit code 2. Util pentru guardrails. Relevant pentru 2.3.

**PostToolUse.** Hook care rulează după ce un tool a executat cu succes. Primește și input-ul, și output-ul tool-ului. Util pentru formatare automată, logging. Relevant pentru 2.3.

**Progressive disclosure.** Pattern în care conținutul unui skill (sau plugin) se încarcă gradual. Claude citește descrierea primă, doar când decide să încarce skill-ul primește conținutul complet. Economisește context.

**Prompt.** Mesajul pe care îl trimiți lui Claude. Poate include text, referințe la fișiere cu `@`, instrucțiuni specifice. Claritate în prompt = rezultate bune.

**Scope.** Nivelul la care e configurat ceva (MCP server, skill, subagent). `user` = peste toate proiectele tale, `project` = partajabil cu colaboratorii, `local` = privat în proiectul curent.

**Sesiune.** O conversație completă cu Claude, de la start până la clear sau închidere. Context acumulat într-o sesiune se resetează cu `/clear`.

**Skill.** Metodologie sau know-how capturat într-un fișier `SKILL.md` cu YAML frontmatter. Claude decide autonom când să-l încarce pe baza descrierii. Relevant pentru 3.1.

**Slash command.** Comandă care se scrie cu `/` în panou pentru acțiuni predefinite. Native (`/plan`, `/clear`, `/compact`, `/resume`, `/mcp`) sau custom (definite în `.claude/commands/` sau generate de MCP/skills/plugins).

**Spark icon.** Iconița cu scânteie care apare în editor când Claude e activ pe un fișier. Indicator vizual că o interacțiune e în curs.

**Subagent.** Claude secundar, definit într-un fișier Markdown în `.claude/agents/`, pe care Claude principal îl invocă autonom pentru task-uri specifice. Rulează în conversație izolată, întoarce doar mesajul final. Relevant pentru 2.1.

**System prompt.** Instrucțiunile de bază care definesc cine e Claude și cum se comportă într-o sesiune. Pentru un subagent sau skill, corpul fișierului `SKILL.md` sau fișierul subagentului devine parte din system prompt-ul lui.

**Tokeni.** Unitatea de măsură pentru tot ce trece prin Claude (input, output, context, system prompt). Costul și limita de context se măsoară în tokeni. Un cuvânt în română este, aproximativ, 2-3 tokeni.

**Tool call.** Un pas executat de Claude: citire fișier (Read), editare (Edit), căutare (Grep), rulare comandă (Bash). Vizibil în panou ca bloc expandabil.

**UserPromptSubmit.** Hook care rulează când tu trimiți un prompt. Nu are matcher, rulează la fiecare prompt. Util pentru injectare de context. Relevant pentru 2.3.

**`.mcp.json`.** Fișier de configurare pentru MCP servers la nivel de proiect. Locație: rădăcina proiectului. Relevant pentru 2.2.

**`.claude/`.** Dosar de configurare per proiect. Conține `agents/`, `skills/`, `commands/`, `settings.json` (pentru hooks), `CLAUDE.md`. Versionabil cu Git ca parte din proiect.

**`~/.claude/`.** Dosar de configurare personal, pe userul tău. Conține aceleași subdosare, dar valabile peste toate proiectele. Nu e versionat.

---

*Ultima revizuire: 2026-04-17.*
