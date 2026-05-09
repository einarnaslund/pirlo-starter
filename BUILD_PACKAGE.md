# How to package this for distribution

This file is for the *author* (Einar / whoever maintains the starter pack), not the end user. It explains how to bundle the starter into a ZIP for distribution via email, Drive, or Slack.

## What goes into the ZIP

Everything in `pirlo-starter/` **except**:

- `BUILD_PACKAGE.md` (this file — internal)
- Any `.git/` folder (if one exists; ZIP should be a fresh-init experience for the recipient)
- `.DS_Store`, `Thumbs.db` (OS clutter)
- Any populated `memory/*.md` (other than the README and empty MEMORY.md template) — those would leak whoever the original author was
- Any populated `agent_reports/agent_*.md` — same reason
- Any populated `HANDOFF.md` content beyond the template — same
- Any populated `TODOS.md` beyond the template — same

## PowerShell (Windows) — recommended

Run from the **parent** of `pirlo-starter/`:

```powershell
# Clean any author-specific artefacts before zipping
Remove-Item -Recurse -Force pirlo-starter\.git -ErrorAction SilentlyContinue
Get-ChildItem pirlo-starter\agent_reports\agent_*.md -ErrorAction SilentlyContinue | Remove-Item -Force
Get-ChildItem pirlo-starter\memory\*.md -Exclude README.md, MEMORY.md -ErrorAction SilentlyContinue | Remove-Item -Force

# Zip
$dest = "$env:USERPROFILE\Desktop\pirlo-starter.zip"
Compress-Archive -Path pirlo-starter\* -DestinationPath $dest -Force
Write-Host "Wrote $dest"
```

## bash (Mac/Linux)

```bash
# Clean
rm -rf pirlo-starter/.git
rm -f pirlo-starter/agent_reports/agent_*.md
find pirlo-starter/memory/ -name "*.md" ! -name "README.md" ! -name "MEMORY.md" -delete

# Zip (the -X flag strips Mac extended attributes)
cd pirlo-starter && zip -r -X ../pirlo-starter.zip . -x "*.DS_Store" && cd ..
```

## Sanity-check before sending

Unzip into a fresh folder, then:

1. Open in VSCode
2. Open Claude Code
3. Run `/setup`
4. Walk through the first 3 steps as if you were a new user

If anything feels broken or jargon-heavy, fix it in the source `pirlo-starter/` and re-zip.

## Distribution channels

| Channel | When |
|---|---|
| **Email attachment** | One-off (sending to a single friend). Personal touch. |
| **Drive shared link** | Internal team or company. Editable; you can update the file and they get the new version. |
| **Slack message** | Team announcement with a Drive link. |
| **GitHub release** | When ready, push the unzipped contents to `github.com/<your-org>/pirlo-starter` as a release zip. Allows version-tracking. |

## When you update the starter

The end user does **not** auto-pull updates. They get a frozen snapshot. If you ship a v2:
1. Re-bundle a new ZIP from the latest `pirlo-starter/`
2. Re-send via the same channel (or post in Slack: "v2 here, copy your `memory/` and `CLAUDE.md` over")
3. Note the version in the README (consider `VERSION` file in the root)

For now, no version-tracking — keep it simple. Iterate the source freely; re-zip when you have a meaningful improvement.
