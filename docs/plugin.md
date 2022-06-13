# 插件

## SagerNet 插件

用于 **提供二进制文件** ，由于它们的大小，它们不能包含在主应用程序中。

用法：为你需要的协议安装插件

目前以下协议可以直接使用 SagerNet 的插件，请以应用内为准。

- [Trojan-Go Plugin](https://sagernet.org/download/#trojan-go-plugin)
- [NaïveProxy Plugin](https://sagernet.org/download/#naiveproxy-plugin)
- [Hysteria Plugin](https://sagernet.org/download/#hysteria-plugin)
- [Wireguard Plugin](https://github.com/SagerNet/SagerNet/releases/tag/wireguard-plugin-20210424-5)

这几个协议是 SagerNet 的支持，不属于高级插件，无需在「高级插件」设置中开启。

## 高级插件

Matsuri 原创的功能，允许把协议支持拆分成插件的形式，大部分情况下本体不需要改动。

高级插件由两部分组成

1. 可以在 Android 上运行的 native 二进制文件，与 SagerNet 的「插件」一致，用于提供协议出口与 socks5 入口。
2. 可以在 Android Webview 中运行的 HTML JS 文件，用于提供 UI 、分享连接、配置文件生成等功能。

由于插件需要执行权限，所以建议从信任的渠道下载任何插件（包括插件和高级插件）。

[Github Plugin 仓库](https://github.com/MatsuriDayo/plugins)

## FAQ

### 插件已安装，但显示“未找到插件”

中国大陆的互联网监控资本主义垄断巨头制作了很多App（我们称之为“毒瘤”），包含非常差的用户体验和许多恶意功能，包括在后台自启动，互相唤醒和推送广告。当他们开始生产手机时，他们将这些东西与普通应用程序一起禁止，但他们几乎总是有某种应用程序白名单。

标准的 Android 操作系统没有“自动启动”或类似的权限，因为这只是正常的通过Android四大组件之一进行跨应用调用。

对于使用垃圾系统的人，我建议调整系统中对于插件 App 的「省电」「自启动」等相关配置，自求多福。

### 因“...exits too fast”而失败

这是一条没有任何信息的通用消息，很有可能是您配置的有问题。
