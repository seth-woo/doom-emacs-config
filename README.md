# Doom Emacs Config

Personal Doom Emacs setup with two parts in one repo:

- `.doom.d/`: your user configuration (modules, package declarations, personal settings)
- `.emacs.d/`: a vendored Doom Emacs framework tree (including modules, CLI, and docs)

This layout gives you a self-contained setup where both framework and private config are versioned together.
[^calendar-note]

## Repository Layout

- `.doom.d/init.el`: enabled Doom modules and flags
- `.doom.d/config.el`: user-level settings (theme, line numbers, org directory, custom Lisp)
- `.doom.d/packages.el`: extra package declarations (`org-gcal`)
- `.doom.d/custom.el`: Emacs Custom variables
- `.doom.d/private/`: ignored private files (credentials/secrets)
- `.emacs.d/`: Doom framework source, CLI scripts, and official module docs
- `.gitignore`: excludes private credentials and common Emacs artifacts

## Current Configuration (from `.doom.d`)

### Enabled modules

- `:completion`
  - `(corfu +orderless)`
  - `vertico`
- `:ui`
  - `doom`, `doom-dashboard`, `hl-todo`, `modeline`, `ophints`
  - `(popup +defaults)`
  - `(vc-gutter +pretty)`
  - `vi-tilde-fringe`, `workspaces`
- `:editor`
  - `(evil +everywhere)`
  - `file-templates`, `fold`, `snippets`
  - `(whitespace +guess +trim)`
- `:emacs`
  - `dired`, `electric`, `tramp`, `undo`, `vc`
- `:checkers`
  - `syntax`
- `:tools`
  - `(eval +overlay)`, `lookup`, `magit`
- `:os`
  - `macos` (conditionally, only when running on macOS)
- `:lang`
  - `emacs-lisp`, `markdown`, `org`, `sh`
- `:config`
  - `(default +bindings +smartparens)`

### User settings

Defined in `.doom.d/config.el`:

- Theme: `doom-acario-dark`
- Line numbers: enabled (`display-line-numbers-type` set to `t`)
- Org directory: `~/org/`

### Extra packages

Defined in `.doom.d/packages.el`:

- `org-gcal`

`custom.el` also records selected packages including:

- `all-the-icons`
- `all-the-icons-nerd-fonts`
- `org-gcal`

## Private Credentials

The repo ignores `.doom.d/private/`.
Use this directory for sensitive files, e.g. OAuth/client secrets for calendar integrations.

Current ignored pattern:

- `.doom.d/private/`

Recommended approach:

1. Keep secrets in files under `.doom.d/private/`.
2. Load them from `config.el` (or a separate private file) with guarded logic.
3. Never commit token/client-secret files.

## Setup

### 1. Prerequisites

Install at minimum:

- Emacs (Doom README in this repo lists supported versions)
- `git`
- `ripgrep` (`rg`)

Optional but recommended:

- `fd`
- Nerd Fonts / icon fonts (for modeline/icons)

### 2. Clone

```powershell
git clone https://github.com/seth-woo/doom-emacs-config.git
cd doom-emacs-config
```

### 3. Point Emacs to this config

Choose one model:

- Use this repo in-place and link/sync the directories to your expected Doom paths.
- Or copy `.doom.d/` and `.emacs.d/` to your preferred locations.

Typical expected locations are user-level dotdirs (`~/.doom.d` and `~/.emacs.d`) unless you use custom env vars/launch scripts.

Windows symlink example (PowerShell, from your home directory):

```powershell
New-Item -ItemType SymbolicLink -Path $HOME\.doom.d -Target "C:\Users\Seth Woo\doom-emacs-config\.doom.d"
New-Item -ItemType SymbolicLink -Path $HOME\.emacs.d -Target "C:\Users\Seth Woo\doom-emacs-config\.emacs.d"
```

### 4. Sync packages

From repo root:

```powershell
.emacs.d\bin\doom.ps1 sync
```

On Unix-like shells:

```bash
./.emacs.d/bin/doom sync
```

Then restart Emacs.

## Daily Maintenance

- After changing `.doom.d/init.el` or `.doom.d/packages.el`:
  - run `doom sync`
- After changing only `.doom.d/config.el`:
  - usually just reload/restart Emacs
- Update Doom/framework and package pins periodically:

```powershell
.emacs.d\bin\doom.ps1 upgrade
```

Useful diagnostics:

```powershell
.emacs.d\bin\doom.ps1 doctor
```

## Notes About This Repo

- This repository tracks the full `.emacs.d` Doom framework tree instead of using it only as an external dependency.
- Because framework code is vendored, normal updates may involve larger diffs than a config-only repo.
- Keep personal credentials and machine-local secrets in `.doom.d/private/`.

## Customization Tips

- Add/disable modules in `.doom.d/init.el`, then run `doom sync`.
- Add packages in `.doom.d/packages.el`, then run `doom sync`.
- Put personal Lisp in `.doom.d/config.el` using `after!`, `use-package!`, and `map!`.

## Related Upstream Docs

- Local upstream README: `.emacs.d/README.md`
- Doom docs in repo: `.emacs.d/docs/`
- Official site: [https://doomemacs.org](https://doomemacs.org)

[^calendar-note]: I am exploring Doom Emacs primarily for Org mode, especially the Google Calendar integration (`org-gcal`). I am still undecided whether this workflow is efficient for me, or if it would be better to use the official Google Calendar web UI directly. Not everything has to be done through a CLI.
