# /projectleader-session — exempel
Date: 2026-05-09
Producerat av: `/projectleader` på ett halv-byggt sales-prospect-projekt (30 sek)

> Det här är vad du får när du säger `/projectleader` mitt i ett projekt.
> Det är inte en chatt-konversation — det är en strukturerad statusöversikt
> som tar din nuvarande HANDOFF, TODOS, agent-rapporter och git-log och
> producerar en handlingsplan.

---

## Projektläge — 2026-05-09

### Var vi är

Sales-prospect-projektet är i Fas 2 (Bolagsverket-integration klar, kontaktdata-enrichment pågår). Tre agenter har delegerats senaste veckan, två klara, en pågående.

### Nyligen gjort

- Bolagsverket API-wrapper byggd och testad (Agent 5, commit `a3f9c01`) — fetchar 50 SaaS-bolag per timme inom rate-limit
- Quality-gate logik klar (Agent 6, commit `b8d2114`) — bara prospects med kontaktperson + omsättning > 5MSEK sparas
- ICP-klassificerare prompt-engineered (Agent 7, commit `c3f6a89`) — 87% accuracy mot manuell labeling

### Blockers

- **LinkedIn scraping returnerar 429** efter 50 requests/dag. Behöver rate-limit på 30-tier eller proxy-rotation. Inte blockerande för MVP men kommer bita oss innan vecka 4.

### Nästa steg (i prioritetsordning)

1. **Outreach-template-systemet** — agenten kan kvalificera men inte skriva ännu. Block:ar end-to-end-demo. Estimat: 1 dag.
2. **Slack-integration för approval-flow** — innan något skickas ut behöver Einar godkänna. Bygg som en webhook till Slack med approve/reject-knappar. Estimat: 0.5 dag.
3. **LinkedIn-rate-limit-fix** — antingen 30-tier ($79/mån) eller proxy-rotation via Bright Data ($35/mån). Säljer mig BrightData billigare och håller LinkedIn ToS-compliant. Estimat: 2 timmar.
4. **Funnel-mätning** — hur många prospects → meetings → ja? Behöver lättviktig dashboard, ev. genom Pirlo's befintliga dashboard. Estimat: 1 dag.
5. **Email warm-up** — om vi skickar från ny domän, varma upp 100 emails/dag i 2 veckor först. Pågående bakgrundsjobb. Estimat: 0 utveckling, bara körtid.

### Agent-status

- **Agent 5** (Bolagsverket API) — ✅ done, commit `a3f9c01`
- **Agent 6** (Quality gate) — ✅ done, commit `b8d2114`
- **Agent 7** (ICP-klassificerare) — ✅ done, commit `c3f6a89`
- **Agent 8** (Outreach-template-system) — ⚠️ Pågår sedan igår 14:00, ingen rapport än. Verifiera om sessionen fortfarande är aktiv.

### Vägskäl / öppna beslut

- **Ska vi använda Pirlo eller Anthropic direct för ICP-klassificeringen?** Pirlo routar Haiku-prompts till Mistral NeMo (5x billigare), men ICP-klassificering kräver good Swedish — mätning behövs. Förslag: kör Pirlo i shadow-mode 1 vecka, jämför accuracy mot direct-Anthropic.

---

## Agent-prompt — Agent 9 (om du vill starta nu, parallellt med Agent 8)

Agent 8 äger `outreach/`-mappen. Agent 9 kan parallellt göra LinkedIn-rate-limit-fixen som rör `prospecting/scraper.py` — helt oberoende fil-scope.

```
# Agent 9 — LinkedIn rate-limit fix via Bright Data residential proxies

You are working on **<projektnamn>**, ett sales-prospekt-projekt som scannar
svenska SaaS-bolag och skriver outreach. Repo: ~/Projects/sales-prospector.

## Why this work

LinkedIn returnerar 429 efter ~50 requests/dag. Vi behöver mer volym för MVP.
Bright Data residential proxies kostar ~$35/månad för 1GB trafik (mer än nog
för 500 requests/dag). LinkedIn ToS tillåter scraping av publika profiler
via proxy om du inte logger-in (vi gör inte det).

## Your task

1. Skapa Bright Data-konto. Ta första-månads-trial om möjligt.
2. Få proxy-credentials.
3. Edit `prospecting/scraper.py` för att routa LinkedIn-anrop genom Bright Data.
4. Lägg backoff-logik på 429: vänta 60s, byt proxy, retry.
5. Säkerställ att INGA andra requests (Bolagsverket, Pirlo, Slack) går genom proxy.

## Constraints
- DO NOT logga in på LinkedIn — bara scraping av publika profiler
- DO NOT inkludera Bright Data credentials i koden — använd .env

## Final step — Write agent report (mandatory)
[standard report format]
```

Klistra in i ny flik om du vill köra parallellt med Agent 8. Annars vänta tills 8 är klar.
