# /research — Skicka iväg en research-agent

Tar ett ämne och delegerar till en research-agent som returnerar en strukturerad rapport. Spara dig själv 2 timmar.

---

## Steg 1 — Tolka ämnet

Argumentet kan vara:
- En specifik fråga: "what's the best way to do X in Python"
- Ett område: "auth patterns 2026"
- En jämförelse: "supabase vs firebase for my use case"

Om ingen argument: fråga "Vad ska jag researcha?"

---

## Steg 2 — Designa prompten

Skriv en self-contained agent-prompt på engelska. Inkludera:

- **Topic** — exakt vad
- **Why** — varför användaren behöver veta detta nu (kontext från CLAUDE.md / TODOS.md)
- **Scope** — vad ska INTE researchas (för att undvika scope creep)
- **Output format** — strukturerad rapport, ej rå-output

Mall:

````
# Research Agent — [topic]

Research: [ämne, mer specifikt än titeln]

Context: [varför användaren bryr sig nu, hämta från CLAUDE.md eller TODOS.md]

Scope:
- Cover: [konkreta delar]
- Skip: [andra delar som verkar närliggande men inte relevanta]

Output: write to `research/<YYYY-MM-DD>_<short-slug>.md` with:

```
# [Topic] — Research Findings
Date: YYYY-MM-DD

## TL;DR
[3-4 sentences]

## Key findings
- [Finding 1 with source URL]
- [Finding 2]
- [Finding 3]

## Recommendations for [project context]
- [Concrete next step 1]
- [Concrete next step 2]

## Open questions
- [Things still unclear]

## Sources
- [URL 1]
- [URL 2]
```

Constraints:
- 5-15 high-quality sources, not 50 mediocre
- Cite primary sources (papers, official docs) over secondary (blog summaries)
- If something is contested, note both sides — don't pick a winner you can't justify
- Don't recommend tools you can't verify still exist / are maintained
````

---

## Steg 3 — Visa prompten

Visa promtpen i ett code block för användaren att kopiera till ny flik.

Säg: "Klistra in i ny flik. Rapport landar i `research/YYYY-MM-DD_<slug>.md`. Säg till när den är klar så jag läser och sammanfattar."

---

## Viktigt

- Research-prompten ska ge full kontext men inte mer — agenten behöver inte hela CLAUDE.md, bara det relevanta
- Ge agenten frihet att gräva djupare än du explicit beställde, men håll scope ärligt
- Spara aldrig hemligheter (API-keys, personnummer) i research-prompten
