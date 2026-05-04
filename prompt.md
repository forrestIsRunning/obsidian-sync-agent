# Obsidian Git Sync Prompt

Copy the entire block below into Claude Code or any AI coding agent:

---

You are a macOS automation expert. I have an Obsidian Vault on my local Mac. The vault path might be under `~/Documents/ObsidianVault`, `~/Documents/Obsidian\ Vault`, `~/Desktop`, or `~` — find it.

Please complete ALL of the following steps. Before each step, tell me what you are about to do:

## Step 1: Check Environment
1. Check if git is installed: `git --version`
2. Check if GitHub CLI (gh) is installed: `gh --version`. If not, install via `brew install gh`
3. Verify git global user.name and user.email are configured
4. Find my Obsidian vault — check common locations: `~/Documents`, `~/Desktop`, `~`

## Step 2: Initialize Git Repository
1. cd into the vault directory
2. If not already a git repo, run `git init`
3. Create a `.gitignore` with these entries:
   - `.obsidian/workspace.json`  (changes too often, meaningless commits)
   - `.obsidian/workspace-mobile.json`
   - `.DS_Store`
   - `*.tmp`
   - `.trash/`
   Keep everything else under `.obsidian/` — plugin configs, themes, and snippets need syncing
4. `git add . && git commit -m "Initial vault backup"`

## Step 3: Create Private GitHub Repo and Push
1. Run `gh auth status` to verify GitHub CLI login. If not logged in, ask me to run `gh auth login`
2. Run `gh repo create obsidian-vault --private --source=. --remote=origin --push`
3. Verify the push succeeded — check git log and remote

## Step 4: Configure Obsidian Git Plugin
1. Check if `.obsidian/plugins/obsidian-git/` exists
2. If yes, read `data.json` and show me the current config
3. Output recommended settings (I'll set these manually inside Obsidian):
   - Vault backup interval: 15 minutes
   - Auto pull interval: 15 minutes
   - Pull on startup: enabled
   - Merge strategy: merge (not rebase, for safety)

## Step 5: Set Up System-Level Cron Fallback Sync
Create a shell script at `~/bin/obsidian-sync.sh` with:

```bash
#!/bin/bash
VAULT="<actual vault path>"
cd "$VAULT"
git add -A
git diff-index --quiet HEAD || git commit -m "Auto sync $(date '+%Y-%m-%d %H:%M')"
git pull --rebase origin main 2>/dev/null
git push origin main 2>/dev/null
```

Make it executable with `chmod +x ~/bin/obsidian-sync.sh`.

Then add an hourly cron job (this is a safety net, the Obsidian Git plugin handles the 15-min cadence):
```
0 * * * * ~/bin/obsidian-sync.sh
```

Add it via `crontab -e`.

## Step 6: Verify and Output Summary
1. Run `git log --oneline -5` — show commit history
2. Run `git remote -v` — show remote URL
3. Print a summary table with:
   - Vault path
   - GitHub repo URL
   - Auto-sync methods configured (Obsidian Git plugin + cron)
   - What I need to do manually next (plugin settings in Obsidian UI)
