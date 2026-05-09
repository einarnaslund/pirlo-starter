# Agent Building — Hur du bygger med den här mallen

Den här filen lär dig hur agent-workflowen fungerar och vad du kan bygga med den. Läs `README.md` först om du inte gjort det.

## Mental modell

Du jobbar inte längre direkt mot AI:n. Du har en **projektledare** (Claude i din session) som har en stab av **subagenter**. När du vill ha något gjort:

1. Du säger åt projektledaren vad du vill ha
2. Projektledaren designar uppdelningen
3. Du startar varje subagent i en parallell flik (kopierar prompten, paste:ar i ny chatt)
4. Subagenterna jobbar autonomt och skriver rapporter till `agent_reports/`
5. Du kör `/projectleader` igen — den läser rapporterna och föreslår nästa steg

```
Du
 │
 ▼
Projektledare (denna session)
 │  delegerar
 ▼
Subagent 1 (ny flik)  ──▶ rapport ──┐
Subagent 2 (ny flik)  ──▶ rapport ──┼─▶ Projektledare läser
Subagent 3 (ny flik)  ──▶ rapport ──┘   och föreslår nästa
```

Skillnaden mot vanlig Claude Code: **persistens + parallellism + struktur**. Du tappar ingenting mellan sessioner. Du kör 3-5 saker parallellt. Allt är spårat.

## Kärn-filer du kommer leva i

| Fil | Vad |
|---|---|
| `CLAUDE.md` | Projektets master-doc — vad det är, varför, vilka beslut är låsta |
| `TODOS.md` | Byggkö — vad ska göras härnäst |
| `HANDOFF.md` | Vad förra sessionen gjorde + var du börjar nästa |
| `agent_reports/LAUNCH_LOG.md` | Lista över alla agenter du startat + status |
| `agent_reports/agent_N_DATE.md` | En rapport per startad agent |
| `memory/MEMORY.md` | Index över persistent minne om dig och projektet |
| `docs/` | Djup-docs per ämne (du fyller efter behov) |

## Slash-kommandon

Alla finns i `.claude/commands/`. Du kan läsa dem som dokumentation.

| Kommando | När |
|---|---|
| `/setup` | Förstagångs-installation. Kör först. |
| `/start` | Början av varje session. Laddar kontext snabbt. |
| `/update` | Slutet av varje session. Sparar handoff. |
| `/projectleader` | Översikt + delegerings-design |
| `/explain <ämne>` | Få något förklarat (kod, koncept, läge) |
| `/research <ämne>` | Skicka iväg en research-agent |
| `/quickwin <område>` | Sök GitHub + webb efter snabbvinster |
| `/chatgpt-memory` | Migrera minne från ChatGPT (görs en gång) |

## Att designa en agent-prompt

Bra agent-prompter är **self-contained**. Agent som tar emot prompten har noll kontext från ditt huvudchatt — den ska kunna köra direkt.

**Mall:**

````
# Agent N — [kort beskrivning]

You are working on **[projektnamn]**. [En mening om vad det är.]
You have noll context från någon tidigare session. Repo: [path].

## Why this work

[2-4 meningar om varför det här ska göras nu, vad problemet är.]

## Your task

[Konkret vad de ska bygga/skriva/researcha. Specifika filer som ska ändras eller skapas.]

## Constraints

- DO NOT [scope creep]
- DO NOT [andra antaganden som inte stämmer]
- [Test som ska passera]

## Final step — Write agent report (mandatory)

When done or blocked, write `agent_reports/agent_N_YYYY-MM-DD.md` using this format:

```markdown
# Agent N — [short description]
Date: YYYY-MM-DD
Status: done | partial | blocked

## Accomplished
## Files changed
## Errors / blockers
```
````

**Det viktigaste:** spara nyttan — `Final step — Write agent report (mandatory)`. Utan rapport vet du inte vad som hände.

## När du ska delegera vs göra själv

| Gör själv | Delegera |
|---|---|
| Frågor om kontext / status | Bygg-tasks som tar >5 min |
| Beslut om strategi / scope | Research / quickwin |
| Kort fix som tar 30 sek | Test-skrivning |
| Edits du vill se direkt | Refaktorering |
| Säkerhetsavvägningar | Bygga subkomponenter parallellt |

Tumregel: **om du kan beskriva det som en self-contained prompt på 500-1500 ord, delegera**. Om det kräver mycket back-and-forth, gör själv.

## Backend-grunderna (om du behöver databas)

Många agent-projekt behöver lagring (data du samlat, history, user prefs). Tre verktyg som hänger ihop bra:

### Supabase (databas + auth + storage)

- **Vad:** PostgreSQL i molnet med kraftfullt row-level security, edge-functions, webhooks. Gratis tier räcker länge (500MB DB, 1GB storage).
- **Varför:** Slipper bygga eget API. JS- och Python-klienter färdiga.
- **Hur:**
  1. Skapa konto på https://supabase.com (gratis)
  2. Skapa nytt projekt — välj Frankfurt eller Stockholm för EU
  3. Spara `Project URL` och `anon key` (under Settings → API)
  4. Installera CLI: `npm install -g supabase` (kräver Node)
  5. I projekt-mappen: `supabase init`, sen `supabase link --project-ref <ref>`
  6. Skapa migrationer i `supabase/migrations/`, push med `supabase db push`
- **Tips:** Kör allt schema som migrationer, inte som SQL i webb-konsolen. Då kan du återskapa allt om något går sönder.

### Railway eller Fly.io (hosting)

Om din agent har en server-del (FastAPI, scheduled jobs, webhooks):

- **Railway** är enklast — peka på GitHub-repot, klicka "Deploy", klart. ~$5/månad efter free tier. https://railway.app
- **Fly.io** är billigare (gratis tier finns) men mer hands-on. Kräver `fly launch` + Dockerfile. Bra för EU (välj `arn` region för Stockholm). https://fly.io

För hobby/test: Railway. För produktion / EU-data: Fly.io i Stockholm.

### MCP (Model Context Protocol)

- **Vad:** En standard för att låta AI-agenter anropa "tools" (funktioner i din kod) på ett strukturerat sätt.
- **Varför:** Om du bygger ett verktyg som flera AI-agenter (Claude Desktop, Cursor, Pirlo) ska kunna använda — exponera det som en MCP-server.
- **Var börja:** https://modelcontextprotocol.io
- **Snabbast:** Använd `FastMCP` (Python) eller `@modelcontextprotocol/sdk` (TypeScript) för att slå upp en server på 50 rader kod.

## Att skapa API-keys

För många integrationer behöver du nycklar. Hantera dem så här:

1. **Aldrig committa keys till git.** Använd `.env`-fil + `.gitignore`.
2. **`.env`-formatet:**
   ```
   OPENAI_API_KEY=sk-...
   SUPABASE_URL=https://xxx.supabase.co
   SUPABASE_KEY=eyJ...
   PIRLO_API_KEY=sk_pirlo_...
   ```
3. **Ladda i Python:** `pip install python-dotenv` → `from dotenv import load_dotenv; load_dotenv()` → `os.getenv("OPENAI_API_KEY")`
4. **Ladda i Node:** `npm install dotenv` → `import 'dotenv/config'`
5. **Produktion:** Sätt env-vars i Railway/Fly via deras CLI eller dashboard. Aldrig i koden.
6. **Roterbarhet:** Spara key-creation-datum. Rotera var 90:e dag (säkerhetsstandard).

## Mappstruktur — när projektet växer

Den här mallen ger dig:

```
.
├── README.md, CLAUDE.md, HANDOFF.md, TODOS.md   ← du fyller in
├── .claude/
│   ├── commands/      ← slash-kommandon
│   └── agents/        ← subagent-konfigurationer
├── agent_reports/     ← rapporter från agenter du startat
├── memory/            ← persistent minne om dig + projektet
├── docs/              ← djup-docs per ämne
└── scripts/           ← engångsskript
```

När du börjar bygga riktig kod, lägg till:

```
├── src/                      (eller proxy/, app/, ditt-paketnamn/)
├── tests/
├── supabase/migrations/      (om du använder Supabase)
├── pyproject.toml            (Python) eller package.json (Node)
└── .env                      (gitignored)
```

## Tips från någon som gjort det här länge

- **Skriv inte långa promptar för korta tasks.** Om du kan göra det själv på 30 sek, gör det.
- **Läs varje agent-rapport.** Du behöver veta vad de gjorde, inte bara att de var klara.
- **Committa ofta.** Om en agent kör amok är `git reset --hard HEAD~1` din vän.
- **`/update` slutet av varje session.** Annars tappar du kontext mellan dagar.
- **Memory är din superpower.** Säg "kom ihåg att jag hatar Tailwind" → nästa session vet Claude det. Använd flitigt.
- **Pirlo är gratis fram till du har riktig trafik.** Plug:a in även om du bara testar — du genererar savings-data och hjälper mig validera klassificeraren.

## Vidare läsning

- [USE_CASES.md](USE_CASES.md) — konkreta projekt-idéer att börja med
- [PIRLO.md](PIRLO.md) — Pirlo-integrationen i detalj
- [VSCODE_SETUP.md](VSCODE_SETUP.md) — VSCode + Drive optimerat
