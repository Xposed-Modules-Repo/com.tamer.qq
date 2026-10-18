# QQTamer（QQ 净化助手 / QQ Keeper）

本项目目前处于停止状态。
This project is currently on hold.

> ## ⚠️ AI-generated module / 本模块由 AI 生成
> 本项目由大语言模型（AI）在人类指导下生成，包括全部 Hook 代码、设置界面、构建流水线与文档。
> 代码未经人工长期审计，请自行评估风险后使用；欢迎人工审查与 PR。
>
> This project was generated and iterated by a large language model (AI) under
> human direction, including all hook code, the settings UI, the build pipeline
> and these docs. The code has not been long-term audited by humans — evaluate the
> risk yourself; human review and PRs are welcome.

面向**手机 QQ** `com.tencent.mobileqq`（实测 **Play 版 8.2.11 / versionCode 1380**；NT 9.3.x 为兼容组）的 LSPosed 模块。
An LSPosed module for the **mobile QQ** app (`com.tencent.mobileqq`, tested against **Play 8.2.11 / versionCode 1380**; NT 9.3.x is the compatibility line).

最新发布 **v1.4.95**（versionCode 100）：省电 · 去开屏广告 · 转发/图片/加载修复 · 夜间主题修复 · 系统框架收敛 · 厂商推送注入。
Latest release **v1.4.95** (versionCode 100): power saving, splash-ad removal, forward/image/loading fixes, night-theme repair, system-framework trimming and vendor push injection.

源码与构建说明见源仓：<https://github.com/mengwuzhuanshou/QQTamer>

---

## 功能特性 / Features

| 开关 Switch (默认 Default) | 说明 / Description |
| --- | --- |
| power_block_core_keepalive (开/on) | 后台拦截并停止 CoreService(GuardManager) 前台保活；前台不受影响 / Block & stop the CoreService keep-alive in background; foreground untouched |
| power_block_bg_services (开/on) | 后台拦截游戏中心(Wadl)/云游戏/小世界发布/彩签/ilink 等杂项服务 / Block game-center & misc services in background |
| power_cap_wakelocks (开/on) | 后台 WakeLock 无限期改 60s、超长截断；音视频/导航/推送白名单 / Cap background wakelocks; audio/video/nav/push whitelisted |
| power_relax_alarms (关/off) | 后台 setExact* 降级为非精确闹钟 / Relax exact alarms in background |
| power_tombstone_mode (关/off) | 激进墓碑：进后台立即停保活与杂项服务组 / Aggressive tombstone: stop keep-alive & misc services on background |
| power_tombstone_keep_push (开/on) | 墓碑/后台拦截时保留消息推送保活（MSF/推送服务/推送唤醒锁豁免） / Keep message-push alive under tombstone (MSF/push services/wake-locks exempt) |
| ad_block_splash (开/on) | 拦截开屏广告：掐断 vas-splash 缓存投喂与广告跳转闸门 / Block splash ads at cache-feed & jump-gate |

所有开关可在设置页独立切换；总开关 master_enabled 关闭后模块完全休眠。
Every switch is independently reversible in the settings UI; the master switch disables the whole module.

### v1.4.x 新增 / Added since v1.1.5

- 转发与消息：合并转发/嵌套转发预加载与重试、单会话分页（本地加宽，不动服务端请求）、气泡换行修复；
- 图片：全原图档（气泡/查看器，按原图长边钳制显示盒）、图片预加载、缩略图放大；
- Play 8.2.11 专修：夜间模式主题一键修复（9 套主题 + currentThemeId 回写）、主题下载修复、升级守卫；
- 其他修复：分享卡片/跳转、视频地址 host 字段、doze 白名单一键移除（需 root）；
- 系统框架收敛（可选，system_server 内生效，默认关）：夜间精确闹钟降级、拒绝 WakeLock、拦截后台服务，按包名收敛，需重启；
- 厂商推送注入（可选）：荣耀/小米/OPPO 已实机，vivo 无设备未实证（纯 LSP 注入，不重签宿主）。

## 设计原则（风控优先）/ Design principles (risk-control first)

- 只做 Java 层钩子，不碰 native 库、不碰 MSF 协议与登录链路 / Java-layer hooks only; never touch native libs, MSF protocol or the login path;
- 省电类钩子仅后台生效，前台体验零改动 / Power hooks act only in background; zero foreground impact;
- 不做任何「返回假数据」式欺骗 / no fake-return deception of any kind;
- 全部开关独立容错，任一 Hook 失败不影响宿主运行 / Every hook is fault-isolated; a failed hook never breaks the host.

## 安装使用 / Installation

1. Magisk/KernelSU + Zygisk + LSPosed 环境 / rooted device with Zygisk + LSPosed;
2. 安装本仓 Releases 里的最新 APK（v1.4.95）/ install the latest release APK (v1.4.95);
3. LSPosed 中启用模块，作用域勾选 **QQ(com.tencent.mobileqq)**；启用「系统框架收敛」时再勾 **Android 系统** 并重启 / select the QQ scope; additionally select **Android System** (and reboot) only when using the system-framework guard;
4. 打开模块桌面图标可调开关（纯代码 UI，无资源依赖）/ open the module's settings icon (pure-code UI);
5. **强制停止 QQ 后重新打开**生效 / force-stop QQ and reopen for changes to take effect.

日志：logcat/LSPosed 日志过滤 `QQTamer`；存活标记写入 QQ 自己的 files 目录（`qq_tamer_alive.txt`）。
Logs: filter `QQTamer` in logcat / LSPosed log; a liveness marker is written to QQ's files dir.

## 已知限制 / Known limitations

- GDT(gdtad) 实时拉新开屏未拦（只掐 vas-splash 缓存投喂与跳转闸门）/ GDT real-time splash fetch not blocked (covers vas-splash cache feed & jump gate);
- 消息流/空间/小世界信息流广告未处理 / AIO feed / Qzone / mini-world in-feed ads not handled;
- 仅主进程生效（:tool/:miniapp/:qzone 等子进程跳过）/ main process only (sub-processes skipped);
- 仅 Play 版 QQ 8.2.11 全量实测；NT 9.3.x 为兼容组，1.4.x 的 Play 专修项不适用 / fully tested on Play QQ 8.2.11; the NT 9.3.x line is compatibility-only and the Play-specific fixes do not apply;
- QQ 升级可能使混淆锚点漂移，升级后先看 `hook FAILED` 日志 / obfuscated anchors may drift on QQ updates — watch `hook FAILED` logs after upgrades.

## 鸣谢 / Acknowledgments

本模块的实现思路与安全取舍参考了以下项目，在此致谢（仅参考思路，未复制代码；第三方许可证详见各自仓库）：
This module's approach and risk trade-offs are inspired by the following projects (ideas only, no code copied; see each repo for its license):

| 项目 / Project | 贡献 / Contribution | 链接 / Link |
| --- | --- | --- |
| **NTQQBattery** (com.wkeqin.ntqqbattery) | 墓碑模式与后台服务收敛思路、beacon 后台静默、模块结构参考 / tombstone-mode & background service trimming, background beacon quieting, module structure | https://modules.lsposed.org/module/com.wkeqin.ntqqbattery |
| **QQNTHookBypass**（QQNT 过环境检测, io.github.jhl337.qqhook） | 环境检测对抗要安静地「说谎」而非硬崩检测 / quiet env-detection deflection instead of hard crashes | https://modules.lsposed.org/module/io.github.jhl337.qqhook/ |
| **qqmax（手表QQ魔改 M2.5）** | 「没动内核」的纯 Java/smali 层改动面，风控最安全的注入范式 / pure-Java modification surface without touching native — the risk-safest injection pattern | 设备端魔改包（未开源）/ device-side mod (not open-source) |
| **TSBattery** (com.fankes.tsbattery) | 后台唤醒抑制与省电模块生态的祖师爷 / the origin of background wake-suppression power modules | https://github.com/Xposed-Modules-Repo/com.fankes.tsbattery |
| **HonorMarketTamer** (mengwuzhuanshou/HonorMarketTamer) | 无 SDK 构建管线、配置三副本方案、容错 Hook 模式与踩坑体系 / no-SDK build pipeline, 3-copy config scheme, fault-tolerant hook patterns & pitfall corpus | https://github.com/mengwuzhuanshou/HonorMarketTamer |

## 许可证 / License

MIT © mengwuzhuanshou。详见 LICENSE。/ MIT © mengwuzhuanshou. See LICENSE.
