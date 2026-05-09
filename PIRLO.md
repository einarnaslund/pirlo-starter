# Vad är Pirlo?

Pirlo är en **AI-proxy** som sitter mellan din app och AI-leverantörer (OpenAI, Anthropic, Mistral, Gemini). Du byter en URL i din kod, allt annat är samma. Pirlo:

- **Routar varje request till den billigaste modellen** som klarar uppgiften (Claude Sonnet för kod, Mistral för enkla frågor, etc). Sparar typiskt 30-50% utan kvalitetstapp.
- **Lagrar fullt audit-spår** av vad som skickades och vad som svarades — för EU AI Act-compliance och fault-attribution.
- **Klassificerar känslig data** och routar den ALDRIG ut ur EU. Sensitiv text → Mistral i Stockholm, aldrig OpenAI i Texas.

## Hur du pluggar in

### Alternativ A: Bring Your Own Key (BYOK) — rekommenderat

Du behåller dina existerande Claude/OpenAI-konton och nycklar. Pirlo encrypterar dina nycklar och använder dem för uppströms-anrop. Du betalar Anthropic/OpenAI direkt som idag, plus en liten % av Pirlos savings till Pirlo.

```python
# Innan:
from openai import OpenAI
client = OpenAI(api_key="sk-...")  # din egen OpenAI key

# Med Pirlo BYOK:
from openai import OpenAI
client = OpenAI(
    api_key="sk_pirlo_...",        # din Pirlo-key (gratis att skapa)
    base_url="https://pirlo.fly.dev/v1"
)
# Sen — efter du laddat upp din OpenAI-key till Pirlo en gång —
# använder Pirlo din egen OpenAI-key för uppströms-anrop. Du fakturerar
# OpenAI direkt som vanligt. Pirlo tar bara % av besparingarna.
```

### Alternativ B: Pirlo trial credits (snabbast att testa)

Du får $5 trial-credits direkt vid signup. Pirlo använder sina egna provider-nycklar tills du har förbrukat $5, sen 402 Payment Required tills du lägger in egna nycklar.

Bra för att verifiera att integrationen fungerar utan att skapa konton hos OpenAI/Anthropic först.

## Signa upp

1. Gå till https://pirlo.fly.dev/
2. Klicka "Get started"
3. Fyll i namn, email, företag (eller "personal")
4. **Har du en invite code?** Skriv in den (om du fått en av Einar/team) — ger dig $50 trial-credits istället för $5
5. Bocka i ToS
6. *(Valfritt)* Paste:a din OpenAI-key och/eller Anthropic-key (Pirlo encrypterar dem)
7. Du får din `sk_pirlo_...`-key — **kopiera den nu**, den visas bara en gång

## Använd din key

I `.claude/settings.json` lägger du:

```json
{
  "env": {
    "PIRLO_API_KEY": "sk_pirlo_..."
  }
}
```

Eller exportera som env-var i ditt shell.

## Vad får du för pengarna?

- **Savings**: 30-50% lägre Claude/GPT-4-kostnader
- **EU-compliance**: bevis-spår, GDPR, AI Act Art. 13
- **Routing-intelligens**: Pirlo's klassificerare lär sig vilka modeller som passar din trafik
- **Audit-trail**: full prompt + response loggad, exporteras som PDF för revisor
- **Risk-scoring**: nattlig analys av agent-beteende, flaggor för avvikelser

Du kan se allt detta på din dashboard: https://pirlo.fly.dev/dashboard (paste:a din `sk_pirlo_`-key)

## Vad är haken?

- Pirlo är en startup. Vi har <10 kunder. Buggar händer. Vi fixar snabbt.
- Vi lagrar dina prompts + responses default. Det är basen i vårt audit-trail-erbjudande. Om du har use case som inte tål det — säg till, vi har en `disable_text_storage`-toggle.
- Trial-tier är begränsad. För riktig produktion: BYOK + invite-code för $50 startbonus eller kontakta oss.

## Frågor

- "Är min OpenAI-key säker hos er?" — Ja. Vi krypterar med Fernet (master-key i Fly secret), aldrig logged eller exposed i fel-svar. Se [legal/dpa](https://pirlo.fly.dev/legal/dpa).
- "Kan jag stänga av prompt-storage?" — Ja, sätt `disable_text_storage=true` på ditt agent-konto. Du tappar audit-trail då men annars fungerar allt.
- "Vad händer om Pirlo går ner?" — Trafiken stannar. Du kan när som helst byta tillbaka `base_url` till provider-direkt och fortsätta. Inga lock-ins.

## Hur tjänar Pirlo pengar?

Shared Savings: vi tar en procentsats av savings vi genererar för dig. Sparar vi ingenting → du betalar ingenting. Inget abonnemang, inga "seats", ingen overage. Bara ren upside-alignment.

För detaljer: läs [pirlo.fly.dev/legal/terms](https://pirlo.fly.dev/legal/terms) eller öppna en issue på [GitHub-repot](https://github.com/einarnaslund/pirlo-starter/issues).
