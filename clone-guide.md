# Clone Guide

Use this guide when spinning up a new project from this template.

## Instructions for Claude

1. Ask the user for a **project name** if they haven't provided one.
2. Ask the user for an **optional remote repo URL** (e.g. a new GitHub repo they've already created). If they don't have one, skip the remote setup commands.
3. Ask the user for an **optional Supabase bucket name**. If not provided, leave `SUPABASE_BUCKET` empty in `.env.local`.
4. Run the following commands, substituting the values provided:

```bash
mkdir <project-name> && cd <project-name>
git clone https://github.com/HunterPfannenstiel/mvp my-app
cd my-app
git remote remove origin
cp ../.env.local .env.local
pnpm install
```

After copying `.env.local`, if the user provided a bucket name, update `SUPABASE_BUCKET=` in `my-app/.env.local` with the value.

If a remote URL was provided:
```bash
git remote add origin <remote-repo-url>
git push -u origin main
```

## Key Rule

`my-app` is the git root and where all dev work happens. The outer `<project-name>` folder is just a namespaced container with no git history of its own.

## Remote Referencing

After setup, wire a read-only `mvp` remote so you can pull updates to Claude/Agent config files without merging unrelated history:

```bash
git remote add mvp https://github.com/HunterPfannenstiel/mvp.git
git remote set-url --push mvp no_push
```

To pull the latest config files:
```bash
git fetch mvp
git ls-tree -r --name-only mvp/main | grep -E '.+/(CLAUDE|AGENTS)\.md$' | xargs git checkout mvp/main --
```

This finds and overwrites every `CLAUDE.md` and `AGENTS.md` in subdirectories with whatever is on `mvp/main` — no merge, no history entanglement. Run this any time you want to sync config updates from the template.
