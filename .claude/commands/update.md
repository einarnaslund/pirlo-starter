# /update — Spara session och förbered handoff

Kör detta i slutet av varje Claude Code-session. Tar emot valfri usage-procent som argument: `/update 11%`

---

## Steg 0 — Fånga usage-nivå

Kolla om ett procenttal angavs som argument. Om ja, notera det. Om nej, lämna usage-sektionen tom.

**Om usage < 20%: kör Low-Usage Protocol** (se längst ned) INNAN steg 1–4.

---

## Steg 1 — Skanna sessionen

Identifiera:

- **A. Slutförda uppgifter** — kod skriven, filer ändrade, beslut låsta
- **B. Ny kunskap** — research-findings, tekniska insikter
- **C. Ändrade planer** — vad visade sig inte fungera, omprioriteringar
- **D. Öppna frågor** — saker som kom upp men inte löstes

---

## Steg 2 — Uppdatera TODOS.md

Markera slutförda steg med ✅. Lägg INTE till "vad jag gjorde"-kommentarer — det hör hemma i git-historiken.

Om nya steg identifierades: lägg till på rätt plats i listan.

---

## Steg 3 — Uppdatera CLAUDE.md (om relevant)

Ändra status på roadmap-tasks om de ändrats:
- `⬜` → `✅` för slutförda
- `⬜` → `🔴` om blockerat

Bara om status faktiskt ändrats.

---

## Steg 4 — Skriv HANDOFF.md

Skriv över `HANDOFF.md` med detta format:

```markdown
# Handoff — [YYYY-MM-DD]

## Gjort denna session
- [konkret punkt per sak som faktiskt slutfördes]

## Nästa session börjar här
**Prioritet:** [vad som ska göras]
**Fil att öppna:** [path]
**Konkret nästa steg:** [en mening]

## Agenter — förberedda men ej körda
[Lista agent-promptar som är skrivna men inte startade.]
[Om inga: utelämna sektionen.]

## Blockers / öppna frågor
- [lista, eller "Inga"]

## Account Usage
- Aktivt konto: [X]% kvar
- Andra konto: okänt / [X]%

## Filer ändrade denna session
- [lista]
```

Håll sektioner korta. HANDOFF.md ska kunna läsas på 20 sekunder.

---

## Low-Usage Protocol (körs om usage < 20%)

Kör DESSA steg FÖRST, innan steg 1–4:

**L1 — Committa allt uncommittat**
Kör `git status`. Committa alla ändrade och otrackade filer i logiska grupper.
Använd `git add <specifika filer>` — aldrig `git add -A`.

**L2 — Formulera agent-promptar**
Titta på TODOS.md (Nästa steg). Skriv klara agent-promptar för de två högsta ostartade prioriteterna.
Spara dem i HANDOFF.md under "Agenter — förberedda men ej körda".
Starta dem INTE (usage för låg — agenten kan avbrytas mitt i).

**L3 — Uppdatera LAUNCH_LOG**
Lägg till en rad i `agent_reports/LAUNCH_LOG.md` för varje förberedd agent med status "⏳ Förberedd — ej startad".

Fortsätt sedan med steg 1–4.
