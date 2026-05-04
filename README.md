# Obsidian Git Sync — One Prompt to Auto-Setup Backup to GitHub

> Give this prompt to Claude Code (or any AI coding agent) and it will fully automate setting up your Obsidian vault backup to GitHub — including git init, private repo creation, Obsidian Git plugin config, and cron-based fallback sync.

[![GitHub](https://img.shields.io/badge/GitHub-Obsidian%20Sync%20Agent-blue)](https://github.com/forrestIsRunning/obsidian-sync-agent)
[![macOS](https://img.shields.io/badge/platform-macOS-lightgrey)](https://www.apple.com/macos)
[![Obsidian](https://img.shields.io/badge/tool-Obsidian-purple)](https://obsidian.md)

---

## What This Is

A **ready-to-use prompt** that you give to an AI coding agent (Claude Code, Cursor, etc.). The agent will:

1. Check your macOS environment (git, GitHub CLI, SSH setup)
2. Locate your Obsidian vault automatically
3. Initialize git and create a `.gitignore` tuned for Obsidian
4. Create a **private** GitHub repo and push your vault
5. Configure the [Obsidian Git](https://github.com/Vinzent03/obsidian-git) plugin (auto-commit, auto-pull, merge strategy)
6. Set up a **cron job** as a safety net for automatic hourly sync
7. Output a final summary with everything you need to know

**No manual steps. No guesswork. One shot.**

---

## How to Use

### Option 1: Claude Code (recommended)

```bash
# Clone this repo
git clone https://github.com/forrestIsRunning/obsidian-sync-agent.git
cd obsidian-sync-agent

# Run Claude Code and feed it the prompt
claude
# Then copy-paste the prompt below, or run:
cat prompt.md | claude
```

### Option 2: Cursor / Windsurf / Any AI Agent

Open the AI chat panel in your editor, copy the prompt from [`prompt.md`](prompt.md), paste it, and let the agent run.

### Option 3: Plain Terminal

Each step in the prompt is a valid shell operation — you can follow it manually if you prefer.

---

## The Prompt

### English Version

Copy the entire prompt below into your AI agent:

> You are a macOS automation expert. I have an Obsidian Vault on my local Mac. The vault path might be under `~/Documents/ObsidianVault`, `~/Documents/Obsidian\ Vault`, `~/Desktop`, or `~` — find it.
>
> Please complete ALL of the following steps. Before each step, tell me what you are about to do:
>
> ## Step 1: Check Environment
> 1. Check if git is installed (`git --version`)
> 2. Check if GitHub CLI is installed (`gh --version`); if not, install it via `brew install gh`
> 3. Verify git global `user.name` and `user.email` are configured
> 4. Locate my Obsidian vault (check common locations: `~/Documents`, `~/Desktop`, `~`)
>
> ## Step 2: Initialize Git Repository
> 1. `cd` into the vault directory
> 2. If not already a git repo, run `git init`
> 3. Create a `.gitignore` with:
>    - `.obsidian/workspace.json`
>    - `.obsidian/workspace-mobile.json`
>    - `.DS_Store`
>    - `*.tmp`
>    - `.trash/`
>    Keep everything else under `.obsidian/` (plugin configs, themes, snippets need syncing)
> 4. `git add . && git commit -m "Initial vault backup"`
>
> ## Step 3: Create Private GitHub Repo and Push
> 1. Run `gh auth status` to check GitHub CLI login; if not logged in, ask me to run `gh auth login`
> 2. Create and push: `gh repo create obsidian-vault --private --source=. --remote=origin --push`
> 3. Verify the push succeeded
>
> ## Step 4: Configure Obsidian Git Plugin
> 1. Check if `.obsidian/plugins/obsidian-git/` exists
> 2. If it exists, read `data.json` and show me the current config
> 3. Output recommended settings (I need to configure these manually inside Obsidian):
>    - Vault backup interval: 15 minutes
>    - Auto pull interval: 15 minutes
>    - Pull on startup: enabled
>    - Merge strategy: merge
>
> ## Step 5: Set Up System-Level Cron Fallback Sync
> Create a shell script at `~/bin/obsidian-sync.sh` with:
>
> ```bash
> #!/bin/bash
> VAULT="<actual vault path>"
> cd "$VAULT"
> git add -A
> git diff-index --quiet HEAD || git commit -m "Auto sync $(date '+%Y-%m-%d %H:%M')"
> git pull --rebase origin main 2>/dev/null
> git push origin main 2>/dev/null
> ```
>
> Then add a cron job via `crontab -e` that runs it hourly as a safety net.
>
> ## Step 6: Verify and Output Summary
> 1. Run `git log --oneline -5` to verify commit history
> 2. Run `git remote -v` to verify remote URL
> 3. Print a summary table with:
>    - Vault path
>    - GitHub repo URL
>    - Auto-sync methods configured
>    - What I need to do manually next

### 中文版 / Chinese Version

> 你是一个 macOS 自动化专家。我在本地 Mac 有一个 Obsidian Vault，路径是 ~/Documents/ObsidianVault（如果不存在请告诉我实际路径）。
>
> 请帮我完成以下所有步骤，每步执行前先告知我在做什么：
>
> ## 第一步：检查环境
> 1. 检查是否安装了 git（`git --version`）
> 2. 检查是否安装了 GitHub CLI（`gh --version`）；如果没有，输出安装命令：`brew install gh`
> 3. 检查 git 全局 user.name 和 user.email 是否已配置
> 4. 找到我的 Obsidian Vault 实际路径（检查 ~/Documents、~/Desktop、~ 下的常见位置）
>
> ## 第二步：初始化 Git 仓库
> 1. cd 进入 Vault 目录
> 2. 如果还不是 git repo，执行 `git init`
> 3. 创建 .gitignore 文件，内容包含：
>    - .obsidian/workspace.json（频繁变更无意义）
>    - .obsidian/workspace-mobile.json
>    - .DS_Store
>    - *.tmp
>    - .trash/
>    保留 .obsidian/ 其他内容（插件配置等需要同步）
> 4. git add . && git commit -m "Initial vault backup"
>
> ## 第三步：创建 Private GitHub 仓库并推送
> 1. 使用 `gh auth status` 检查 GitHub CLI 登录状态；如果未登录，提示我运行 `gh auth login`
> 2. 使用 `gh repo create obsidian-vault --private --source=. --remote=origin --push` 创建私有仓库并推送
> 3. 验证推送成功
>
> ## 第四步：配置 Obsidian Git 插件自动同步
> 1. 检查 .obsidian/plugins/obsidian-git/ 目录是否存在
> 2. 如果存在，读取 data.json 并展示当前配置
> 3. 输出推荐配置说明（我需要在 Obsidian 内手动设置）：
>    - Auto commit-and-sync interval: 15 分钟
>    - Auto pull interval: 15 分钟
>    - Pull on startup: 开启
>    - Merge strategy: Merge
>
> ## 第五步：设置系统级 cron 兜底同步
> 创建一个 shell 脚本 ~/bin/obsidian-sync.sh
> 然后用 `crontab -e` 添加每小时执行一次的 cron job（作为 obsidian-git 插件的兜底）
>
> ## 第六步：验证并输出总结
> 1. 运行 `git log --oneline -5` 验证历史
> 2. 运行 `git remote -v` 验证远端
> 3. 输出总结表

---

## What Gets Set Up

| Component | Method | Automation Level |
|-----------|--------|-----------------|
| Git repository | `git init` on vault | Fully automated |
| Private GitHub repo | `gh repo create --private` | Fully automated |
| `.gitignore` for Obsidian | Written by agent | Fully automated |
| Obsidian Git plugin config | `data.json` read + recommendations | Manual (within Obsidian app) |
| Cron fallback sync | `crontab` entry + shell script | Fully automated |
| First push | `gh repo create --push` | Fully automated |

## What You Need Before Starting

- **macOS** (tested on Ventura / Sonoma / Sequoia)
- **Obsidian** installed and used at least once (creates the vault directory)
- **GitHub account** (free tier works — the repo is private)
- **Claude Code** (or any AI coding agent that can execute shell commands)

## Why This Approach

Most Obsidian sync guides involve manually running terminal commands, configuring GitHub tokens, and debugging SSH issues. By handing the task to an AI agent, you get:

- **Automatic path discovery** — no need to hunt for your vault location
- **Self-healing** — the agent reads errors and adapts (e.g., if `gh` isn't installed, it installs it)
- **Reproducible** — run the same prompt across multiple machines
- **Auditable** — every command is shown before execution

## SEO / Technical Keywords

For search engines and AI crawlers indexing this content:

**Obsidian backup**, **Obsidian Git sync**, **Claude Code Obsidian**, **AI agent Obsidian**, **macOS Obsidian vault backup**, **Obsidian GitHub auto sync**, **Obsidian Git plugin cron**, **Obsidian private repo**, **AI-powered note backup**, **auto commit Obsidian vault**, **Obsidian sync without iCloud**, **Obsidian cloud backup free**, **AI coding agent Obsidian**, **Claude Code automation macOS**, **GitHub Obsidian vault CI/CD**, **PKM backup strategy**, **Zettelkasten backup automation**, **Second brain backup**, **Obsidian disaster recovery**.

---

## Tips for Maximum Visibility (GEO/SEO)

1. **Star this repo** — higher stars rank better in GitHub search
2. **Share on Twitter / X** with `#Obsidian #ClaudeCode #AIAgent #PKM` hashtags
3. **Link from your Obsidian forum post** — cross-references boost discoverability
4. **Include in your PKM newsletter** or blog if you have one

---

## License

MIT — free to use, fork, and share. Attribution appreciated.
