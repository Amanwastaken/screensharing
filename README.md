# Screensharing

Copies of Minecraft screenshare PowerShell scripts. **Scripts only: no .exe files are downloaded or run.** Each one was reviewed before being added here, so players run a fixed, known version rather than whatever is live upstream.

All scripts are by their original authors (credited below). The only change is that they are hosted here.

## Full system (recommended)

Open **PowerShell as Administrator** (right-click Start → Terminal (Admin)) and run:

```powershell
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/AutoSS.ps1 | iex
```

Choose **[0] FULL SCAN** for everything in one go:

1. Asks once for the mods folder (Enter = find it automatically)
2. Runs all 12 built-in checks
3. Runs the extra scripts hidden in the background (Yarp + Habibi mod analyzers, Doomsday finder, Red Lotus prefetch integrity, ShellSearch, scheduled tasks), answering their prompts automatically and putting their full output into the report
4. Saves one HTML report to the Desktop with a suspicion score and the top reasons (cheat mods and Doomsday hits count heavily)
5. Offers to open Spokwn's BAM report (programs run, with signatures) in the browser

**[1] Quick scan** runs only the built-in checks. Every tool can also be run on its own from the menu:

| Key | Tool |
|---|---|
| S | Services check (lilith) |
| M | Yarp mod analyzer |
| H | Habibi mod analyzer |
| D | Doomsday client finder (lilith) |
| E | ShellSearch, suspicious PowerShell in event logs (lilith) |
| T | Scheduled tasks, unsigned + suspicious (lilith) |
| P | Prefetch integrity check (Red Lotus) |
| A | BAM parser (spokwn), opens a report in the browser |
| N | Service Enabler (lilith). **Changes services**, asks for YES |
| C | Cleanup: removes files left by older versions from `%ProgramData%\TonytheboiAutoSS` |

All tools are reviewed PowerShell scripts loaded from this repo.

Not included on purpose: all .exe tools (Spokwn's programs, System Informer, NirSoft, P1AE), so nothing compiled runs on the player's PC. P1AE was also found to contain an undisclosed MongoDB network client. Lily's Collector (adds a Defender exclusion), IHateScreensharing (rewrites system settings) and Spokwn-Collect (bulk downloads) are left out too.

## Individual scripts

Run these one at a time instead of, or alongside, the full system:

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
| AutoSS.ps1 | [tonytheboi](https://github.com/tonytheboi/Tonytheboi-Auto-SS-Tool) | Writes the HTML report to the Desktop | Loads the scripts in this repo; nothing else |
| YarpsModAnalyzer.ps1 | [YarpLetapStan](https://github.com/YarpLetapStan/PowershellScripts) | Nothing (read-only) | Sends mod hashes/names to Modrinth and megabase.vercel.app |
| DoomsdayFinder.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) (credits Zedoon) | Nothing (read-only) | None |
| ShellSearch.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | Writes a .txt to `%TEMP%`, opens Notepad | None |
| SchedulesV2.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) (based on N0LW) | Nothing (read-only) | None |
| Service-Enabler.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | **Sets chosen services to Automatic and starts them** | None |
| Services.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | Nothing (read-only) | None |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | [bacanoicua / Red Lotus](https://github.com/bacanoicua/Screenshare) | Nothing (read-only) | None |
| bamparser.ps1 | [spokwn](https://github.com/spokwn/powershells) | Writes `%TEMP%\BAMKeyEntries.html` | Report loads a Google font |
| HabibiModAnalyzer.ps1 | [HadronCollision](https://github.com/HadronCollision/PowershellScripts) | Temp folder in `%TEMP%`, deleted after | Sends mod SHA-1 hashes to Modrinth and megabase.vercel.app |
| Streams.ps1 | [spokwn](https://github.com/spokwn/powershells) | Nothing (read-only) | None |
| SuspiciousScheduler.ps1 | [N0LW](https://github.com/nolww/project-mohr) | Nothing (read-only) | None |

## Source versions

| Script | Upstream commit |
|---|---|
| AutoSS.ps1 | tonytheboi/Tonytheboi-Auto-SS-Tool@cc60349 (modified: scripts loaded from this repo, all .exe tools removed, added [0] full scan and extra scripts) |
| YarpsModAnalyzer.ps1 | YarpLetapStan/PowershellScripts@66fd1d7 (YarpsModAnalyzer6.0.ps1) |
| Services.ps1, DoomsdayFinder.ps1, ShellSearch.ps1, SchedulesV2.ps1, Service-Enabler.ps1 | praiselily/lilith-ps@ecf7280 |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | bacanoicua/Screenshare@e212b72 |
| bamparser.ps1, Streams.ps1 | spokwn/powershells@544b5f4 |
| HabibiModAnalyzer.ps1 | HadronCollision/PowershellScripts@e0175a5 |
| SuspiciousScheduler.ps1 | nolww/project-mohr@1a7df47 |

## Notes

- AutoSS treats Secure Boot being off as an automatic ban "under your server rules". That's the original author's rule; ignore it if it isn't yours.
- Results are heuristics. A flag (duplicate prefetch hash, cmd/powershell scheduled task, common cheat word in a mod) is a lead to check, not proof.
- Run the system check **before** re-enabling any services, or you lose evidence that they were off.
