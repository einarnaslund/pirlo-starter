# Agent Reports — Konventioner

Den här mappen är där delegerade agenter lämnar sina rapporter. Varje gång du skickar iväg en agent i en ny Claude Code-flik, ber du dem att skriva en rapport hit som sista steg.

## Struktur

| Fil | Vad |
|---|---|
| `LAUNCH_LOG.md` | Index över alla agenter du startat. En rad per agent. |
| `agent_N_YYYY-MM-DD.md` | En rapport per startad agent. Skapas av agenten själv som sitt sista steg. |

## Rapport-format (kopiera in i agent-prompten)

````
# Agent N — [kort beskrivning]
Date: YYYY-MM-DD
Status: done | partial | blocked

## Accomplished
- [konkret sak 1]
- [konkret sak 2]

## Files changed
- path/to/file.py — vad som ändrades
- ...

## Test results
- [pytest output eller liknande]

## Errors / blockers
- [vad som inte fungerade]

## Next session needs to know
- [vad nästa person/agent måste vara medveten om]
````

## När du tar emot en rapport

1. **Läs rapporten** — inte bara "klar/inte klar". Vad gjorde de faktiskt?
2. **Uppdatera `LAUNCH_LOG.md`** — flytta status från `🟡 in flight` till `✅ done` eller `🔴 blocked`
3. **Verifiera tester** — om agenten säger "tester gröna", kör själv: `python -m pytest tests/`
4. **Committa** om allt ser bra ut: `git add <agentens filer>; git commit -m "Agent N: [vad]"`

## Vad du gör om en agent rapporterar `partial` eller `blocked`

- **partial:** läs vad som saknas, designa en uppföljningsagent eller fixa själv
- **blocked:** läs blocker:n, är det miljö-problem (CLI saknas), API-key missing, eller spec-fel? Lös root cause innan du försöker igen.

## Vad du gör om en agent **inte** rapporterar tillbaka

- Sessionen kan ha tappats, eller usage tog slut. Kolla agentens flik om den fortfarande är öppen.
- Om filerna finns i ditt working tree (du ser dem i `git status`): kör tester och verifiera. Skriv en kort rapport själv om vad du hittar.
- Om inga filer finns: starta om agenten i en ny flik med samma prompt.

## Varför detta system finns

Agent-projekt utan rapport-disciplin blir oöverskådliga inom en vecka. Du tappar bort vad agenter gjorde, deras antaganden, deras blockers. Det här systemet kostar dig 30 sekunder per agent och sparar timmar av re-debugging.

Var inte slarvig med rapport-formatet. Låt aldrig en agent säga "klar" utan att skriva en. Det är hur du behåller kontrollen när du har 5 agenter igång parallellt.
