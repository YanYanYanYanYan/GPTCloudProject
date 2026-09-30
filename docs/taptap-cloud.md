# TapTap 技能的云端使用

## 安装结果

2026-09-30 从 GitHub 提交 `cc8f7a4` 的 `taptap-skills-2026-09-30/` 安装了 6 个技能。
安装副本位于 `.agents/skills/`，包含原有的 `SKILL.md`、`agents/` 和 `references/`。
所有安装文件统一为 UTF-8；`taptap-ktg-build` 的原始文件使用 GB18030 兼容编码。
原始备份保留，安装副本补充了本说明的入口，KTG 的触发说明改为跨平台项目名称。

| 技能 | 使用范围 |
| --- | --- |
| `taptap-maker-mini-game-dev` | 通用游戏开发、调试、质量检查 |
| `taptap-maker-new-project-build` | 新项目、绑定、MVP 和首轮构建 |
| `taptap-maker-game-analytics` | 埋点、关卡漏斗、广告统计 |
| `taptap-maker-rewarded-ads` | 激励广告接入与故障排查 |
| `taptap-maker-save-reset` | 存档重置和排行榜清理 |
| `taptap-ktg-build` | 仅用于已确认的 elevator-dispatch 项目 |

通过根目录 `AGENTS.md` 按任务读取技能。可直接发送：

```text
使用 $taptap-maker-mini-game-dev 开发当前项目。
使用 $taptap-maker-new-project-build 创建一个新的 TapTap Maker 游戏。
使用 $taptap-maker-rewarded-ads 排查当前游戏的激励广告。
```

当前聊天可以直接读取安装文件。技能选择器是否即时刷新取决于客户端；文件路径和 `AGENTS.md` 提供了明确的读取入口。

## 云端路径与命令

技能正文保留了原 Windows 工作流。云端使用下面的 Linux 命令对应它们，
不要执行 `npx.cmd`、PowerShell 命令或 `C:\` / `D:\` 路径。
先确认真实游戏根目录；仓库内只有技能时，不将技能仓库误认为已经绑定的游戏。

核验和本地校验可以执行：

```bash
pwd
git status --short --branch
npx -y -p @taptap/maker taptap-maker --help
npx -y -p @taptap/maker taptap-maker doctor --target-dir "$PWD" --json
```

有游戏源码后，在其根目录执行：

```bash
npx -y luaparse scripts/main.lua > /dev/null
git diff --check
```

用户要求创建游戏时，使用已确认的游戏名称和绝对目录：

```bash
maker_game_root='/workspace/GPTCloudProject/games/NewGameName'
mkdir -p "$maker_game_root"
npx -y -p @taptap/maker taptap-maker init \
  --target-dir "$maker_game_root" \
  --create --name '用户指定的游戏名称' --skip-confirm --json
```

已有游戏使用其实际根目录和项目绑定。`target_dir` 应为云端绝对路径。
现有 demo 需要备份时，使用当前项目的 `.codex_tmp/` 和 `cp -a`。
读取 JSON 使用 `cat`，查看日志使用 `tail -n 80`。

MCP 自检命令：

```bash
npx -y -p @taptap/maker taptap-maker mcp verify --mode self --json
```

首轮构建以当前 Maker 工具声明为准；`maker_build_current_directory` 的
`target_dir` 使用真实目录，`scriptsPath` 与 `entry` 使用实际项目配置。
技能仓库的 Git 提交只保存开发文件，不能报告为 Maker 游戏构建。

## 当前依赖核验

安装时已核验 Node.js 24.19.0、npm 11.9.0、Python 3.12.14 和 Git 可用。
从 npm 查询到 `@taptap/maker` 0.0.36，CLI 可以启动并输出帮助，支持 Linux。
`npx` 缓存属于执行环境；环境重建后可以按上面的命令重新下载。

Maker MCP 的 `--mode self` 自检已通过：`ok: true`，阶段为 `tools_list`，
返回了 `maker_status_lite`、`maker_build_current_directory` 和 `get_ad_config` 等工具。
这是 MCP runtime 启动与工具声明验证；当前聊天的原生工具选择器是否热加载需另行确认。

Maker `doctor` 的核验结果：

- Git 与 Python 就绪。
- TapTap 登录和 PAT 尚未配置。
- 当前仓库尚未绑定 Maker 项目，也没有游戏源码或开发套件。
- `maker-lua-lsp` 未安装，不能声称 LSP 检查已通过。
- 开发套件及版本策略的在线查询失败；开始实际 Maker 项目任务时检查网络错误和当前允许的域名。

因此技能可以用于策划、实现和本地检查；远端项目创建、构建、预览及广告配置
需要完成 Maker 的登录、项目绑定和接口连通性检查。
登录与凭据通过 Maker 支持的授权流程或云环境凭据配置提供，不将令牌写入仓库或聊天。

## 来源案例

`references/elevator-evidence.md` 中的 Windows 路径、数据键、函数和临时测试脚本
用于说明原项目实现。本次上传未包含 elevator-dispatch 游戏源码，
不能把这些脚本当作已安装的依赖，也不能将该游戏的配置用于新项目。
`maker-lua-lsp` 的本地插件配置未包含 `SKILL.md`，不在这 6 个技能的安装范围内。
