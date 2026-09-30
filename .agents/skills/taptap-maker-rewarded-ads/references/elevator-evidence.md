# 电梯调度项目证据
核对日期：2026-09-27；本地 HEAD：9cfd362。来源目录 D:/WorkSpace/AIProject/Games/elevator-dispatch，仅作为实例定位；其他项目需重新确认API与配置。

- scripts/main.lua：ShowRewardedAd、StartRewardedAdAttempt、CompleteRewardedAdRequest、UpdateRewardedAdRequestTimeout、QueueRewardedAdResult。
- 请求含 serial/owner/attempt/completed；重复回调与旧尝试拦截。加载超时释放UI但允许后续成功；成功分支调用 AdStatsSystem.Record，再执行 onReward。
- 来源参数：加载10秒、回调延迟0.45秒、前台恢复宽限1.5秒；最多2次尝试，重试间隔至少3.2秒。属于项目参数，不是平台规范。
- ArmRewardedAdAudioRecovery：4秒有限窗口、0.35秒检查、0.7秒后一次强恢复。scripts/audio_runtime.lua 的 RecoverAfterInterruption/ResumeAudio 处理设备与声源。
- 本次检索未发现专用广告预加载调用，不可把“即时广告预热”写成已实现功能。
- .codex_tmp 中 run_audio_ad_resume.cjs、verify_ad_retry.cjs、verify_summary_ad.cjs 可作本项目验证线索，临时测试可能过期，不作为通用依赖。
- 历史用户多次反馈 iPhone 广告后失声；当前恢复代码存在不等于所有机型已修复，仍需真机重复验证。
