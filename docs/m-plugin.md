# Matsuri & NB4A 插件

Github Plugin 仓库

[MatsuriDayo/plugins](https://github.com/MatsuriDayo/plugins)

!!! warning ""

    **由于插件需要执行代码，所以建议从信任的渠道下载任何插件（包括插件和高级插件）**

## 插件

用于 **提供二进制文件** ，由于它们的大小，它们不能包含在主应用程序中。

用法：为你需要的协议安装插件。已安装的插件在「关于」页面显示。

目前除了 `Trojan-Go` 和 `NaïveProxy`，其余插件只兼容 `moe.matsuri.*` 版本（具体请以应用内能否正常识别运行为准）

### SagerNet 发布的插件

`io.nekohasekai.sagernet.plugin.*`

- [Trojan-Go Plugin](https://github.com/SagerNet/SagerNet/releases/tag/trojan-go-plugin-0.10.6) （上游不再更新）
- [NaïveProxy Plugin](https://github.com/SagerNet/SagerNet/releases?q=naive)

### Matsuri 发布的插件

`moe.matsuri.exe.*`

- [Hysteria Plugin](https://github.com/MatsuriDayo/plugins/releases?q=Hysteria)
- [TUIC(v4) Plugin](https://github.com/MatsuriDayo/plugins/releases/tag/tuic-0.8.5-2) （上游不再更新）
- [TUIC(v5) Plugin](https://github.com/MatsuriDayo/plugins/releases?q=tuic) （1.2.0 版本后不再使用）

## 高级插件

`moe.matsuri.plugin.*`

Matsuri 原创的功能，允许把协议支持拆分成插件的形式，大部分情况下本体不需要改动。

用法：在 设置 - 高级插件 查看已安装的插件，可以启用或禁用。

高级插件由两部分组成

1. 可以在 Android 上运行的 native 二进制文件，与 SagerNet 的「插件」一致，用于提供协议出口与 socks5 入口。
2. 可以在 Android Webview 中运行的 HTML JS 文件，用于提供 UI 、分享连接、配置文件生成等功能。

!!! warning "警告"
    Matsuri + Xray / sing-box plugin 等组合暂时废弃。替代方案为 NekoBox For Android

## 常见问题：插件已安装，但显示“未找到插件”

中国大陆的互联网监控资本主义垄断巨头制作了很多App（我们称之为“毒瘤”），包含非常差的用户体验和许多恶意功能，包括在后台自启动，互相唤醒和推送广告。当他们开始生产手机时，他们将这些东西与普通应用程序一起禁止，但他们几乎总是有某种应用程序白名单。

标准的 Android 操作系统没有“自动启动”或类似的权限，因为这只是正常的通过Android四大组件之一进行跨应用调用。

这个问题的常见解决方法：

1. 调整 插件 App 的「省电」「自启动」等相关权限。
2. 调整 Matsuri / NekoBox 的 「读取应用列表」 权限。

## 常见问题：因“...exits too fast”而失败

这是一条没有任何信息的通用消息，很有可能是您配置的有问题。
