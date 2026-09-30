# 电梯调度项目证据
核对日期：2026-09-27；本地 HEAD：9cfd362；目录 D:/WorkSpace/AIProject/Games/elevator-dispatch。

- main.lua：ProfileResetReady 检查 campaign、待合并数据、CanDelete、IsBusy；不依赖云档必须完整。
- OpenProfileResetConfirm/ConfirmProfileReset：局外设置入口，输入“确认重置”，处理中禁用重复提交。
- BuildProfileResetSettings 保留音效/音量/部分视觉偏好，回到默认皮肤音乐并关闭测试功能。
- cloud_save_system.lua Delete(callback, appendBatch)：删除 LEGACY_KEY、主/备份meta、SaveSchema.PARTITIONS 的两个槽；批次可附加榜单清理。成功清理待上传与提交指纹。
- leaderboard_system.lua AppendProfileReset：free/endless分数设0、active设0、repair设1，删除成绩上下文、成就数量、楼层勋章。读取侧与恢复侧须一起检查。
- ApplySuccessfulProfileReset：成功后清本地、旧恢复候选，重新创建并保存新档；leaderboardRepairApprovedScores归0。
- 来源错误提示将timeout描述为“旧存档未被清除”，不能作为云端结果确定性的证据；通用实现应区分未知结果。
- 本次阅读未建立“跨设备重置世代协议已实现”的证据，不要宣称已防止另一台设备上传旧档。
- 参考 scripts/save_schema.lua、cloud_save_system.lua、save_recovery_system.lua 盘点数据范围。测试线索 .codex_tmp/run_profile_reset.cjs、verify_cloud_live_safety.lua，需核对版本后使用。

