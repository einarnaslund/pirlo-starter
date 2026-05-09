# Memory — Persistent kunskap om dig och projektet

Den här mappen är Claudes "långtidsminne". Filer här läses automatiskt i framtida sessioner så Claude inte glömmer vem du är eller hur du jobbar.

## Fyra typer av minne

### `user`

Vem du är. Roll, jobb, expertis-områden, ansvar.

**När:** När Claude lär sig något om din profession eller perspektiv.

**Exempel-fil:** `user_role.md`
```markdown
---
name: Användarens roll
description: Hugo är data scientist på fintech-bolag, fokus observability
type: user
---

Hugo är senior data scientist på fintech-startup. Driver
observability-stacken (logging, metrics, tracing). Tio års Python.
Lär sig React just nu — frame frontend-förklaringar i termer av
backend-analoger.
```

### `feedback`

Hur du vill att Claude ska bete sig. Do's och don'ts.

**När:** När du säger "stop doing X", "always do Y", "yes, perfect, keep doing that".

**Exempel-fil:** `feedback_communication_style.md`
```markdown
---
name: Kommunikationsstil
description: Vill ha terse svar, inga sammanfattningar i slutet, läs diff:en själv
type: feedback
---

Hugo vill ha kort, koncist språk. Ingen "Här är vad jag gjorde:"-summary
i slutet av varje svar — han läser diff:en själv. Frågor besvaras
direkt, inte med "Bra fråga!".

**Why:** Han har sagt "stop summarizing" två gånger.
**How to apply:** Avsluta utan summary om inte explicit ombedd.
```

### `project`

Pågående arbete, mål, deadlines, kontext kring specifika projekt.

**När:** När du lär dig om projekt-status, vem som gör vad, deadlines.

**Exempel-fil:** `project_q3_dashboard.md`
```markdown
---
name: Q3 dashboard-bygget
description: Bygger en savings-dashboard till första pilot-kund, deadline 2026-09-15
type: project
---

Q3-projekt: bygga en savings-dashboard som visar månads-trend per
kund. Deadline 2026-09-15. Pilot-kund jobbar inom regulerad sektor
och behöver EU AI Act-bevis-spår.

**Why:** Pitch-artefakt — visar konkret savings-data i sales-möten.
**How to apply:** Skapa allt med EU-only-routing default;
prompt-storage on; PII-tags på allt med kund-data.
```

### `reference`

Pekare till saker i externa system. Linear-projekt, Slack-kanaler, Grafana-dashboards, GitHub-repos.

**När:** När du lär dig var nyttig info finns utanför projektmappen.

**Exempel-fil:** `reference_linear_pirlo_project.md`
```markdown
---
name: Linear PIRLO-projektet
description: Bugs och feature-requests trackas i Linear under "PIRLO"
type: reference
---

Pirlo-buggar och feature-requests trackas i Linear under projektet
"PIRLO". URL: https://linear.app/<org>/project/PIRLO

Kolla där först innan du föreslår new features.
```

---

## Hur du lägger till en ny memory

1. Skapa `memory/<typ>_<kort-namn>.md` med frontmatter (se exempel ovan)
2. Lägg en rad i `memory/MEMORY.md`:
   ```markdown
   - [Titel](typ_kort-namn.md) — en-mening hook
   ```

Eller säg åt Claude: "Kom ihåg att [fakta]." — Claude skapar filen åt dig.

## Vad du INTE ska spara i memory

- **Hemligheter** (lösenord, API-keys, kreditkortsnummer) — använd `.env` istället
- **Saker som ändras varje vecka** (status, deadlines för småprojekt) — använd TODOS.md
- **Saker som finns i koden** — Claude läser koden om hen behöver
- **Saker du inte vill att framtida Claude-sessioner ska veta** — saved memories är bestående

## Säkerhet

Memory-filerna är vanliga textfiler. De ligger i din Drive-backup. **Skriv inte hemligheter där.** Om du händelsevis råkat: ta bort filen och `git filter-repo` (eller skapa nytt repo om historiken är farlig).

---

*Memory-systemet är inspirerat av Claude Codes `auto-memory`-feature. Det fungerar offline och är 100% under din kontroll.*
