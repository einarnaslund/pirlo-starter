# /chatgpt-memory — Migrera ChatGPTs minne om dig till Claude

Det här kommandot extraherar vad ChatGPT vet om användaren och flyttar det till Claude Codes persistenta minne. Värdet: Claude känner användaren från första sekunden istället för att börja från noll.

Kör detta kommando under `/setup` (Steg 5) eller fristående om användaren har använt ChatGPT mycket innan.

---

## Steg 1 — Förklara vad som händer

Säg till användaren:

> "Vi ska kopiera över vad ChatGPT vet om dig. Det är gjort en gång — sen behöver du aldrig göra det igen. Räkna med 5 minuter."
>
> "Vad händer: jag ger dig en prompt. Du klistrar in den i ChatGPT (chatgpt.com), kopierar svaret, klistrar in det här. Jag organiserar det till permanent minne i `memory/`-mappen så Claude använder det automatiskt i framtida sessioner."
>
> "Är du redo?"

Vänta på OK.

---

## Steg 2 — Ge användaren prompten

Säg: "Här är prompten. Klistra in den i en NY ChatGPT-konversation (inte fortsätt på en gammal):"

Visa följande i ett code block (så användaren kan kopiera med ett klick):

````
Du hjälper mig flytta vad du vet om mig till ett nytt verktyg.
Sammanfatta allt du vet om mig från våra tidigare samtal.

Inkludera:
- Min roll och profession (jobbtitel, företag, ansvarsområden)
- Hur jag föredrar att jobba och kommunicera (ton, detaljnivå, lugnt/snabbt)
- Vilka typer av uppgifter jag oftast ber dig om
- Verktyg och teknik jag använder regelbundet
- Återkommande projekt, teman, eller intressen
- Starka preferenser jag uttryckt ("alltid X", "aldrig Y")
- Människor i mitt liv som har relevans för mitt arbete (familj, kollegor, kunder)
- Begränsningar du känner till (deadlines, budgetar, geografi, hälsa)
- Specifika fakta jag delat (företagsnamn, produkter, kundnamn, mål)

Formatera utdata som Markdown med dessa rubriker:

## Roll och profession
## Arbetsstil
## Verktyg och teknik
## Återkommande projekt
## Starka preferenser
## Människor
## Begränsningar
## Specifika fakta

Var konkret och citera mig där du kan. Hitta inte på något jag inte sagt.
Om en sektion är tom: skriv "Inget noterat" istället för att gissa.
Om jag har sagt något du anser känsligt (hälsa, finanser, relationer):
inkludera det ändå — jag kommer hantera vilka delar jag flyttar in.
Skriv på svenska om jag har skrivit på svenska, annars engelska.
````

Säg: "Kopiera prompten ovan, klistra in i ChatGPT. När du fått svaret, klistra in det här i chatten."

---

## Steg 3 — Ta emot ChatGPTs svar

Vänta tills användaren klistrar in ChatGPTs Markdown-svar.

När du fått det:

1. **Bekräfta läsbart** — om svaret är för kort (mindre än 200 tecken) eller bara säger "Inget noterat" på alla sektioner, säg "ChatGPT verkar inte ha så mycket sparat. Vi börjar med vad vi har — du kommer berätta resten under tiden."
2. **Identifiera känsligt material** — om du ser saker som personnummer, lösenord, andra människors privata info: fråga "Du har nämnt X. Vill du att jag inkluderar det i minnet eller skip:ar det?"
3. **Säg "Tack — jag organiserar nu."**

---

## Steg 4 — Konvertera till Pirlo Starter-minnesformat

Pirlo Starter använder fyra typer av minne (samma som Claude Codes auto-memory):

| Typ | Vad |
|---|---|
| **user** | Vem användaren är, hur de jobbar |
| **feedback** | Hur de vill att du ska bete dig (do's och don'ts) |
| **project** | Pågående arbeten, deadlines, kontext |
| **reference** | Pekare till externa system (Linear, Slack, GitHub-repos) |

Översätt ChatGPTs sektioner ungefär så här:

| ChatGPT-sektion | Pirlo-minnestyp | Filnamn |
|---|---|---|
| Roll och profession | user | `user_role.md` |
| Arbetsstil | feedback | `feedback_communication_style.md` |
| Verktyg och teknik | user | `user_tooling.md` |
| Återkommande projekt | project | `project_<namn>.md` (en per projekt) |
| Starka preferenser | feedback | `feedback_preferences.md` |
| Människor | user | `user_people.md` |
| Begränsningar | user | `user_constraints.md` |
| Specifika fakta | reference | `reference_facts.md` |

För **varje** sektion som har innehåll (inte "Inget noterat"):

1. Skapa `memory/<filnamn>.md` med frontmatter:
   ```markdown
   ---
   name: {{koncis titel}}
   description: {{en mening om vad detta är, så framtida-Claude vet relevans}}
   type: {{user|feedback|project|reference}}
   ---

   {{innehåll, lätt redigerat — inte rå-kopiering, utan organiserat}}

   *Migrerat från ChatGPT 2026-MM-DD via /chatgpt-memory.*
   ```

2. Lägg en pekare i `memory/MEMORY.md` (en rad per memory-fil):
   ```markdown
   - [Titel](filnamn.md) — en-mening hook som förklarar relevans
   ```

3. Kvalitetscheck:
   - Skriv inte memorial som lyder som ChatGPT-export — skriv som Claude skulle skrivit (kort, konkret, tydligt). Lätt redigerat.
   - Klipp bort fluffigt språk ("Det verkar som att...", "Du har nämnt...") och skriv direkt.
   - Om en sektion innehåller flera oberoende fakta — skapa en fil per fakta hellre än att stoppa allt i en stor fil.
   - Om något skulle vara ett **konkret task** snarare än fakta (t.ex. "jag ska skicka mejl till X imorgon") — sätt det i TODOS.md istället för memory/.

---

## Steg 5 — Visa användaren

När alla filer är skapade:

```
Klart. Jag har skapat följande memory-filer:

memory/MEMORY.md           (index över allt minne)
memory/user_role.md        — din roll
memory/feedback_*.md       — hur du vill jobba
memory/project_*.md        — projekt du driver
memory/reference_*.md      — pekare till externa system

Allt är på plats. Nästa session kommer Claude att läsa dessa
automatiskt och bete sig som om vi pratat tidigare. Du behöver
inte göra något mer.

Om du senare vill lägga till eller ta bort något: säg bara "kom
ihåg X" eller "glöm Y" — Claude uppdaterar memory/-filerna åt dig.
```

---

## Säkerhet — vad du INTE ska göra

- **Aldrig spara hemligheter** (lösenord, API-keys, kreditkort, personnummer för andra) i memory/. Om ChatGPT råkat ge dig sådant: fråga användaren om de vill ta bort det, sen ta bort.
- **Aldrig spara känslig hälso/finansinfo om andra människor** utan användarens explicita OK.
- **Aldrig pasta hela ChatGPT-export-blocket rakt in i memory/MEMORY.md** — det är ett index, inte content. Bryt ner per sektion.
- **Aldrig modifiera CLAUDE.md** baserat på ChatGPT-export. Det är en separat manuell process under `/setup` Steg 8.

---

## Felhantering

**"ChatGPT vill inte sammanfatta — säger 'jag har inget minne'"**
- Användaren använder gratis-versionen utan Memory-feature aktiverat. Säg: "OK, då börjar vi med tom slate. Du berättar för Claude under tiden så bygger den upp minnet automatiskt."

**"ChatGPT-svaret är på fel språk"**
- Be användaren be ChatGPT translate:a, eller översätt själv.

**"Användaren vill skip:a vissa sektioner"**
- Helt OK. Skapa bara filer för sektioner de godkänner.

**"Användaren vill lägga till saker som ChatGPT inte sagt"**
- Toppen — fråga, lägg till, samma format.
