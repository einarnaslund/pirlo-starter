# /projectleader — Projektöversikt + delegerings-design

Ger en omedelbar, ärlig bild av var projektet befinner sig — och bestämmer hur nästa arbetspass ska fördelas mellan dig själv och delegerade agenter.

---

## Steg 1 — Hämta fakta

Kör parallellt:
- Läs `HANDOFF.md` (om den finns)
- Läs `TODOS.md`
- Läs `CLAUDE.md` (Strategiska beslut + roadmap)
- Kör `git log --oneline -8`
- Läs `agent_reports/LAUNCH_LOG.md` (vilka agenter har startats)
- Läs alla övriga filer i `agent_reports/` (vad rapporterade de tillbaka)

---

## Steg 2 — Korskolla startade agenter mot rapporter

Jämför LAUNCH_LOG mot filerna i `agent_reports/`:

För varje rad i LAUNCH_LOG: leta efter en rapport-fil som matchar agentens nummer + datum (`agent_reports/agent_N_YYYY-MM-DD.md`).

Kategorisera varje agent:
- **Klar** — rapport finns med `Status: done`
- **Delvis klar** — rapport finns med `Status: partial`
- **Blockad** — rapport finns med `Status: blocked`
- **⚠️ Pågår / Obekräftad** — agent finns i LAUNCH_LOG men ingen rapport

---

## Steg 3 — Presentera läget

```
## Projektläge — [datum]

### Var vi är
[En mening om fas/stadie.]

### Nyligen gjort
- [Konkret sak 1]
- [Konkret sak 2]
- [Konkret sak 3]
(Max 5 punkter, från HANDOFF.md + git log.)

### Blockers
- [Vad som hindrar oss]
(Om inga: "Inga kända blockers.")

### Nästa steg (i prioritetsordning)
1. [Första steget — vad, varför högst prio]
2. [Andra]
3. [Tredje]

### Agent-status
[Lista alla från LAUNCH_LOG. Markera ⚠️ pågående tydligt.]
```

---

## Steg 4 — Agent-analys

### 4a — Pågående agenter blockerar nya

Finns agenter med status **⚠️ Pågår / Obekräftad**?

**Om JA:**
- Lista dem: "Agent N startades [datum] och har inte rapporterat."
- Identifiera vilket arbetsområde de äger (filer, scope).
- Starta **INTE** en ny agent som rör samma filer eller scope.
- Skriv: "Väntar på rapport från Agent N innan X kan startas."

**Om NEJ:** fortsätt till 4b.

### 4b — Välj nästa agent(er)

Titta på "Nästa steg"-listan. Frågan: finns minst två tasks som är **genuint oberoende** — delar ingen kod, ingen state — och var och en tillräckligt avgränsad för en separat Claude Code-flik?

**Om JA** — skriv ut en prompt per agent. Varje prompt self-contained.
**Om NEJ** — skriv ut en enda prompt för nästa agent.

Promptformat:

````
# Agent N — [kort beskrivning]

You are working on **[projektnamn]**. [En mening om vad det är.]
You have noll context från någon tidigare session. Repo: [path].

## Why this work
[2-4 meningar om varför det här ska göras.]

## Your task
[Konkret vad de ska bygga.]

## Constraints
- DO NOT [scope creep]

## Final step — Write agent report (mandatory)

When done, write `agent_reports/agent_N_YYYY-MM-DD.md`:

```markdown
# Agent N — [short description]
Date: YYYY-MM-DD
Status: done | partial | blocked

## Accomplished
## Files changed
## Errors / blockers
## Next session needs to know
```
````

---

## Viktigt

- Hitta inte på något. Om du inte vet, skriv "Okänt — kolla TODOS.md".
- Agent-prompter på engelska och self-contained. Mottagaren har noll kontext.
- **Första raden i varje agent-prompt code block:** `# Agent N — [beskrivning]`. Det blir tabbtiteln när du har flera agenter öppna.
- Börja inte arbeta efteråt om inte användaren ber dig.
