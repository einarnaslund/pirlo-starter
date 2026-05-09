# CLAUDE.md

This file provides guidance to Claude Code when working in this project.

> Projektets levande beslutsdokument. Uppdaterad: {{DATE}}

---

## Vad är detta projekt?

{{Beskrivning av vad du bygger. Fyll i under /setup eller manuellt.}}

Ersätt detta med 2-3 meningar om vad projektet faktiskt gör. Exempel:

> "En personlig assistent som varje morgon kompilerar en briefing baserat på min kalender, Slack-DMs, och GitHub-notifikationer. Mejlas kl 07:30."
>
> "Sales-prospektör som scannar svenska SaaS-bolag i SCB, kvalificerar mot min ICP, och skriver outreach. Jag godkänner innan utskick."

---

## Strategiska beslut (låsta)

Den här tabellen håller alla beslut som du *inte ska ifrågasätta utan diskussion*. Lägg till en rad varje gång du gör ett medvetet val som du inte vill omförhandla varje vecka.

| Fråga | Beslut | Datum |
|---|---|---|
| {{Exempel: Hur ska användardata lagras?}} | {{Exempel: Supabase i Frankfurt, EU-only}} | {{datum}} |
| {{Lägg till fler under tiden}} | | |

---

## Tekniska beslut (låsta)

| Beslut | Värde |
|---|---|
| Språk | {{Python 3.12 / TypeScript / etc.}} |
| Hosting | {{Railway / Fly.io Stockholm / lokalt}} |
| Databas | {{Supabase / SQLite / ingen}} |
| LLM-routing | Pirlo (https://pirlo.fly.dev/v1) |
| Versionskontroll | git, main-branch |

---

## Filer och dokument

| Dokument | Plats | Innehåll |
|---|---|---|
| CLAUDE.md | denna | Master-doc — fylls under tiden |
| HANDOFF.md | rot | Vad förra sessionen lämnade + var nästa börjar |
| TODOS.md | rot | Byggkö |
| README.md | rot | Onboarding för nya användare |
| PIRLO.md | rot | Pirlo-integration |
| AGENT_BUILDING.md | rot | Hur agent-workflowen funkar |
| USE_CASES.md | rot | Konkreta projekt-idéer |
| `memory/` | dir | Persistent minne om dig + projektet |
| `agent_reports/` | dir | Rapporter från delegerade agenter |
| `docs/` | dir | Djup-docs per ämne (du fyller efter behov) |

---

## Roadmap

Fyll i när du har en bild av vad du bygger. Format:

**Fas 1 — MVP (vecka 1-2)**
- ⬜ {{Första milstolpe}}
- ⬜ {{Andra}}

**Fas 2 — Polish (vecka 3-4)**
- ⬜ ...

---

*Den här filen är skriven för Claude Code. Skriv på svenska eller engelska — Claude förstår båda. Var konkret. Ta bort placeholder-text när du fyllt i.*
