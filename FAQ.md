# FAQ

Vanliga frågor om Pirlo Starter och Pirlo-proxyn. Saknas något? Öppna en [issue](https://github.com/einarnaslund/pirlo-starter/issues) — jag svarar inom 24h.

## Pirlo Starter (mallen)

### Vad är skillnaden mellan Pirlo Starter och vanlig Claude Code?

Pirlo Starter är ett *projekt-template* med inbyggd struktur. Vanlig Claude Code är bara IDE:n. Mallen ger dig:

- **Projektledare-workflow** — du säger "bygg X", den föreslår delegering till parallella agent-flikar, du läser rapporter, hen håller ordning
- **Persistent minne** — vad du gör, hur du jobbar, vilka projekt du har. Försvinner inte mellan sessioner.
- **Slash-kommandon** — `/setup`, `/research`, `/quickwin`, `/projectleader` osv. Strukturerade flöden istället för fritext.
- **ChatGPT memory-migration** — hämtar in vad ChatGPT redan vet om dig

### Måste jag använda Pirlo (proxyn) för att Pirlo Starter ska fungera?

Nej. Mallen fungerar lika bra mot raw Anthropic API. Pirlo-integrationen är bara *en config-rad* (sätt `PIRLO_API_KEY` istället för `ANTHROPIC_API_KEY` i `.claude/settings.json`).

Att använda Pirlo ger dig:
- 30-50 % lägre AI-kostnad
- Bevis-spår för EU AI Act
- Inget vendor lock-in (vi routar genom OpenAI/Anthropic/Mistral/Gemini smart)

### Är min kod / mina prompts privata?

Ja för det som ligger på din dator. Pirlo Starter skickar ingenting någonstans. Det är bara en mappstruktur.

Om du *använder Pirlo-proxyn*, då logger Pirlo varje request + response i `orchestration_logs` (det är basen i bevis-spåret som EU AI Act kräver). Vi visar inte den datan för någon annan kund. Vill du opt-out? Sätt `disable_text_storage=true` på ditt agent-konto — du tappar audit-trail då men proxyn fungerar.

### Måste jag vara utvecklare?

Nej men du behöver kunna kopiera-klistra och följa instruktioner. Räkna med 60 min första installationen om du aldrig gjort det här. Sedan tar nya projekt 30 sekunder.

### Måste jag betala något?

Nej för att börja. Claude Code har gratis basanvändning. Pirlo-proxyn ger $5 trial-credits gratis. Sen plug:ar du in din egen Claude/OpenAI-key (BYOK) och betalar dem direkt — vi tar bara % på besparingarna.

---

## Pirlo (proxyn)

### Vad är "BYOK" och varför är det viktigt?

BYOK = "Bring Your Own Key". Du behåller dina existerande OpenAI/Anthropic-konton och nycklar. Pirlo encrypterar dem och använder dem för uppströms-anrop. Du betalar Anthropic/OpenAI direkt som idag, plus en liten % av Pirlos savings till Pirlo.

Varför viktigt: du behöver inte cancellera ditt Claude-konto eller migrera billing. Pirlo är ett *infrastrukturlager ovanpå*, inte en ersättare.

### Hur tjänar Pirlo pengar?

Shared Savings: vi tar en procent av besparingarna vi genererar för dig. Sparar vi ingenting → du betalar ingenting. Inget abonnemang, inga "seats", ingen overage. Bara ren upside-alignment.

### Är min OpenAI-key säker hos er?

Ja. Vi krypterar med Fernet (master-key i Fly secret), aldrig logged eller exposed i fel-svar. Se [pirlo.fly.dev/legal/dpa](https://pirlo.fly.dev/legal/dpa).

### Vad händer om Pirlo går ner?

Trafiken stannar. Du kan när som helst byta tillbaka `base_url` till provider-direkt och fortsätta. Inga lock-ins. Det är hela poängen med proxy-modellen.

### Vad är skillnaden mot Martian / Not Diamond / OpenRouter?

Kortast version:
- **Martian** = statisk model-mapping, inget online-learning, ingen audit-trail
- **Not Diamond** = meta-model för routing, ingen EU-fokus, inget BYOK
- **OpenRouter** = ren proxy utan intelligens, ingen savings-optimering, ingen audit
- **Pirlo** = BERT-klassificerare (lär sig från trafiken), EU-jurisdiktion-medveten, full bevis-chain, BYOK default

Vi är den enda routern som är audit-ready för EU AI Act.

---

## Setup-frågor

### `/setup` säger inget händer

Du är troligen inte i Pirlo Starter-mappen. Kolla VSCodes title bar — den visar mappnamnet. Stäng och öppna mappen via `File → Open Folder...`.

### Drive-sync gör mappen okontaktbar

Drive Desktop i "stream files"-läge laddar inte ner filer förrän något öppnar dem. Två lösningar:

1. Höger-klicka på mappen i Utforskaren → "Behåll alltid på enheten"
2. Eller byt till "Mirror files" i Drive-inställningarna (Drive-ikonen → kugghjul)

### Claude Code "Sign in"-knappen gör inget

Du behöver ett Anthropic-konto. Skapa på https://claude.ai. Gratis-planen funkar för att börja.

### Hur uppdaterar jag mallen?

Idag: ladda ner ZIP:en igen och kopiera över `.claude/commands/`-mappen. Behåll dina egna `memory/`, `CLAUDE.md`, `TODOS.md`.

Senare: vi lägger upp en `update-template`-skript som syncar bara workflow-overlayen utan att röra ditt content. Säg till om det här är nödvändigt för dig så prioriterar jag det.

---

## Vidare

- Bug? → [öppna issue på GitHub](https://github.com/einarnaslund/pirlo-starter/issues)
- Allmän fråga? → DM mig på LinkedIn
- Säljfrågor / enterprise? → einar.naslund@gmail.com
