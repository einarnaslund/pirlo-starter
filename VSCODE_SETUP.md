# VSCode + Claude Code + Drive — Optimal setup

Det här är finputs efter du följt `INSTALL.md`. Inte krav, men gör vardagen smidigare.

## VSCode-inställningar att slå på

`File → Preferences → Settings` (eller `Cmd ,` på Mac):

- **Auto Save:** `onFocusChange` — sparar automatiskt när du klickar bort
- **Format On Save:** `true` — kör code-formatter när du sparar
- **Editor: Word Wrap:** `on` — Markdown blir läsbart
- **Files: Trim Trailing Whitespace:** `true` — git-historiken blir renare
- **Terminal: Default Profile (Windows):** `PowerShell` (inte cmd)

Dessa kan också läggas i `.vscode/settings.json` i projekt-mappen om du vill att de följer projektet.

## Tema och typsnitt

Anti-AI-slop-vänlig setup som inte tröttar ögonen:

- **Tema:** "GitHub Light Default" eller "Atom One Light" (inte purple/Dracula)
- **Font:** `JetBrains Mono` eller `IBM Plex Mono` — installera från fonter-katalogen
- **Font size:** 13-14
- **Line height:** 1.5

## Drive-sync — så det funkar friktionsfritt

### Windows + Drive Desktop

1. Drive Desktop installerat (se `INSTALL.md` Steg 4)
2. I `Drive for Desktop`-inställningarna: välj **Mirror files** (snabbare, alltid offline)
3. Drive synkar då till `C:\Users\<namn>\Min enhet\` (eller `C:\Users\<namn>\Drive\` beroende på språk)
4. Skapa en `Projects/`-mapp där: `C:\Users\<namn>\Min enhet\Projects\`
5. Lägg alla projekt-mappar där. Då har du:
   - Backup automatiskt (Drive)
   - Sync mellan datorer (om du har flera)
   - Versioner via Drive ("File → Manage versions") om du behöver rulla tillbaka

### Mac + Drive Desktop

Samma princip. Drive ligger ofta på `~/Library/CloudStorage/GoogleDrive-<email>/Min enhet/`. Skapa `Projects/` där.

Symlink-tipp om du tycker pathen är jobbig:

```bash
ln -s "~/Library/CloudStorage/GoogleDrive-<email>/Min enhet" ~/Drive
```

Då kan du `cd ~/Drive/Projects/min-app` istället för full-path.

### Förebygg sync-konflikter

- Stäng VSCode innan du shutdown:ar datorn
- Ha **inte** två datorer öppna i samma fil samtidigt (Drive löser det inte alltid)
- Om Drive säger "kan inte synka" — kolla att inte VSCode har låst filen (stäng VSCode + öppna igen)

## Claude Code — viktiga inställningar

### `.claude/settings.json` per projekt

Pirlo Starter kommer med en bra default. Redigera om du behöver. Viktiga fält:

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "allow": ["Bash(git:*)", "Bash(python:*)", "WebSearch(*)"]
  },
  "env": {
    "PIRLO_API_KEY": "sk_pirlo_..."
  }
}
```

`defaultMode: "bypassPermissions"` betyder Claude får köra kommandon som matchar `allow`-listan utan att fråga. Default är säker (du blir tillfrågad varje gång) men trögt.

### Globala inställningar — `~/.claude/settings.json`

Här lägger du saker som ska gälla alla projekt:

```json
{
  "model": "claude-sonnet-4-6",
  "outputStyle": "concise",
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-..."
  }
}
```

(Eller använd `PIRLO_API_KEY` istället så routar dina anrop genom Pirlo. Spara 30-50%.)

### Slash-kommandon globalt vs projekt

- **Projekt-specifika** (`<projekt>/.claude/commands/`) — t.ex. `/setup`, `/projectleader`, `/explain`. Följer med ZIP-mallen.
- **Globala** (`~/.claude/commands/`) — t.ex. ditt personliga `/journal`, `/standup`. Funkar i alla projekt.

Tips: lägg dina mest använda i globala mappen så du har dem direkt även i projekt utan Pirlo Starter.

## VSCode-extensions som spelar med agent-workflow

- **Markdown All in One** — TOC + preview för dina docs
- **GitLens** — se git blame inline (förstår varför kod ser ut som den gör)
- **Better Comments** — färgar `// TODO`, `// FIXME` så du ser dem
- **Path Intellisense** — auto-complete för file paths (bra när du skriver agent-prompter)
- **vscode-icons** — tydligare mappikoner

Ingen av dessa är krav. Lägg till över tid.

## Kortkommandon värda att lära sig

| Mac | Windows | Vad |
|---|---|---|
| `Cmd P` | `Ctrl P` | Öppna fil snabbt |
| `Cmd Shift P` | `Ctrl Shift P` | Command palette (allt i VSCode) |
| `Cmd Shift F` | `Ctrl Shift F` | Sök i alla filer |
| `Cmd /` | `Ctrl /` | Kommentera rad |
| `Ctrl Shift L` | `Ctrl Shift L` | Markera alla förekomster av samma ord |
| `Cmd Shift K` | `Ctrl Shift K` | Ta bort rad |
| `Alt ↑/↓` | `Alt ↑/↓` | Flytta rad upp/ner |

## Vanliga problem

**Claude Code "tappar" mappen mitt i sessionen**
- Hände när Drive synkar och VSCode tappar bort filen. Lös: `File → Revert File` eller stäng/öppna mappen igen.

**Tester körs men output kommer inte upp**
- VSCodes terminal har ibland buffer-problem. Lös: öppna terminal (`Ctrl ` ` `) och kör manuellt. Eller installera "Terminal Tabs" extension.

**`/setup` säger inget händer**
- Du är troligen inte i Pirlo Starter-mappen. Kolla VSCodes title bar — den visar mappnamnet.

**Claude Code tar för lång tid att svara**
- Plug:a in Pirlo-keyn — den routar smartare modeller på enkla saker. Spara tid + pengar.

---

Klar. Du har nu en setup som proffsen använder. Resten är att börja bygga.
