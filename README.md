# Pirlo Starter

> En projekt-mall för att bygga AI-agenter — med projektledare, persistent minne, slash-kommandon och färdig integration mot [Pirlo](https://pirlo.fly.dev) (AI-proxy som sparar 30-50 % på dina Claude/GPT-4-anrop).

**[👉 Klicka "Use this template" ovan för att skapa din egen kopia](https://github.com/new?template_name=pirlo-starter&template_owner=einarnaslund)**

Eller klona direkt: `git clone https://github.com/einarnaslund/pirlo-starter.git ditt-projekt`

---

Det här är en projektmapp som ger dig en superpower: en **personlig stab av AI-agenter** som du delegerar arbete till, med automatiskt minne, projektledning och rapportering. Den är förkonfigurerad för att routa alla AI-anrop genom **Pirlo** — en intelligent AI-proxy som gör att du betalar 30-50 % mindre för Claude/GPT-4 utan att tappa kvalitet.

Du behöver inte vara utvecklare. Du behöver kunna följa instruktioner och kopiera-klistra. Räkna med 60 minuter första gången, sen tar du nya projekt på 30 sekunder.

## Vad du får

- **Projektledare som hanterar dina agenter** — du säger "bygg X", den föreslår uppdelning i delegerade tasks, du startar dem i parallella flikar.
- **Persistent minne** — vad du gör, hur du jobbar, vilka projekt du har. Försvinner inte mellan sessioner.
- **Slash-kommandon** — `/start`, `/projectleader`, `/explain`, `/research`, `/quickwin`, `/update` osv. Sätter strukturen som proffsare använder.
- **Subagenter** — researcher, builder, code-reviewer. Skickar du iväg en agent får du tillbaka en rapport, inget gnäll.
- **Pirlo-koppling** — billigare och bevis-spår-baserade AI-anrop, ingen vendor-lockin.

## Kom igång (i den här ordningen)

1. **Läs [INSTALL.md](INSTALL.md)** — installerar VSCode, Claude Code och git om du inte har dem. Säg till om något krånglar.
2. **Öppna den här mappen i VSCode** — `File → Open Folder...`
3. **Starta Claude Code** — klicka på Claude-loggan i sidofältet eller tryck `Ctrl/Cmd + Shift + P` → "Claude Code: Start"
4. **Skriv `/setup`** i Claude-chatten — den går igenom resten med dig steg-för-steg.

Om du fastnar: läs [INSTALL.md#felsökning](INSTALL.md#felsökning) eller mejla `<din kontakt>`.

## Vad ska du bygga?

Idéer i [USE_CASES.md](USE_CASES.md). Exempel:

- **Personlig assistent** — sammanfattar mejl, prioriterar, skriver utkast
- **Sales-prospektör** — letar bolag enligt din ICP, gör outreach
- **Innehållsskribent** — drar ihop research → utkast → granskning
- **Kodgranskare** — läser dina PRs och kommenterar
- **Daglig sammanställare** — kompilerar status från kalender + Slack + Linear
- **Research-assistent** — gör djupgående analyser och skriver rapporter
- **Inkomstdeklaration-assistent** — drar ihop bokföring → SKV-format

## Vad kostar det?

- **Claude Code** själva: gratis för basanvändning, betalas via Anthropic-konto vid större volym.
- **Pirlo (AI-proxy)**: $5 trial-credit gratis. Sen plug:ar du in din egen Anthropic/OpenAI-key (BYOK) — Pirlo tar bara % av besparingarna du gör.
- **Supabase / Railway** (om du bygger backend-agenter): gratis fram till du har riktig trafik.

Allt går att köra utan att betala något i början.

## Vidare läsning

- [PIRLO.md](PIRLO.md) — vad Pirlo är + hur du pluggar in
- [AGENT_BUILDING.md](AGENT_BUILDING.md) — hur agent-workflowen funkar + hur du bygger nya
- [USE_CASES.md](USE_CASES.md) — konkreta projekt-idéer
- [VSCODE_SETUP.md](VSCODE_SETUP.md) — VSCode + Drive-sync + Claude Code optimerat

---

*Kräver: VSCode + Claude Code (extension) + Git + ett Pirlo-konto. INSTALL.md guidar dig genom alla.*
