# Use Cases — Vad du kan bygga

Sju konkreta agent-projekt rangordnade efter svårighetsgrad. Varje har: vad det gör, vad det kräver, ungefär hur lång tid det tar att bygga, vilka delar som blir agent-jobb.

---

## 1. Personlig assistent — daglig sammanställare ⭐ Lättast

**Vad:** Varje morgon kl 07:30 får du en mejl med:
- Dagens kalender (parsing av ICS från Google Calendar)
- Olästa Slack-DMs sammanfattade
- Senaste GitHub-notifikationer
- Tre saker du sa du skulle göra igår men inte gjort (TODOS.md-läsning)
- Väder + pendlingstid

**Tid att bygga:** 1 dag.

**Verktyg:** Python script + cron, Pirlo för LLM-anrop, Resend för mejl-utskick. Ingen databas nödvändig.

**Agenter du delegerar:** Researcher (vilka API:er finns? Vilka är gratis?), Builder (skriv hela skriptet), Tester.

---

## 2. Sales-prospektör

**Vad:** Du beskriver din ICP (ideal customer profile). Agenten letar bolag på SCB / LinkedIn / Bolagsverket, kvalificerar enligt din ICP, hittar kontaktpersoner, skriver personifierad outreach. Du godkänner innan utskick.

**Tid att bygga:** 1 vecka.

**Verktyg:** Python + Supabase (lagrar bolag + leads), web scraping, Pirlo för LLM, Resend / Lemlist för utskick.

**Agenter:** Builder för Bolagsverket-API, Builder för LinkedIn-scraping, Builder för outreach-templates, Code reviewer.

**Tips:** Bygg quality-gate först (t.ex. "spara bara företag med kontaktperson + omsättning > X kr"). Annars drunknar du i prospects. Operator-review är ofta den verkliga flaskhalsen, inte koden.

---

## 3. Innehållsskribent / SEO-assistent

**Vad:** Du säger "skriv en blogpost om X". Agenten gör research (10 källor), skriver utkast, granskar mot SEO-checklist, optimerar bilder, postar på din WordPress / Ghost / Substack.

**Tid att bygga:** 3-5 dagar.

**Verktyg:** Pirlo för skrivande, image-generation API (DALL-E / Stable Diffusion), Headless CMS via API.

**Agenter:** Researcher för källor, Writer för utkast, SEO-reviewer, Image-prompter.

---

## 4. Kodgranskare för dina egna PRs

**Vad:** En GitHub Action som triggas när du öppnar en PR. Granskar koden, kommenterar inline med förslag, kollar mot dina personliga preferenser ("hatar du Tailwind? Den nämner inte det.").

**Tid att bygga:** 2 dagar.

**Verktyg:** GitHub Actions, GitHub API för PR-comments, Pirlo för granskning.

**Agenter:** Builder för Action-yml, Builder för review-prompt, Tester med syntetisk PR.

**Tips:** Använd `memory/feedback_*.md` som indata. Då följer reviewen dina preferenser automatiskt.

---

## 5. Möteshanterare

**Vad:** Lyssnar på dina möten (Zoom/Meet), transkriberar, sammanfattar, hittar action items, lägger dem i din TODOS / Linear / Notion. Mejlar deltagare med decision-log inom 5 min.

**Tid att bygga:** 1 vecka.

**Verktyg:** Whisper för transkription, Pirlo för sammanfattning, Calendar-API för deltagare, Linear/Notion-API för task-skapande.

**Agenter:** Builder för Whisper-pipeline, Builder för action-extractor, Builder för Linear-integrationen.

---

## 6. Research-assistent

**Vad:** Du säger "lär mig X" eller "vad ska jag tänka på inför Y". Agenten gör djupdykning: läser papers, GitHub-repos, blogposts, sammanfattar, ger dig en strukturerad rapport + 5 frågor du borde ställa nästa.

**Tid att bygga:** 2-3 dagar.

**Verktyg:** Pirlo, web-search-API (Brave / SerpApi), arxiv-API, GitHub-API.

**Agenter:** Pre-built — använd `/research`-kommandot i Pirlo Starter direkt. Bygger du egen agent för det är det mest om att specialisera scope (research om läkemedel? räkenskapsregler? AI-papers?).

---

## 7. Inkomstdeklaration-assistent ⭐ Svårast

**Vad:** Du laddar upp dina kvitton som bilder. Agenten OCR:ar, klassificerar (avdragsgillt eller inte), kategoriserar (resor, utrustning, mat), summerar, fyller i SKV-formuläret K10 / NE / etc., förbereder för signering.

**Tid att bygga:** 2-3 veckor.

**Verktyg:** OCR (Tesseract eller Anthropic Claude Vision), Pirlo, BankID för signering, SKV:s API:er (det de exponerar).

**Agenter:** Builder för OCR-pipeline, Builder för kategori-klassificerare, Builder för SKV-mappning, Builder för audit-trail (krävs av Skatteverket).

**Varning:** Skatteverket har strikta krav på audit-trail. Bygg detta som "förslag som du godkänner", inte "agent som skickar in själv".

---

## Vilken börjar du med?

Om du aldrig byggt med agenter förut: **#1 (daglig sammanställare)**. Räcker en dag, du ser nyttan, du lär dig flödet.

Om du redan kan koda: **#2-#7** beroende på vad du behöver. Sales-prospektören är högst ROI om du driver eget bolag.

Om du bygger för andra (kund, kollega): **börja med deras pain point, inte med tekniken**. Sätt dig med dem 30 min, fråga "vad gör du som tar 5+ timmar i veckan", bygg det.

## Träna Pirlo medan du bygger

Varje gång din agent gör ett LLM-anrop genom Pirlo:
- Pirlo loggar prompt + svar i `orchestration_logs`
- Pirlos klassificerare lär sig av din trafik
- Du genererar `training_examples` som gör nästa Pirlo-version smartare
- Du sparar 30-50% i kostnad direkt

Det är samma anrop du hade gjort ändå. Bara en URL byts. Vinsten är ren.

## Frågor / fastnar

- Mejla din kontakt
- Eller bygg ihop med en annan Pirlo Starter-användare i en delad VSCode Live Share
- Eller: skriv en `/research`-prompt som beskriver problemet och ta emot rapporten — det funkar förvånansvärt ofta
