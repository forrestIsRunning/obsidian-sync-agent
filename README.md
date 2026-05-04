# Obsidian Git Sync: One Prompt to Auto-Backup Your Vault to GitHub

[![GitHub release](https://img.shields.io/badge/status-active-brightgreen)]()
[![Platform](https://img.shields.io/badge/platform-macOS-lightgrey)]()
[![Obsidian](https://img.shields.io/badge/tool-Obsidian-purple)](https://obsidian.md)
[![GitHub Repo](https://img.shields.io/badge/GitHub-obsidian--sync--agent-blue)](https://github.com/forrestIsRunning/obsidian-sync-agent)
[![GitHub stars](https://img.shields.io/github/stars/forrestIsRunning/obsidian-sync-agent?style=social)](https://github.com/forrestIsRunning/obsidian-sync-agent)

---

> Give this **one prompt** to Claude Code, and it will automatically:
> - Locate your Obsidian vault
> - `git init` + create a `.gitignore` tuned for Obsidian
> - Create a **private** GitHub repo and push your vault
> - Configure [Obsidian Git](https://github.com/Vinzent03/obsidian-git) plugin (auto-commit, auto-pull)
> - Set up an hourly **cron fallback** sync
> - Print a final summary with everything configured

**No manual terminal commands. No GitHub token setup. No debugging SSH issues. One shot.**

---

## The Problem

You write daily notes in Obsidian. Your vault grows — ideas, projects, journals, your second brain. But:

- iCloud sync is unreliable and has no version history
- Manual git backups are easy to forget
- Every sync guide assumes you know the terminal inside out

You need a **set-it-and-forget-it** backup that costs nothing (GitHub private repos are free) and takes **30 seconds to set up**.

## The Solution

A prompt. Paste it into Claude Code, and the agent does everything — from `git init` to cron job — while explaining each step. Your vault is backed up to GitHub, auto-synced every 15 minutes by the Obsidian Git plugin, with an hourly cron fallback as a safety net.

---

## Quick Start (30 seconds)

```bash
# 1. Clone this repo
git clone https://github.com/forrestIsRunning/obsidian-sync-agent.git
cd obsidian-sync-agent

# 2. Feed the prompt to Claude Code
cat prompt.md | claude

# 3. That's it. Watch the agent do the rest.
```

> Using Cursor, Windsurf, or another AI agent? Open [`prompt.md`](prompt.md), copy the contents, and paste it into your AI chat panel.

---

## The Prompt

Copy the entire block below into your AI agent:

> You are a macOS automation expert. I have an Obsidian Vault on my local Mac. The vault path might be under `~/Documents/ObsidianVault`, `~/Documents/Obsidian\ Vault`, `~/Desktop`, or `~` — find it.
>
> Please complete ALL of the following steps. Before each step, tell me what you are about to do:
>
> **Step 1: Check Environment** — verify git, GitHub CLI (`gh`), user config, and locate the vault.
>
> **Step 2: Initialize Git** — `git init`, create an Obsidian-tuned `.gitignore` (exclude workspace files, `.DS_Store`, `.trash`), `git add` and commit.
>
> **Step 3: Create Private GitHub Repo** — `gh repo create obsidian-vault --private --source=. --remote=origin --push`. Verify the push.
>
> **Step 4: Configure Obsidian Git Plugin** — check `data.json` for current settings, output recommended config (15-min auto backup, auto pull on startup, merge strategy).
>
> **Step 5: Cron Fallback Sync** — create `~/bin/obsidian-sync.sh` with `git add -A`, auto-commit with timestamp, pull-rebase, and push. Add an hourly `crontab` entry.
>
> **Step 6: Verify** — `git log --oneline -5`, `git remote -v`, and a summary table.

Full prompt files:
- [prompt.md](prompt.md) — English (copy-paste ready)
- [prompt-zh.md](prompt-zh.md) — 中文版

---

## What Gets Configured

| Component | Method | Automation Level |
|-----------|--------|-----------------|
| Git repository | `git init` on vault directory | Fully automated |
| Private GitHub repo | `gh repo create --private --push` | Fully automated |
| Obsidian `.gitignore` | Written by agent (excludes workspace/tmp/trash) | Fully automated |
| Obsidian Git plugin | Reads `data.json` + recommends settings | Manual (in Obsidian) |
| Hourly cron sync | `~/bin/obsidian-sync.sh` + `crontab` entry | Fully automated |
| First backup push | Auto-pushed during repo creation | Fully automated |

## What You Need

- **macOS** (Ventura / Sonoma / Sequoia)
- **Obsidian** (used at least once — the vault directory must exist)
- **GitHub account** (free — the repo will be private)
- **Claude Code** (or any AI agent with shell access: Cursor, Windsurf, etc.)

---

## Why This Approach Works

| Approach | Setup Time | Cost | Version History | Auto-Sync |
|----------|-----------|------|----------------|-----------|
| **This prompt** | 30 seconds | Free | Full git history | Plugin + cron |
| Manual git | 15-30 min | Free | Full git history | Manual only |
| Obsidian Sync | 5 min | $5/month | 30-day | Built-in |
| iCloud | 0 (built-in) | Free | None | Unreliable |
| Dropbox | 10 min | $10/month | 30-day | Yes |

---

## About the Author

Built by [**Forrest**](https://github.com/forrestIsRunning) — a macOS automation engineer who got tired of losing Obsidian notes and decided that a well-crafted AI prompt should do the work instead of writing yet another blog tutorial.

- GitHub: [@forrestIsRunning](https://github.com/forrestIsRunning)
- Building: AI agent workflows, developer tooling, macOS automation

---

## Support

If this saved you time or gave you peace of mind:

- **Star** this repo — it helps others find it on GitHub
- **Share** on [Twitter](https://twitter.com/intent/tweet?text=One%20prompt%20to%20auto-backup%20your%20Obsidian%20vault%20to%20GitHub%20using%20Claude%20Code.%20%F0%9F%94%A5%20No%20manual%20terminal%20setup.%20%F0%9F%93%93%20https%3A%2F%2Fgithub.com%2FforrestIsRunning%2Fobsidian-sync-agent) or [LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Fgithub.com%2FforrestIsRunning%2Fobsidian-sync-agent) — tag me so I can connect with you
- **Follow** [@forrestIsRunning](https://github.com/forrestIsRunning) for more automation tools and AI agent workflows

---

## License

MIT — free to use, fork, and share.
