# Surge 去开屏广告模块

针对 iOS 常用 App 的开屏广告(splash ad)去除方案,Surge 专用,共两个模块:

| 模块 | 原理 | 是否需要 MITM | 覆盖范围 |
|---|---|---|---|
| [SplashAdBlock.sgmodule](SplashAdBlock.sgmodule) | 从 DNS/连接层直接拦截穿山甲、优量汇、百青藤、快手联盟等广告 SDK 域名,广告无法下发,App 直接跳过开屏 | ❌ 不需要,开启即用 | 所有接入了主流广告 SDK 的 App(覆盖面最广) |
| [SplashAdBlock-MITM.sgmodule](SplashAdBlock-MITM.sgmodule) | 对 70+ 常用 App 的开屏广告接口逐一做 URL 重写,直接返回空广告 | ✅ 需要,并信任证书 | 淘宝/京东/拼多多/美团/百度系/滴滴/携程等 70+ 常用 App |

> 💡 建议两个模块同时启用:SDK 拦截版负责「广」,MITM 增强版负责「精」。
> 个别 App 广告请求被拦截后可能短暂白屏 1~2 秒(等待超时),属正常现象。

## 安装方式

### 方式一:Surge 模块链接(推荐)

Surge → 模块 → 添加模块,填入以下 URL:

```
https://raw.githubusercontent.com/wuquan0527/Surge-Modules/main/SplashAdBlock.sgmodule
https://raw.githubusercontent.com/wuquan0527/Surge-Modules/main/SplashAdBlock-MITM.sgmodule
```

### 方式二:手动下载

下载 `.sgmodule` 文件后用 Surge 打开导入。

## MITM 增强版启用步骤

1. 启用 `SplashAdBlock-MITM.sgmodule` 模块
2. Surge → 设置 → MITM → 安装并信任 CA 证书(设置 → 通用 → 关于本机 → 证书信任设置)
3. 重新打开目标 App(建议先杀掉后台),开屏广告即消失

> 若不信任证书,启用 MITM 后所有加密流量会解密失败,请务必完成证书安装。
> 不需要 MITM 时,只开第一个模块即可,零配置。

## 常见问题

**Q: 启用后某些 App 开屏还是出现广告?**
A: 该 App 可能使用私有广告 SDK 或自建广告下发接口。请到 [Issues](https://github.com/wuquan0527/Surge-Modules/issues) 反馈 App 名称,或自行在 MITM 版模块中添加对应接口规则。

**Q: 拦截后 App 白屏/加载变慢?**
A: 广告请求被拒绝后 App 需要等待超时。一般 1~2 秒后自动跳过,属正常。若长时间卡住,说明该 App 不兼容此方案,可在模块中注释掉对应规则。

**Q: 为什么第一个模块不用 MITM 也能拦广告?**
A: 国内主流 App 的开屏广告几乎都来自第三方广告 SDK(穿山甲、优量汇等),这些 SDK 的服务器域名是固定的。在连接层直接拒绝这些域名,广告素材和广告配置都拿不到,App 就会直接进入主界面。

## 维护说明

- 主模块的 SDK 域名清单来源于社区长期维护的规则(参考 [fmz200/wool_scripts](https://github.com/fmz200/wool_scripts)、[blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script))
- MITM 版接口规则精选自 [ddgksf2013(墨鱼去开屏2.0)](https://github.com/ddgksf2013) 的 400+ App 抓包规则
- 规则会随 App 版本更新而失效,欢迎提交 Issue / PR 更新

## 致谢

- [ddgksf2013 / 墨鱼去开屏2.0](https://github.com/ddgksf2013/ddgksf2013)
- [fmz200 / wool_scripts](https://github.com/fmz200/wool_scripts)
- [blackmatrix7 / ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
- [Hey-sayiwanna / Surge-Modules](https://github.com/Hey-sayiwanna/Surge-Modules)

## 免责声明

本项目仅供学习交流使用,请勿用于商业用途。拦截规则可能随 App 更新失效,使用即代表接受由此带来的任何后果。
