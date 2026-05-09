# /quickwin — Sök efter snabbvinster

Tar ett område och delegerar till en agent som söker GitHub + webb efter "färdiga lösningar du kan stjäla". Snabbare än att bygga från noll.

---

## Steg 1 — Tolka området

Argumentet är ett område där du fastnat eller vill effektivisera:
- "anti-AI-slop UI patterns"
- "fastest way to deploy Python on edge in EU"
- "library for x"

Om inget argument: fråga "Vad letar du quickwins inom?"

---

## Steg 2 — Designa prompten

Skriv en agent-prompt på engelska:

````
# Quickwin Agent — [område]

Context: [varför användaren letar lösningar]

Find 5-7 quick wins in this space. A "quick win" is:
- An existing library / repo / pattern you can adopt in <1 day
- A blog post / article that lays out a tested approach
- A tool that already does what you'd otherwise build

Sources to prioritize:
- GitHub trending in [språk/ram]
- Hacker News best-of-2026
- Awesome-* lists curated by experts
- Recent (2025-2026) blog posts from credible authors

Output: write to `quickwins/<YYYY-MM-DD>_<slug>.md` with:

```
# Quickwins — [topic]
Date: YYYY-MM-DD

## TL;DR
[Single-sentence verdict — what to actually adopt.]

## Findings (in priority order)

### 1. [Name] — [one-line what]
- Source: [URL]
- Stars: [N]
- Maintenance: [active/abandoned/risky]
- Adopt cost: [hours]
- Why it fits: [2-3 sentences]
- Why not: [counter — when this is a wrong fit]

### 2. ...

## Recommendation
[1-3 specific things the user should adopt this week.]

## Skip these (so the user knows what NOT to look at)
- [Thing X — why it's tempting but bad]
```

Constraints:
- Verify each source exists and is maintained (not 5-year-old abandoned repo)
- Don't pad — 5 great wins beats 15 mediocre ones
- Be opinionated. If two tools do the same thing, pick one and explain why
````

---

## Steg 3 — Visa prompten + förvänta dig rapport

Visa code block + säg: "Klistra in i ny flik. Rapport landar i `quickwins/YYYY-MM-DD_<slug>.md`."

---

## Viktigt

- Quickwin är inte research. Research drar konceptuella slutsatser; quickwin pekar på konkreta saker att adoptera.
- Var skeptisk mot agentens rekommendationer — verifiera själv minst en innan du adopterar.
- Spara quickwin-rapporter i `quickwins/`-mappen — annars tappar du dem.
