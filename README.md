# winget-packages

My curated WinGet app list for setting up another Windows PC. Use
[`packages.json`](packages.json) for installs and WinGet's upgrade command
for updates.

**The app list is empty.** Populate it from a reviewed export before importing
it on another PC. No package IDs or version pins have been added.

## Requirements

Use Windows with [WinGet (App Installer)](https://learn.microsoft.com/en-us/windows/package-manager/winget/#install-winget).
Check that it is available in PowerShell:

```powershell
winget --version
```

Run the commands below from this repository's folder unless noted otherwise.

## Populate the app list

On the PC with the apps you want to keep, export to a temporary file outside
the public repository:

```powershell
winget export --output "$env:TEMP\winget-export.json"
notepad "$env:TEMP\winget-export.json"
```

Omit `--include-versions` so future installs use the latest available versions.
Review the export before copying it into the repository or sharing it:

- Keep only the apps you want to install on another PC. Use the exported
  `PackageIdentifier` values; do not guess IDs.
- WinGet can recognize apps installed through other methods. Remove those
  entries if they are outside your intended WinGet-managed list.
- Keep each retained source's `SourceDetails` and `Packages` structure intact.
  Remove an entire source entry if you remove all of its packages.
- Remove any `Version` fields to keep the list unpinned. Review optional
  scope or installer settings for assumptions specific to the original PC.
- Check source URLs and installer arguments for private information, internal
  addresses, credentials, or personal paths. Remove unwanted entries or
  sensitive optional fields before publishing. Do not publish a private source.

WinGet warns about apps it cannot match to a configured source and omits them.
Review those warnings; this export is not a full inventory or backup of Windows,
app settings, licenses, or user data.

After reviewing and saving the temporary file, replace the template:

```powershell
Copy-Item "$env:TEMP\winget-export.json" .\packages.json
```

Review the diff before committing it. To get help populating this list, attach
the reviewed JSON export in chat; there is no need to publish the raw export.

## Install on another PC

Clone or download this repository, open PowerShell in its folder, and run:

```powershell
winget import --import-file .\packages.json --ignore-versions
```

`--ignore-versions` selects the latest available versions even if an export
contains version fields. WinGet may also upgrade apps that are already installed.
Review any source or package agreements and administrator prompts.

Read the output for unavailable packages or installation failures and address
them before considering setup complete. Required sources must be available on
the new PC. Importing the list does not remove other installed apps.

## Update installed apps

Check for updates about once a week, or whenever you choose. Save your work
and close apps you intend to update.

Preview available updates:

```powershell
winget upgrade
```

Apply available updates:

```powershell
winget upgrade --all
```

This updates eligible installed apps that WinGet recognizes, **not just the
apps in `packages.json`**. It does not read or change the curated list.
WinGet may skip pinned packages, apps with unknown installed versions, or apps
that require an explicit upgrade. Review the output and any installer,
agreement, administrator, or restart prompts; some updates may need follow-up.

Run these commands yourself. This repository does not schedule updates or run
them at startup. Update `packages.json` only when your desired app selection
changes, using the export and review process above.

## WinGet reference

[Export](https://learn.microsoft.com/en-us/windows/package-manager/winget/export)
| [Import](https://learn.microsoft.com/en-us/windows/package-manager/winget/import)
| [Upgrade](https://learn.microsoft.com/en-us/windows/package-manager/winget/upgrade)
