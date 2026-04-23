*Ghid Claude Code > Partea III. Construcție*

# Partea III. Construcție

**Din consumator în constructor. La final, îți împachetezi metodologia în skills și plugins reutilizabile, sau scoți Claude din VS Code în pipeline-uri.**

---

## Ce conține

Trei straturi care transformă folosirea în producție:

[**3.1 Skills**](skills.md) capturi metodologia proprie în fișiere `SKILL.md` cu YAML frontmatter. Claude le încarcă autonom când descrierea match-uiește contextul. Scop: `.claude/skills/` (proiect) sau `~/.claude/skills/` (global).

[**3.2 Plugins**](plugins.md) împachetezi skills, agents, hooks și MCP servers într-un bundle distribuibil. Definești `marketplace.json` ca alții să descarce cu versiuni și update-uri automate.

[**3.3 SDK și headless**](sdk-headless.md) scoți Claude din VS Code. CLI headless cu `claude -p`, Agent SDK TypeScript și Python pentru pipeline-uri de producție, integrări programatice, CI/CD.

---

## Prerechizite pentru Partea III

- Partea I completă, minim 1.5 Memorie de proiect
- Pentru 3.1: ai metodologie proprie repetabilă de capturat (nu construiești skill pentru un task unic)
- Pentru 3.2: o echipă sau o comunitate cu care împărtășești
- Pentru 3.3: familiaritate cu TypeScript sau Python, plus caz de producție definit

---

## Ce ai după Partea III

- Metodologia ta capturată în skills pe care Claude le invocă singur
- Plugin publicabil pentru echipa ta sau comunitate
- Claude rulând headless în pipeline-uri, fără VS Code deschis
- Înțelegi granița dintre "unealtă personală" și "componentă de producție"

---

## Ordinea recomandată

Parțial independentă, dar 3.1 Skills e fundația pentru 3.2 Plugins (plugin-urile împachetează skills). 3.3 SDK e separat, relevant doar dacă ai caz de producție.

Ordine recomandată: 3.1 → 3.2. 3.3 poate veni oricând după ce ai Partea I.

---

→ Începe cu [3.1 Skills](skills.md).
