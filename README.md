# Screensharing

Copies of Minecraft screenshare PowerShell scripts. Each one was reviewed before being added here, so players run a fixed, known version rather than whatever is live upstream.

All scripts are by their original authors (credited below). The only change is that they are hosted here.

## Full system (recommended)

Open **PowerShell as Administrator** (right-click Start → Terminal (Admin)) and run:

```powershell
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/AutoSS.ps1 | iex
```

Choose **[1]** to run all checks. It saves an HTML report with a suspicion score and reasons. The menu also launches extra tools:

| Key | Tool | How it's trusted |
|---|---|---|
| S | Services check (lilith) | Reviewed copy in this repo |
| M | Yarp mod analyzer | Reviewed copy in this repo |
| H | Habibi mod analyzer | Reviewed copy in this repo |
| J | P1AE javaw memory scanner v1.12 | Pinned SHA-256 (open source, unsigned) |
| P | Spokwn Prefetch Parser v1.5.5 | Pinned SHA-256 (signed by espouken) |
| A | Spokwn BAM Parser v1.2.9 | Pinned SHA-256 (signed by espouken) |
| W / L | NirSoft WinPrefetchView / LastActivityView | Must be validly signed by Nir Sofer |
| K | System Informer 4.0.26241.138 (portable) | Pinned SHA-256 + must be signed by Winsider |
| C | Cleanup | Removes downloaded tools from `%ProgramData%\TonytheboiAutoSS` |

Downloaded programs are refused and deleted if the fingerprint or signature doesn't match.

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
| AutoSS.ps1 | [tonytheboi](https://github.com/tonytheboi/Tonytheboi-Auto-SS-Tool) | Writes the HTML report; menu tools download to `%ProgramData%\TonytheboiAutoSS` | Checks: none. Menu tools: downloads listed above |
| YarpsModAnalyzer.ps1 | [YarpLetapStan](https://github.com/YarpLetapStan/PowershellScripts) | Nothing (read-only) | Sends mod hashes/names to Modrinth and megabase.vercel.app |
| Services.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | Nothing (read-only) | None |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | [bacanoicua / Red Lotus](https://github.com/bacanoicua/Screenshare) | Nothing (read-only) | None |
| bamparser.ps1 | [spokwn](https://github.com/spokwn/powershells) | Writes `%TEMP%\BAMKeyEntries.html` | Report loads a Google font |
| HabibiModAnalyzer.ps1 | [HadronCollision](https://github.com/HadronCollision/PowershellScripts) | Temp folder in `%TEMP%`, deleted after | Sends mod SHA-1 hashes to Modrinth and megabase.vercel.app |
| Streams.ps1 | [spokwn](https://github.com/spokwn/powershells) | Nothing (read-only) | None |
| SuspiciousScheduler.ps1 | [N0LW](https://github.com/nolww/project-mohr) | Nothing (read-only) | None |

## Source versions

| Script | Upstream commit |
|---|---|
| AutoSS.ps1 | tonytheboi/Tonytheboi-Auto-SS-Tool@cc60349 (modified: repo-hosted scripts, pinned downloads, added [H]) |
| YarpsModAnalyzer.ps1 | YarpLetapStan/PowershellScripts@66fd1d7 (YarpsModAnalyzer6.0.ps1) |
| Services.ps1 | praiselily/lilith-ps@ecf7280 |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | bacanoicua/Screenshare@e212b72 |
| bamparser.ps1, Streams.ps1 | spokwn/powershells@544b5f4 |
| HabibiModAnalyzer.ps1 | HadronCollision/PowershellScripts@e0175a5 |
| SuspiciousScheduler.ps1 | nolww/project-mohr@1a7df47 |

## Notes

- AutoSS treats Secure Boot being off as an automatic ban "under your server rules". That's the original author's rule; ignore it if it isn't yours.
- Results are heuristics. A flag (duplicate prefetch hash, cmd/powershell scheduled task, common cheat word in a mod) is a lead to check, not proof.
- Run the system check **before** re-enabling any services, or you lose evidence that they were off.
