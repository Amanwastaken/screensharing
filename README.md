# Screensharing

Copies of Minecraft screenshare PowerShell scripts. Each one was reviewed before being added here, so players run a fixed, known version rather than whatever is live upstream.

All scripts are by their original authors (credited below). The only change is that they are hosted here.

## Full system (recommended)

Open **PowerShell as Administrator** (right-click Start → Terminal (Admin)) and run:

```powershell
irm https://raw.githubusercontent.com/Amanwastaken/screensharing/main/AutoSS.ps1 | iex
```

Choose **[1]** to run all checks. It saves an HTML report with a suspicion score and reasons. The menu also launches extra tools:

| Key | Tool | Hosted | How it's trusted |
|---|---|---|---|
| S | Services check (lilith) | This repo | Reviewed copy |
| M | Yarp mod analyzer | This repo | Reviewed copy |
| H | Habibi mod analyzer | This repo | Reviewed copy |
| D | Doomsday client finder (lilith) | This repo | Reviewed copy |
| E | ShellSearch, suspicious PowerShell in event logs (lilith) | This repo | Reviewed copy |
| T | Scheduled tasks, unsigned + suspicious (lilith) | This repo | Reviewed copy |
| F | Non-Microsoft files → `C:\Screenshare\paths.txt` (lilith) | This repo | Reviewed copy |
| N | Service Enabler (lilith). **Changes services**, asks for YES | This repo | Reviewed copy |
| K | System Informer 4.0.26241.138 (portable, MIT) | This repo (`tools/`) | Pinned SHA-256 + must be signed by Winsider |
| P | Spokwn Prefetch Parser v1.5.5 | Spokwn's GitHub | Pinned SHA-256 (signed by espouken) |
| A | Spokwn BAM Parser v1.2.9 | Spokwn's GitHub | Pinned SHA-256 (signed by espouken) |
| O | Spokwn Paths Parser v1.2 (use after F) | Spokwn's GitHub | Pinned SHA-256 (signed by espouken) |
| V | Spokwn PcaSvc Executed v0.8.7 | Spokwn's GitHub | Pinned SHA-256 (signed by espouken) |
| Y | Spokwn ActivitiesCache Parser v0.6.5 | Spokwn's GitHub | Pinned SHA-256 (signed by espouken) |
| J | P1AE javaw memory scanner v1.12 | p1aegg's GitHub | Pinned SHA-256 (open source, unsigned) |
| W / L | NirSoft WinPrefetchView / LastActivityView | nirsoft.net | Must be validly signed by Nir Sofer |
| C | Cleanup | | Removes downloaded tools from `%ProgramData%\TonytheboiAutoSS` |

Downloaded programs are refused and deleted if the fingerprint or signature doesn't match, so a file swapped upstream can't run.
Spokwn's and P1AE's programs have no licence that allows re-hosting, so they are fetched from the author at a locked version instead of being copied here.

Not included on purpose: Lily's Collector (adds a Defender exclusion), IHateScreensharing (rewrites system settings), Spokwn-Collect (unpinned bulk downloads), and Spokwn's unsigned tools (JournalTrace, BamDeletedKeys, KernelLiveDumpTool, espouken.exe).

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
| DoomsdayFinder.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) (credits Zedoon) | Nothing (read-only) | None |
| ShellSearch.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | Writes a .txt to `%TEMP%`, opens Notepad | None |
| SchedulesV2.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) (based on N0LW) | Nothing (read-only) | None |
| CommonDirectories.ps1 | [praiselily](https://github.com/praiselily/lilith-ps) | Writes `C:\Screenshare\paths.txt` | None |
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
| AutoSS.ps1 | tonytheboi/Tonytheboi-Auto-SS-Tool@cc60349 (modified: repo-hosted scripts, pinned downloads, added [H]) |
| YarpsModAnalyzer.ps1 | YarpLetapStan/PowershellScripts@66fd1d7 (YarpsModAnalyzer6.0.ps1) |
| Services.ps1, DoomsdayFinder.ps1, ShellSearch.ps1, SchedulesV2.ps1, CommonDirectories.ps1, Service-Enabler.ps1 | praiselily/lilith-ps@ecf7280 |
| tools/systeminformer-4.0.26241.138-bin.zip | winsiderss/systeminformer release v4.0.26241.138 (MIT) |
| RedLotusPrefetchIntegrityAnalyzer.ps1 | bacanoicua/Screenshare@e212b72 |
| bamparser.ps1, Streams.ps1 | spokwn/powershells@544b5f4 |
| HabibiModAnalyzer.ps1 | HadronCollision/PowershellScripts@e0175a5 |
| SuspiciousScheduler.ps1 | nolww/project-mohr@1a7df47 |

## Notes

- AutoSS treats Secure Boot being off as an automatic ban "under your server rules". That's the original author's rule; ignore it if it isn't yours.
- Results are heuristics. A flag (duplicate prefetch hash, cmd/powershell scheduled task, common cheat word in a mod) is a lead to check, not proof.
- Run the system check **before** re-enabling any services, or you lose evidence that they were off.
