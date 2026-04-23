*Ghid Claude Code > Partea II. Extensii*

# Partea II. Extensii

**Delegare, conectare și automatizare. La final, Claude ajunge la sistemele tale externe și reacționează la evenimente.**

---

## Ce conține

Trei straturi care extind ce poate face Claude dincolo de workspace-ul local:

[**2.1 Subagenți**](subagenti.md) agenți secundari cu context izolat, pentru delegarea task-urilor cu multe subetape sau pentru verificări independente care nu poluează conversația principală. Fișier de config `.claude/agents/` cu YAML frontmatter.

[**2.2 MCP servers**](mcp-servers.md) conectare la Gmail, Notion, GitHub, baze de date, API-uri. Configurare prin `.mcp.json`, management cu `claude mcp list/get/remove`. Prompturile MCP devin slash commands gen `/mcp__server__prompt`.

[**2.3 Hooks**](hooks.md) automatizare pe evenimente Claude Code: `PreToolUse`, `PostToolUse`, `Stop`, `SubagentStart`, `SessionStart` și altele. Tipuri: command hooks, HTTP hooks, prompt hooks, agent hooks.

---

## Prerechizite pentru Partea II

- Partea I completă (1.1 până la 1.4)
- Pentru 2.2: Node.js sau Python instalat (majoritatea MCP-urilor sunt scrise în una din ele)
- Pentru 2.3: familiaritate cu bash sau cunoștințe de scripting de bază

---

## Ce ai după Partea II

- Deleghezi task-uri complexe la subagenți fără să îți umpli contextul principal
- Claude citește emailuri din Gmail, notează în Notion, deschide PR-uri în GitHub
- Automatizezi verificări pe pre-commit, pre-tool-use, post-edit
- Structurezi workflow-uri unde Claude reacționează la schimbări, nu doar la prompturi

---

## Ordinea recomandată

Parțial independentă. 2.2 MCP servers dă cea mai mare valoare directă pentru solopreneuri și poate fi primul. 2.1 Subagenți devine util când 2.2 îți aduce task-uri multi-pas. 2.3 Hooks e ultimul ca importanță, util doar când ai repetiție identificată.

Ordine recomandată: 2.2 → 2.1 → 2.3.

---

→ Începe cu [2.2 MCP servers](mcp-servers.md) dacă vrei valoare rapidă, sau [2.1 Subagenți](subagenti.md) dacă urmezi ordinea canonică.
