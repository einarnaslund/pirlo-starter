# /setup — Förstagångs-installation av projektet

Kör detta kommando som första sak när du öppnat Pirlo Starter i Claude Code. Det går igenom en checklista och sätter upp allt du behöver. Räkna med 30-60 min första gången.

---

## Steg 1 — Hälsa och bekräfta miljö

Säg "hej" och ställ följande frågor till användaren (en i taget, vänta på svar mellan):

1. "Vad heter du? (t.ex. 'Hugo Andersson')"
2. "Vad ska projektet heta? (t.ex. 'min-personliga-assistent', 'sales-prospektor')"
3. "Vad är din mejl? (används bara lokalt, t.ex. för git config)"
4. "Vilket OS kör du? (Windows / Mac)"

Spara svaren. Du kommer behöva dem i nästa steg.

---

## Steg 2 — Verifiera att VSCode + Claude Code + git fungerar

Kör (PowerShell på Windows, bash på Mac):

```bash
git --version
```

Om kommandot misslyckas: stoppa, säg åt användaren att läsa `INSTALL.md` igen, kör inte vidare förrän git fungerar.

Om OK: säg "Git fungerar ✓".

---

## Steg 3 — Sätt git-config (om inte redan satt)

```bash
git config --global user.name "<namn från Steg 1>"
git config --global user.email "<mejl från Steg 1>"
```

Säg åt användaren: "Git är nu konfigurerad med ditt namn och mejl."

---

## Steg 4 — Initiera projektet som git-repo

```bash
git init
git add -A
git commit -m "Initial commit from Pirlo Starter"
```

Säg "Projektet är nu ett git-repo. Du har en första commit som baseline."

---

## Steg 5 — Migrera minne från ChatGPT

Detta är **det viktigaste steget**. ChatGPT vet redan mycket om hur användaren jobbar och vad de gör. Vi tar med det till Claude Code så den känner sig som hemma direkt.

Kör skill:n `chatgpt-memory` (`/chatgpt-memory`) så får användaren en prompt att skicka till ChatGPT, sen tar du tillbaka resultatet och fyller `memory/MEMORY.md` + relevanta `memory/*.md`-filer.

Den skill:n hanterar hela det flödet — du behöver bara säga "Nu kör vi minnesmigrationen — kör `/chatgpt-memory` som nästa steg."

**Vänta tills användaren har klistrat in resultatet och du har sparat memory-filerna.** Gå inte vidare till nästa steg innan dess.

---

## Steg 6 — Pirlo-konto

Fråga: "Vill du sätta upp Pirlo nu? Det ger dig 30-50% billigare Claude/GPT-4-anrop. Du kan hoppa över och göra senare om du vill, men då routar dina anrop direkt till Anthropic till fullpris."

**Om JA:**
1. "Gå till https://pirlo.fly.dev/ och klicka 'Get started'"
2. "Fyll i namn, mejl, företag (eller 'personal'). Om du har en invite code — skriv in den."
3. "På steg 2 kan du klistra in din OpenAI-key och/eller Anthropic-key för BYOK. Eller skip:a för $5 trial-credits."
4. "På steg 3 får du en `sk_pirlo_...`-key. Kopiera den och säg 'klar' till mig."

När användaren säger "klar":

5. Be dem klistra in keyn
6. Skapa eller uppdatera `.claude/settings.json` med:
   ```json
   {
     "env": {
       "PIRLO_API_KEY": "<deras-key>"
     }
   }
   ```
7. Säg "Klart. Nu routar Claude Code-anrop genom Pirlo."

**Om NEJ:**
- Säg "OK, vi hoppar över. Du kan göra det senare genom att läsa `PIRLO.md`."

---

## Steg 7 — Drive-sync (om Drive Desktop installerat)

Fråga: "Har du Google Drive Desktop installerat?"

**Om JA:**
- Kontrollera att projekt-mappen ligger inom Drive-paddan: `C:\Users\<namn>\Min enhet\` eller `~/Google Drive/`
- Om inte: föreslå att flytta. "Drag mappen i Finder/Utforskaren från där den ligger till `Min enhet\Projects\`. VSCode och Claude Code kommer fortfarande hitta den när du öppnar."
- Säg "Bra — backup är ordnad automatiskt nu."

**Om NEJ:**
- Säg "OK, du kan installera senare. Se INSTALL.md sektion 4."

---

## Steg 8 — Skapa CLAUDE.md för det här projektet

CLAUDE.md är projektets master-doc. Säg "Nu fyller vi CLAUDE.md med ditt projekts kontext."

Fråga 3-4 frågor:

1. "Vad ska projektet göra? Beskriv i 2-3 meningar."
2. "Vilka är de tre viktigaste sakerna projektet ska klara?"
3. "Vilka begränsningar / no-gos har du? (t.ex. 'får aldrig spam:a folk', 'får bara köra på söndagar')"
4. "Vilka teknik-val är låsta? (t.ex. 'Python', 'måste fungera på Windows')"

Skriv ihop svaren till en CLAUDE.md med sektionerna:
- `## Projektmål`
- `## Strategiska beslut`
- `## Tekniska beslut`
- `## Filer och dokument` (lämna med placeholder för senare)

Spara över den nuvarande placeholder-CLAUDE.md.

---

## Steg 9 — Sätt upp första TODO

Skriv en TODOS.md med:

```markdown
# {{Projektnamn}} — Byggkö

> Första task från /setup. Uppdatera denna fil löpande.

## Nästa steg

- ⬜ Bygg [första konkreta sak] — [varför, från CLAUDE.md]
- ⬜ Sätt upp [verktyg du behöver]

## Senare

- ⬜ ...
```

Använd användarens projektmål för att hitta första task.

---

## Steg 10 — (Valfritt) Supabase / Railway

Fråga: "Ska din agent ha ett backend (databas, API)?"

**Om JA:**
- "Vill du sätta upp Supabase + Railway nu, eller senare?"
- Om NU: läs `AGENT_BUILDING.md` sektion "Backend Setup" och guida igenom.
- Om SENARE: säg "OK, läs `AGENT_BUILDING.md` när du är redo."

**Om NEJ:**
- Säg "Bra — du behöver bara Claude Code + Pirlo. Vi kan alltid lägga till backend senare."

---

## Steg 11 — Test-kör en agent

Föreslå: "Vill du köra ditt första agent-uppdrag nu? Jag kan delegera ett enkelt research-task som värmer upp dig på flödet."

**Om JA:**
- Kör `/research <något användaren bryr sig om>` (t.ex. "best practice för X" där X kommer från deras projektmål)
- Vänta på rapport
- Visa hur de hittar rapporten i `agent_reports/`
- Säg "Det här är hur du jobbar fram nu. Du säger 'gör X', jag delegerar, du läser rapporten."

**Om NEJ:**
- Säg "OK. När du är redo, prova `/projectleader` för att få en överblick."

---

## Steg 12 — Säg klart

Avsluta med:

```
Setup klar! Nästa gång du börjar en session: kör /start.
När du avslutar en session: kör /update.
För projektöversikt: kör /projectleader.

Lista över allt: läs filerna i .claude/commands/

Lycka till. Räkna med att du behöver en vecka för att vänja dig vid flödet,
sen kommer du aldrig vilja jobba utan det.
```

---

## Var noga med

- **Hoppa inte över Steg 5 (ChatGPT-migration).** Det är vad som gör att Claude känner användaren från första prompten — utan det blir varje session som att starta om.
- **Mata inte över för många frågor i taget.** Vänta på svar mellan varje fråga. Det är 60 min onboarding, inte ett jobbintervju.
- **Berätta varför du gör varje steg.** Användaren ska förstå systemet, inte bara följa det.
- **Om något misslyckas (git, Pirlo, etc.):** stanna, fixa, fortsätt. Ingen mening att gå vidare med en bruten setup.
