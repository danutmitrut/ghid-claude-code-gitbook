*Ghid Claude Code > Anexe > Prompturi esențiale*

# Prompturi esențiale

**Colecție de prompturi testate pentru scenarii tipice ale solopreneurului. Copiezi, adaptezi pe contextul tău, folosești.**

---

## Cum folosești anexa asta

Fiecare prompt de aici e un schelet. Substituie numele de fișiere, referințele la proiect, detaliile specifice cu ale tale. Prompturile sunt grupate pe scenarii. Acolo unde e relevant, am adăugat "Variantă pentru Plan Mode" (apasă `Shift+Tab` până vezi Plan Mode activ, apoi aplici promptul).

Majoritatea prompturilor presupun că ai deschis proiectul în VS Code și Claude Code panel e activ.

---

## Explorare workspace

### Mapare rapidă a proiectului

> **Prompt:**
> "Explorează structura proiectului și dă-mi o hartă de o pagină: ce e fiecare dosar principal, care sunt fișierele-cheie, care e logica generală de organizare. Nu intra în detalii de cod, vreau doar privirea de sus."
>
> **Ce ar trebui să vezi:**
> Claude listează rădăcina, intră în câteva dosare principale, îți dă o descriere narativă a organizării. Util la preluarea unui proiect nou sau când te întorci la unul după luni.

### Căutare funcționalitate existentă

> **Prompt:**
> "Caută dacă în proiect există deja vreo implementare pentru X (descrie pe scurt ce cauți). Dacă există, arată-mi unde (fișier, linii, fragment scurt). Dacă nu există, spune-mi explicit 'nu am găsit'."
>
> **Ce ar trebui să vezi:**
> Claude folosește Grep și Glob să caute, îți arată locații concrete cu fragmente. Previne duplicarea de cod sau funcționalitate.

### Înțelegere fișier specific

> **Prompt:**
> "Citește @path/catre/fisier.ts și explică-mi în 5 bullet-uri: ce face fișierul, care sunt funcțiile principale, ce dependențe are, ce convenții de stil folosește, unde e folosit în rest."

---

## Editare și refactor

### Editare localizată

> **Prompt:**
> "În @src/utils/format.ts, modifică funcția `formatDate` să accepte un al doilea argument opțional `timezone` (default UTC). Actualizează apelurile existente dacă e necesar."
>
> **Ce ar trebui să vezi:**
> Claude deschide fișierul, propune diff-ul în side-by-side, poate căuta alte locuri unde e folosit. Tu aprobi sau ceri ajustări.

### Refactor cu plan prealabil

> **Prompt (cu Plan Mode activ, `Shift+Tab` până apare):**
> "Vreau să refactorez @src/components/Dashboard.tsx în trei componente mai mici: HeaderWidget, StatsPanel, ActivityFeed. Fă-mi un plan detaliat înainte să atingi codul: ce extragi în fiecare, ce props transmiți, cum gestionezi state-ul comun."
>
> **Ce ar trebui să vezi:**
> Claude produce un plan scris, fără să modifice cod. Tu citești, ajustezi dacă e cazul, apoi ieși din Plan Mode și spui "execută planul".

### Rescriere cu stil consistent

> **Prompt:**
> "Rescrie @src/api/client.ts folosind stilul de coding din @src/api/auth.ts. Păstrează logica identică, aliniază doar structura și conveția de nume."

---

## Depanare și diagnostic

### Eroare reprodusă cu mesaj

> **Prompt:**
> "Am următoarea eroare în produție (lipesc stack trace-ul): [stack trace]. Uită-te la @path/catre/fisier.ts unde se întâmplă și propune-mi cauza cea mai probabilă. Nu modifica cod până nu confirmăm cauza împreună."
>
> **Ce ar trebui să vezi:**
> Claude citește codul relevant, propune ipoteze, îți cere eventual mai multe informații înainte să încerce fix-ul.

### Eroare intermitentă fără stack trace

> **Prompt:**
> "Văd comportament ciudat în [descrie simptomul] dar nu am stack trace clar. Investighează @src/features/X/ și identifică trei suspecți principali pentru ce ar putea cauza asta. Pentru fiecare, spune-mi de ce l-ai inclus și ce test aș putea face să verific."

### Verificare incrementală

> **Prompt:**
> "Rulează testele pentru @src/features/X. Dacă eșuează, arată-mi output-ul complet și propune un plan de fix. Nu modifica nimic până nu îți dau ok pe plan."

---

## Scriere și conținut

### Articol de blog în stilul tău

> **Prompt:**
> "/scriere-articole (dacă ai skill-ul configurat din 3.1)"
> urmat de
> "Subiectul: [titlu sau unghi]. Audiența: [descriere]. Lungime: [aprox cuvinte]."
>
> **Variantă fără skill:**
> "Scrie un articol de blog pentru solopreneuri români despre [subiect]. Structură: hook narativ, problemă, soluție, exemplu concret, call to action scurt. Ton: direct, diacritice complete, fără em dash, paragrafe de 2-4 fraze."

### Email către client

> **Prompt:**
> "Clientul mi-a scris asta: [cite conținutul sau atașezi cu @]. Scrie un draft de răspuns care: (1) confirmă ce am înțeles corect, (2) clarifică [ce anume], (3) propune [acțiunea]. Ton profesionist dar familiar, fără formalism excesiv."

### Rezumat întâlnire

> **Prompt:**
> "Citește @notite/intalnire-2026-04-20.md și produ un rezumat structurat: decizii luate, action items cu responsabil și termen, întrebări deschise. Fără să adaugi nimic care nu e în note."

---

## Procesare și analiză

### Extragere date structurate dintr-un text

> **Prompt:**
> "Citește @notite/interviuri-clienti.md. Extrage-mi pentru fiecare client: nume, industrie, problemă principală menționată, cuvinte cheie relevante. Rezultatul într-un tabel Markdown."

### Clasificare și triaj

> **Prompt:**
> "Ți-am dat o listă de 30 de idei de articole în @idei/lista.md. Clasifică-le în trei bucăți: (1) cele mai probabil să rezoneze cu audiența mea (solopreneuri români, AI/productivitate), (2) cele care necesită mai multă cercetare, (3) cele pe care le-aș sări. Pentru fiecare, o propoziție de justificare."

### Comparare variante

> **Prompt:**
> "Am două variante de landing page salvate ca @landing-v1.md și @landing-v2.md. Compară-le pe: claritatea propunerii de valoare, fluxul narativ, concretețea CTA-ului. Spune-mi care funcționează mai bine și de ce."

---

## Memorie și setup

### Scriere inițială CLAUDE.md

> **Prompt:**
> "Explorează proiectul și propune-mi un draft pentru `CLAUDE.md`. Include: stack-ul tehnologic, structura dosarelor, convențiile de stil observate în cod, dependențele cheie. Scurt și factual, sub 50 linii."

### Actualizare CLAUDE.md după schimbare majoră

> **Prompt:**
> "Am făcut schimbări importante în structura proiectului (pe scurt, [descrie]). Citește `CLAUDE.md` actual și propune-mi actualizări: ce ar trebui eliminat, ce ar trebui adăugat, ce ar trebui rescris."

### Creare skill nou

> **Prompt:**
> "Vreau să capturez într-un skill metodologia mea de [descriere metodologie]. Pune-mi întrebările care mi-ar ajuta să articulez clar: pașii, regulile, exemplele. Apoi produ `SKILL.md` complet, cu YAML frontmatter corect (descriere la persoana a treia)."

---

## Plan Mode, prompturi specifice

Când vrei să planifici înainte să execuți, activezi Plan Mode cu `Shift+Tab` până vezi modul activat în panou.

### Plan înainte de o schimbare mare

> **Prompt (în Plan Mode):**
> "Vreau să [descrie obiectivul major, ex. 'să migrez autentificarea de la JWT la session cookies']. Fă-mi un plan detaliat: pași în ordine, fișiere afectate, riscuri principale, ce verificăm între pași. Nu atinge cod până nu îți dau ok pe plan."

### Plan pentru un feature nou

> **Prompt (în Plan Mode):**
> "Feature nou: [descriere]. Plan de implementare în etape, cu fișiere noi pe care le creez și cele existente pe care le modific. Dependențe între etape. Propune și o strategie de rollback dacă ceva merge prost."

---

## Prompturi pentru subagenți și MCP

### Invocare manuală subagent

> **Prompt:**
> "Folosește subagentul `code-reviewer` pentru @src/utils/csv-import.ts. Vreau un review complet înainte să consider task-ul terminat."

### Verificare acces MCP

> **Prompt:**
> "Listează-mi ultimele 5 pagini pe care le-am editat în Notion. (Dacă MCP server Notion e conectat, hai să verific că merge.)"

### Task cu tool-uri externe

> **Prompt:**
> "Citește ultimele 3 emailuri de la [client] prin Gmail MCP. Rezumă-mi ce întreabă și propune-mi schelete de răspuns pentru fiecare. Nu trimite nimic fără confirmarea mea."

---

## Varianta B: prompt cu cod inclus

Când vrei ca Claude să analizeze sau să modifice un fragment de cod pe care îl lipești direct, nu dintr-un fișier.

> **Prompt:**
> "Uită-te la fragmentul de cod de mai jos. Identifică bug-urile potențiale și propune un fix."

```typescript
function parseAmount(input: string): number {
  const cleaned = input.replace(",", ".");
  return parseFloat(cleaned);
}
```

> **Ce ar trebui să vezi:**
> Claude analizează fragmentul (ex. "parseFloat întoarce NaN pentru input invalid, nu ai validare"), propune fix.

---

## Prompturi de verificare

### Verificare propriu output

> **Prompt (după ce Claude tocmai a produs ceva):**
> "Recitește output-ul de mai sus cu ochi critici. Identifică trei lucruri pe care le-ai putea îmbunătăți. Apoi aplică-le."

### Double-check pe fapte sau cifre

> **Prompt:**
> "Ai afirmat mai sus că [citat cu cifră sau afirmație]. Verifică această afirmație. Dacă e corectă, spune-mi sursa. Dacă e presupunere, marchează-o ca atare."

---

*Ultima revizuire: 2026-04-17.*
