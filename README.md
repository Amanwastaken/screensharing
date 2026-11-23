# Screensharing

Copies of Minecraft screenshare PowerShell scripts. Each one was reviewed before being added here, so players run a fixed, known version rather than whatever is live upstream.

All scripts are by their original authors (credited below). The only change is that they are hosted here.

## How to run

Open **PowerShell as Administrator** (right-click Start → Terminal (Admin)) and run these one at a time:

```powershell
# 1) System check: services, logs, prefetch, recycle bin, history
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/Services.ps1 | iex

# 2) Prefetch integrity
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/RedLotusPrefetchIntegrityAnalyzer.ps1 | iex

# 3) Recently run programs (BAM), opens an HTML report
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/bamparser.ps1 | iex

# 4) Mod analyzer: press Enter for default .minecraft\mods, or paste the launcher's mods path
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/HabibiModAnalyzer.ps1 | iex

# 5) Downloaded files / alternate data streams (answer y)
cd $env:USERPROFILE\Downloads
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/Streams.ps1 | iex

# 6) Suspicious scheduled tasks
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/SuspiciousScheduler.ps1 | iex
```

If a script gives a "missing terminator" error, use this form instead (replace URL):

```powershell
iex ([Text.Encoding]::UTF8.GetString((iwr URL -UseBasicParsing).RawContentStream.ToArray()))
```

## What each script does

| Script | Author | Changes | Network |
|---|---|---|---|
| Services.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | Nothing (read-only) | None |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | [bacanoicua / Red Lotus](https://github.com/bacanoicua/Screenshare) | Nothing (read-only) | None |
| bamparser.ps1 | [spokwn](https://github.com/spokwn/powershells) | Writes `%TEMP%\BAMKeyEntries.html` | Report loads a Google font |
| HabibiModAnalyzer.ps1 | [HadronCollision](https://github.com/HadronCollision/PowershellScripts) | Temp folder in `%TEMP%`, deleted after | Sends mod SHA-1 hashes to Modrinth and megabase.vercel.app |
| Streams.ps1 | [spokwn](https://github.com/spokwn/powershells) | Nothing (read-only) | None |
| SuspiciousScheduler.ps1 | [N0LW](https://github.com/nolww/project-mohr) | Nothing (read-only) | None |

## Source versions

| Script | Upstream commit |
|---|---|
| Services.ps1 | praiselily/lilith-ps@ecf7280 |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | bacanoicua/Screenshare@e212b72 |
| bamparser.ps1, Streams.ps1 | spokwn/powershells@544b5f4 |
| HabibiModAnalyzer.ps1 | HadronCollision/PowershellScripts@e0175a5 |
| SuspiciousScheduler.ps1 | nolww/project-mohr@1a7df47 |

## Notes

- Results are heuristics. A flag (duplicate prefetch hash, cmd/powershell scheduled task, common cheat word in a mod) is a lead to check, not proof.
- Run the system check **before** re-enabling any services, or you lose evidence that they were off.
