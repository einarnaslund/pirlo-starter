# /start — Börja ny session direkt

Kör detta i början av varje ny Claude Code-session. Laddar kontext snabbt och identifierar exakt nästa task.

---

## Steg 1 — Läs handoff och byggkö

Läs dessa filer parallellt:
- `HANDOFF.md` (om den finns — berättar vad förra sessionen lämnade)
- `TODOS.md` (hitta första ostartade steget)
- `memory/MEMORY.md` (vem är användaren, hur jobbar de)

Kör `git status` för att se osparade ändringar och `git log --oneline -5` för senaste commits.

---

## Steg 2 — Läs CLAUDE.md kort

Skanna CLAUDE.md "Strategiska beslut"-sektionen om den finns. Du behöver inte läsa allt — bara veta vilka beslut som är låsta så du inte föreslår något som strider.

---

## Steg 3 — Presentera läget och börja

Skriv ett kort statusblock (max 8 rader):

```
## Session start — [datum]

**Förra sessionen:** [en mening om vad som gjordes, från HANDOFF.md]
**Nuvarande prioritet:** [från TODOS.md]
**Nästa konkreta åtgärd:** [en mening — vad du gör nu]
**Eventuella blockers:** [eller "Inga"]
```

Börja sedan direkt på nästa steg utan att vänta på bekräftelse — om det är ett tydligt nästa steg i TODOS.md, kör det.

---

## Viktigt

- Läs **inte** alla docs/-filer — det slösar kontext.
- Fråga **inte** vad användaren vill göra om TODOS.md är tydlig.
- Om HANDOFF.md saknas: läs CLAUDE.md + TODOS.md och hitta första ⬜.
