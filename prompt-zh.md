# Obsidian Git 同步 Prompt

将以下内容完整复制到 Claude Code 或任意 AI 编码助手中：

---

你是一个 macOS 自动化专家。我在本地 Mac 有一个 Obsidian Vault，路径是 ~/Documents/ObsidianVault（如果不存在请告诉我实际路径）。

请帮我完成以下所有步骤，每步执行前先告知我在做什么：

## 第一步：检查环境
1. 检查是否安装了 git（`git --version`）
2. 检查是否安装了 GitHub CLI（`gh --version`）；如果没有，输出安装命令：`brew install gh`
3. 检查 git 全局 user.name 和 user.email 是否已配置
4. 找到我的 Obsidian Vault 实际路径（检查 ~/Documents、~/Desktop、~ 下的常见位置）

## 第二步：初始化 Git 仓库
1. cd 进入 Vault 目录
2. 如果还不是 git repo，执行 `git init`
3. 创建 .gitignore 文件，内容包含：
   - .obsidian/workspace.json（频繁变更无意义）
   - .obsidian/workspace-mobile.json
   - .DS_Store
   - *.tmp
   - .trash/
   保留 .obsidian/ 其他内容（插件配置等需要同步）
4. git add . && git commit -m "Initial vault backup"

## 第三步：创建 Private GitHub 仓库并推送
1. 使用 `gh auth status` 检查 GitHub CLI 登录状态；如果未登录，提示我运行 `gh auth login`
2. 使用 `gh repo create obsidian-vault --private --source=. --remote=origin --push` 创建私有仓库并推送
3. 验证推送成功

## 第四步：配置 Obsidian Git 插件自动同步
1. 检查 .obsidian/plugins/obsidian-git/ 目录是否存在
2. 如果存在，读取 data.json 并展示当前配置
3. 输出推荐配置说明（我需要在 Obsidian 内手动设置）：
   - Auto commit-and-sync interval: 15 分钟
   - Auto pull interval: 15 分钟
   - Pull on startup: 开启
   - Merge strategy: Merge

## 第五步：设置系统级 cron 兜底同步
创建一个 shell 脚本 ~/bin/obsidian-sync.sh，内容为：

```bash
#!/bin/bash
VAULT="<实际vault路径>"
cd "$VAULT"
git add -A
git diff-index --quiet HEAD || git commit -m "Auto sync $(date '+%Y-%m-%d %H:%M')"
git pull --rebase origin main 2>/dev/null
git push origin main 2>/dev/null
```

然后用 `crontab -e` 添加每小时执行一次的 cron job（作为 obsidian-git 插件的兜底）。

## 第六步：验证并输出总结
1. 运行 `git log --oneline -5` 验证历史
2. 运行 `git remote -v` 验证远端
3. 输出一张总结表：
   - Vault 路径
   - GitHub Repo URL
   - 自动同步方式
   - 下一步你需要手动做什么
