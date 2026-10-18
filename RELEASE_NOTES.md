## v1.4.95

> 汇总 v1.1.5 之后所有设备版迭代（versionCode 100）。宿主：Play 版 QQ 8.2.11（versionCode 1380）；NT 9.3.x 为兼容组。
> Cumulative release of all device builds since v1.1.5. Host: Play QQ 8.2.11 (versionCode 1380); NT 9.3.x is the compatibility line.

- **转发与消息**：合并转发/嵌套转发预加载与重试、单会话分页（本地加宽，不动服务端请求）、气泡换行修复。
- **图片**：全原图档（气泡/查看器，按原图长边钳制显示盒）、图片预加载、缩略图放大。
- **Play 8.2.11 专修**：夜间模式主题一键修复（9 套主题 + currentThemeId 回写）、主题下载修复、升级守卫、分享卡片/跳转与视频地址 host 修正。
- **省电与系统框架**：保留原有后台收敛；新增 system_server 夜间收敛（闹钟/WakeLock/后台服务，默认关，需重启）与 doze 白名单一键移除（需 root）。
- **厂商推送注入**：荣耀/小米/OPPO 已实机，vivo 无设备未实证；纯 LSP 注入，不重签宿主。

---

## v1.1.5（versionCode 7）

⚠ AI-generated module / 本模块由 AI 生成，代码未经人工长期审计，请自行评估风险。

- **移除热更新守卫**（hotfix_disable_redirect / hotfix_block_dexpatch）：实机确认会触发风控，价值太低，
  连同 QFix 重定向失效、补丁安装拦截与 Tinker 服务过滤一并删除。
- **移除伪装回报**（report_clean_env / report_quiet_beacon_bg / report_clean_beacon_root）：
  环境探测清洗与 isRooted 清洗全部删除——不做任何「返回假数据」式欺骗。
- 设置页同步移除「伪装回报」「热更新守卫」两个分组；模块功能收敛为 **省电 + 去开屏广告**。
- 顺带收编 v1.1.4 未提交改动：confSrc 配置来源日志、debug_alive_marker 存活标记开关、
  conf 文件 BOM 兼容与 su 同步结果日志。

## v1.1.3（versionCode 5）

⚠ AI-generated module / 本模块由 AI 生成，代码未经人工长期审计，请自行评估风险。

- **更换签名密钥**：旧签名密钥密码曾泄露，已废弃并换新钥重新签名；
  与 1.1.2 及更早版本签名不兼容，安装前请先卸载旧版本。
- 墓碑模式新增「保留消息推送保活」开关（默认开）：MSF/推送服务/推送唤醒锁豁免，收消息不受影响。
- WakeLock 豁免判定加实例级缓存，省去重复反射。
- 前后台转换日志改为每冷启动各记一次，QQ 频繁切换不再刷日志。
- 构建引擎统一到工作区共享构件 common/，仓库不含任何本地路径与密钥。
