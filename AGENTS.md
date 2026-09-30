# TapTap 云端开发项目

本仓库使用项目级 TapTap Maker 技能，安装目录为 `.agents/skills/`。

## 技能选择

开始 TapTap 游戏任务时，读取 `.agents/skills/taptap-maker-mini-game-dev/SKILL.md`，再按任务读取相关技能：

| 任务 | 技能文件 |
| --- | --- |
| 新建游戏、项目绑定、首轮构建 | `.agents/skills/taptap-maker-new-project-build/SKILL.md` |
| 埋点、关卡漏斗、广告观看统计 | `.agents/skills/taptap-maker-game-analytics/SKILL.md` |
| 激励广告接入与故障排查 | `.agents/skills/taptap-maker-rewarded-ads/SKILL.md` |
| 存档重置、排行榜清理 | `.agents/skills/taptap-maker-save-reset/SKILL.md` |
| elevator-dispatch 专用 KTG | `.agents/skills/taptap-ktg-build/SKILL.md` |

用户指定 `$技能名` 时读取相应的 `SKILL.md`。只读取任务需要的引用文档。

## 云端执行

- 在云端工作前读取 `docs/taptap-cloud.md`，使用实际 Linux 项目路径和 Bash 命令。
- `taptap-skills-2026-09-30/` 是上传的原始备份；执行时使用 `.agents/skills/` 中的 UTF-8 安装副本。
- 来源技能中的 Windows 路径、elevator-dispatch 案例和测试脚本是历史参考；先确认当前游戏的源码、项目绑定和工具能力。
- Maker CLI/MCP、账号授权和项目绑定分别核验。技能文件存在不能作为远端构建或手机验证已完成的证据。
