---
name: taptap-maker-new-project-build
description: Create and bind a new TapTap Maker Lua mini-game project, verify or restore Maker MCP, build a playable MVP, run local checks, and submit a remote Maker build. Use when starting a new Maker game or when the user asks for project initialization, binding, MCP recovery, or the first build.
---

# TapTap Maker 新项目创建与构建

用于从零创建 TapTap Maker 小游戏：准备本地目录、绑定 Maker 项目、确认 MCP、制作可试玩 MVP、完成本地校验，并提交远端构建。与 `taptap-maker-mini-game-dev` skill 一起使用；本 skill 负责新项目和首轮构建流程，后者负责完整的游戏开发、运行可靠性和发布质量。

## 一句话流程

```text
准备目录 -> 初始化并创建 Maker 项目 -> 确认 MCP -> 编写 scripts/main.lua -> 本地校验 -> Maker 状态检查 -> Maker 构建提交 -> 查看日志和预览
```

## 1. 准备项目目录

先确认真实项目根目录，不要复用其他游戏的项目 ID、云配置、广告配置或资源路径：

```powershell
$projectRoot = "D:\WorkSpace\AIProject\Games\NewGameName"
New-Item -ItemType Directory -Force -Path $projectRoot | Out-Null
Set-Location $projectRoot
```

如果目录中已经有 demo，先备份用户代码，再初始化 Maker：

```powershell
$backup = Join-Path $projectRoot ".codex_tmp\pre_maker_init_backup"
New-Item -ItemType Directory -Force -Path $backup | Out-Null
Copy-Item -LiteralPath (Join-Path $projectRoot "scripts") -Destination $backup -Recurse -Force -ErrorAction SilentlyContinue
Copy-Item -LiteralPath (Join-Path $projectRoot "assets") -Destination $backup -Recurse -Force -ErrorAction SilentlyContinue
```

`.codex_tmp/` 只用于本地备份，不应提交到 Maker 构建。

## 2. 创建并绑定 Maker 项目

在 `$projectRoot` 中执行：

```powershell
npx.cmd -y -p @taptap/maker taptap-maker init `
  --target-dir $projectRoot `
  --create `
  --name "新游戏名称" `
  --skip-confirm `
  --json
```

初始化后检查绑定文件和 Git 状态：

```powershell
Get-Content -LiteralPath "$projectRoot\.maker-mcp\config.json" -Raw
git status --short --branch
```

`.maker-mcp/config.json` 应包含 `project_id` 和 `user_id`。通常还会生成 `.git/`、`.maker/`、`.maker-mcp/`、`AGENTS.md`、`CLAUDE.md`、`engine-docs/`、`examples/`、`schemas/`、`templates/`、`tools/` 和 `urhox-libs/`。

## 3. 确认或恢复 Maker MCP

执行 MCP 自检：

```powershell
npx.cmd -y -p @taptap/maker taptap-maker mcp verify --mode self --json
```

正常结果应包含 `"ok": true`，以及 `maker_status_lite`、`maker_build_current_directory` 等工具。

如果自检成功但当前 Codex 会话没有热加载出 `mcp__taptap_maker`，说明配置层通常已经恢复；重启或新开任务后再使用原生工具。必须继续当前任务时，可使用同一个 Maker MCP runtime 直连调用，不要改用普通 Git 提交代替 Maker 构建。

## 4. 先做可试玩 MVP

首版只实现能验证乐趣的最小闭环：

1. 一个清晰、可重复的核心循环。
2. 一个局内目标和即时反馈。
3. 一个结束、结算或失败状态。
4. 一个局外成长点，或明确的重开理由。
5. 一个能展示核心玩法的首屏或结果画面。

优先把玩家在前 10 秒要做的事做清楚，30 秒内让玩家完成一次有意义的动作并得到反馈，1 分钟内给出下一个目标。代码放在 `scripts/main.lua`，资源放在 `assets/`；不要修改 `urhox-libs/`。

## 5. 构建前本地校验

每次提交构建前执行：

```powershell
npx.cmd -y luaparse scripts/main.lua > $null
git diff --check
git status --short --branch
```

如果 Lua LSP 可用，再执行：

```powershell
npx.cmd -y -p @taptap/maker taptap-maker lua-lsp doctor --json
```

也可以直接检查：

```powershell
& "C:\Users\admin\.taptap-maker\lua-lsp-venv\Scripts\maker-lua-lsp.exe" `
  --path "$projectRoot\scripts" `
  --mode check `
  --quiet
```

如果 doctor 显示 ready，但 check 报缺少 `emmylua_check`，记录为本地诊断器依赖缺口，不要把它误判为 Lua 语法错误；以 `luaparse` 和 Maker 远端构建继续验证。

## 6. 检查 Maker 状态

优先调用 `maker_status_lite`，参数：

```json
{
  "target_dir": "D:\\WorkSpace\\AIProject\\Games\\NewGameName"
}
```

重点确认：

```text
project: bound
tap_auth: found
pat: found
git: ready
python: ready
lua_lsp: ready
```

如果提示 `.project/project.json` 缺失，第一次明确的 `maker_build_current_directory` 可以继续执行，Maker 构建会生成必要配置。

## 7. 用 Maker 提交并构建

绑定后的项目不要用普通 `git commit` 或 `git push` 代替构建。调用 `maker_build_current_directory`，最小参数如下：

```json
{
  "target_dir": "D:\\WorkSpace\\AIProject\\Games\\NewGameName",
  "scriptsPath": "scripts",
  "entry": "main.lua",
  "files": ["scripts/main.lua"],
  "message": "Add playable MVP demo",
  "timeout_ms": 600000
}
```

首次构建需要提交的资源和 meta 文件也要明确加入 `files`，例如 `.gitignore`、`scripts/main.lua.meta`、字体、图片、音频及对应 meta 文件。

成功结果应包含：

```text
Maker project submitted, then remote Maker build finished
preview_refresh: ok
committed: yes
commit_hash: <hash>
项目构建成功
```

记录 commit hash、Maker 项目地址、本地校验结果、未提交文件和 runtime log 状态。

## 8. 处理 `needs_pull`

如果 Maker 提示远端有新提交而本地也有未提交改动，先只 stash 本次改过的文件：

```powershell
git status --short --branch
git stash push -m "codex-maker-work" -- scripts/main.lua
git pull --rebase origin main
git stash pop
```

如果改了资源文件，把资源路径一并加入 stash。恢复后重新执行 `luaparse`、`git diff --check` 和 `git status`，再重新调用 Maker 构建。不要直接 pull 覆盖本地改动。

## 9. 构建后检查日志

常见运行文件：

```text
.maker/logs/runtime/runtime.log
.maker/logs/runtime/state.json
.maker/logs/runtime/watcher.out.log
.maker/logs/runtime/watcher.err.log
```

```powershell
Get-Content -LiteralPath "$projectRoot\.maker\logs\runtime\state.json" -Raw
Get-Content -LiteralPath "$projectRoot\.maker\logs\runtime\runtime.log" -Tail 80
```

如果 watcher 正常、没有新的 Lua error，说明首轮运行至少没有明显脚本崩溃；这仍不等于手机验收，触控、布局、广告、性能和视觉效果需要单独验收。

## 10. 交付报告

构建完成后简短报告：

```text
已提交并构建成功。

- 项目：<游戏名>
- 路径：<projectRoot>
- Commit：<hash>
- Maker 地址：<url>
- 本地校验：luaparse 通过，git diff --check 通过
- 运行日志：watcher 正常，未见 Lua error
- 遗留：<例如 .codex_tmp/ 为本地备份目录，未提交>
```

## 11. 常见边界

1. 当前会话看不到 Maker MCP，不等于 MCP 配置没修好；先运行 `taptap-maker mcp verify --mode self --json`。
2. Maker 构建必须走 `maker_build_current_directory`。
3. `.project/` 可能在第一次远端构建后才生成。
4. `.codex_tmp/`、策划文档和提示词素材默认不要提交。
5. 接入广告前先查询 `get_ad_config`，不要只凭本地 SDK 文件猜测。
6. 第一版优先验证核心循环的趣味和反馈，不要用大系统掩盖玩法问题。

## 12. 下一款游戏开场模板

```text
请在 D:\WorkSpace\AIProject\Games\<NewGameName> 新建并绑定 TapTap Maker 项目，
游戏名《<中文名>》。
先做一个可试玩 MVP demo，突出核心玩法，不要过早堆大系统。
每次改动先本地校验，需要提交或构建时走 Maker MCP。
请按 D:\WorkSpace\AIProject\TapTap\docs\08_taptap_maker_new_project_build_workflow.md 执行。
```
