# 电梯调度项目证据
核对日期：2026-09-27；本地 HEAD：9cfd362；目录 D:/WorkSpace/AIProject/Games/elevator-dispatch。

scripts/release_analytics.lua：
- first_enter；tutorial_step_1/2/3（装人/选层/发车）；trial_1_complete 至 trial_11_complete。
- formal_operation_started；floor_6_reached 至 floor_10_reached。
- floor_10_day_20/40/60/80；test_user。
- 模式 trial/free/endless；run_start、run_end、complete/quit/fail，进度总和、投诉总和/分桶、退出进度分桶；endless 波次总和。
- 里程碑 BatchSet:SetInt(1)，失败5秒起指数退避至60秒；计数 AddMetrics 使用 Add，是 best-effort，不是可靠事件日志。
- main.lua 的 SyncReleaseAnalyticsMilestones 依据存档补报；first_enter不是未经鉴权的唯一设备数。

scripts/ad_stats_system.lua：
- SOURCES：结算、每日福利、2倍速卡、5倍速卡、自动体验套装、黄金运单、三星定向契约、重构芯III、校准器III、挂机奖励。
- Record 通过 Add 累加；日期来自 BusinessTime，存在设备日期回退。
- LoadToday 每来源 GetRankList 前100名后求和；达到100即标 sampled。不能当作全量广告曝光/收入。
- LoadReleaseAnalytics：里程碑 GetRankTotal；各模式前100名携带额外指标。全量人数与截断次数必须区别。
- CancelPending 使用请求代号；单项 completed 防双回调。
- scripts/ad_stats_ui.lua 渲染，main.lua RefreshAdStatsPage/ShowAdStatsPage 接入。
- 不能从现有代码宣称已记录完整广告展示漏斗、所有崩溃退出或真实次日留存。重置、测试标记与历史回填也会影响分析口径。

