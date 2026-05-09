# /explain — Förklara progress eller ett ämne

Används för att förstå vad som byggts, varför det är uppbyggt så, eller vad ett begrepp innebär. Håll förklaringen kort och konceptuell — detta är inte produktivt arbete.

---

## Bestäm vad som ska förklaras

**Om inget argument ges:** Förklara den senaste progressen (git status + TODOS.md + senaste agent-rapport räcker).

**Om ett argument ges** (t.ex. `/explain mcp` eller `/explain hur dashboard funkar`): Förklara det specifika ämnet.

---

## Format

### Ämnesförklaring

```
## [Begrepp]

**Vad det är:** [1–2 meningar, konceptuellt — inte implementation]

**Hur det fungerar:** [kärnmekanismen, gärna med analogi]

**I projektet:** [var det lever i koden, varför det valdes, vilket problem det löser]
```

Max 20 rader. Inga kodblock om de inte är nödvändiga. Fokus på sambanden, inte stegen.

### Progressförklaring

Strukturera efter **lager**:

```
## [Projekt] — vad som finns nu

**Kärnflödet:** [en mening om input → output]

**Komponent A:** [vad det gör och varför det finns]

**Komponent B:** [...]

**Databas / state:** [vilken data som lagras, var]

**Saknas / nästa:** [en mening om vad som kommer härnäst]
```

Max 30 rader.

---

## Viktigt

- Svara direkt i text — inga extra filläsningar om det inte behövs
- Läs max **en** fil om du behöver friska upp kontexten (CLAUDE.md eller relevant kodfil)
- Förklara **varför** saker är byggda som de är, inte bara **vad** de gör
- Undvik stegvisa instruktioner — fokusera på koncept och samband
- Håll det kort. Om något klickar brukar en mening räcka.
