# Aaron's AGENTS.md

This repository tracks a global Codex `AGENTS.md` file. Codex reads this file from your Codex home directory, which defaults to `~/.codex`.

The goal is simple: keep one personal instruction file under version control, then pull updates onto each device.

## Install

If you do not already have a Codex home directory, create it:

```powershell
mkdir $HOME\.codex
```

Initialize Git in that directory:

```powershell
cd $HOME\.codex
git init
git remote add origin https://github.com/SCHW-AI/aarons-agents-md.git
git fetch origin main
git switch -c main --track origin/main
```

This keeps the existing `.codex` directory in place and checks out only the tracked files from this repo. If you already have a local `AGENTS.md`, move or commit it before switching to the synced branch.

## Update

Pull the latest instructions:

```powershell
cd $HOME\.codex
git pull
```

After editing `AGENTS.md`, commit and push:

```powershell
cd $HOME\.codex
git add AGENTS.md README.md .gitignore
git commit -m "Update Codex instructions"
git push
```

## What This Repo Tracks

This repo intentionally tracks only:

- `AGENTS.md`
- `README.md`
- `.gitignore`

Everything else in `~/.codex` is ignored because it may contain local configuration, sessions, caches, credentials, plugins, logs, and other machine-specific Codex state.

## Notes

If `~/.codex/AGENTS.override.md` exists, Codex uses that file instead of `AGENTS.md` at the global level. Use `AGENTS.override.md` only for temporary local overrides that should not be synced.
