# Installation — Windows + Mac

Räkna med 30-60 min första gången. Du behöver bara göra detta en gång — sen tar du nya projekt genom att kopiera mappen.

## Vad du installerar

1. **VSCode** — kodredigerare (gratis, från Microsoft)
2. **Claude Code** — Anthropics AI-assistent som plugin i VSCode (gratis basanvändning)
3. **Git** — versionshantering (gratis)
4. **Google Drive Desktop** *(valfritt men rekommenderat)* — så filerna synkas
5. **Python 3.12** *(valfritt)* — om du ska bygga Python-agenter

---

## Windows

### 1. VSCode

- Gå till https://code.visualstudio.com/download
- Klicka "Windows" → installern laddas ner
- Kör installern, klicka "Next" på alla steg utom: kryssa **"Add to PATH"**
- Starta VSCode

### 2. Claude Code

- I VSCode, klicka extensions-ikonen i sidofältet (eller `Ctrl + Shift + X`)
- Sök "Claude Code"
- Klicka "Install" på extensionen från Anthropic
- Du behöver ett Anthropic-konto. Klicka "Sign in" när prompted, gå genom flödet på claude.ai
- Plan: gratis plan funkar för att börja. Du kan uppgradera senare.

### 3. Git

Öppna PowerShell (sökfältet → "PowerShell" → enter) och kör:

```powershell
winget install --id Git.Git -e
```

Stäng och öppna PowerShell igen, verifiera:

```powershell
git --version
```

Ska visa något i stil med `git version 2.45.x`.

### 4. Google Drive Desktop *(valfritt)*

- Gå till https://www.google.com/drive/download/
- Ladda ner "Drive for desktop"
- Logga in med ditt Google-konto
- I inställningarna: aktivera "Stream files" eller "Mirror files" — välj **Mirror** om du har plats (snabbare, alltid offline)
- Vänta tills "Min enhet" / "My Drive" har synkat. Den ligger normalt på `C:\Users\<ditt-namn>\Min enhet\`

### 5. Python *(valfritt)*

```powershell
winget install --id Python.Python.3.12 -e
```

Verifiera:

```powershell
python --version
```

---

## Mac

### 1. VSCode

- https://code.visualstudio.com/download → klicka "Mac"
- Drag VSCode.app till Applications-mappen
- Starta VSCode
- I VSCode: `Cmd + Shift + P` → "Shell Command: Install 'code' command in PATH"

### 2. Claude Code

Samma som Windows ovan — extensions-ikonen → sök "Claude Code" → Install.

### 3. Git

Öppna Terminal (Cmd + Space → "Terminal"). Skriv:

```bash
git --version
```

Om git inte finns säger Mac:en att den vill installera Xcode Command Line Tools. Klicka "Install". Vänta 5-10 min.

Eller via Homebrew (om du har det):

```bash
brew install git
```

### 4. Google Drive Desktop *(valfritt)*

https://www.google.com/drive/download/ → välj Mac. Installation och inloggning. Drive ligger normalt på `~/Google Drive/` eller `~/Library/CloudStorage/GoogleDrive-<din-mejl>/`.

### 5. Python *(valfritt)*

```bash
brew install python@3.12
```

---

## Verifiering — kolla att allt fungerar

Öppna VSCode. I terminalen (`Ctrl/Cmd + ` ` `):

```
git --version       # ska visa version
code --version      # ska visa VSCode-version
```

Klicka Claude-loggan i sidofältet → ska visa "Claude Code: Ready" eller liknande.

---

## Packa upp Pirlo Starter

1. Du har fått en zip-fil (`pirlo-starter.zip`) via mejl/Drive/Slack
2. **Bestäm var dina projekt ska bo:**
   - Windows: typiskt `C:\Users\<ditt-namn>\Min enhet\Projects\` eller `C:\Users\<ditt-namn>\Documents\Projects\`
   - Mac: typiskt `~/Google Drive/Projects/` eller `~/Documents/Projects/`
   - Tips: lägg det i Drive om du har Drive Desktop — då har du backup automatiskt
3. **Skapa en mapp för det här projektet** — t.ex. `min-personliga-assistent/`
4. **Packa upp ZIP:en in i den mappen** — du ska se `README.md`, `.claude/`, etc. direkt i mappen (inte i en undermapp)
5. **Öppna mappen i VSCode** — `File → Open Folder...` → välj din nya mapp
6. **Starta Claude Code** — klicka loggan i sidofältet
7. **Skriv `/setup`** i chatten — `/setup`-kommandot tar dig genom resten

---

## Felsökning

**"Claude Code säger 'unauthenticated'"**
- Klicka loggan → "Sign in" → följ flödet på claude.ai
- Du måste ha ett Anthropic-konto. Skapa på claude.ai om du saknar.

**"git inte erkänt som kommando" (Windows)**
- Du behöver stänga och öppna PowerShell efter `winget install`. PATH uppdateras inte i öppna fönster.

**"Mappen finns inte i Drive"**
- Drive Desktop kan ta 10-30 min på första synket. Vänta. Eller kolla `Drive` istället för `Min enhet`.

**Slash-kommandon (`/setup`, `/start`) gör inget**
- Verifiera att du faktiskt är i Pirlo Starter-mappen (inte i någon annan mapp). VSCode visar mappnamnet uppe vänster.
- Kolla att `.claude/commands/setup.md` finns. Om inte: din ZIP packades inte upp korrekt.

**VSCode öppnar inte mappen**
- `File → Open Folder...` (inte "Open File"). Du ska välja MAPPEN, inte en fil i den.

**Något annat krånglar**
- Mejla din kontakt med en skärmdump.

---

Efter `/setup` är du klar att börja. Läs [README.md](README.md) → "Vad ska du bygga?" för idéer.
