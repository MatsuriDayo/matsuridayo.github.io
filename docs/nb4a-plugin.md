# NekoBox Android 插件

Github Plugin 仓库

[MatsuriDayo/plugins](https://github.com/MatsuriDayo/plugins)

!!! warning ""

    **由于插件需要执行代码，所以建议从信任的渠道下载任何插件**

## 插件

用于 **提供二进制文件** ，由于它们的大小，它们不能包含在主应用程序中。

用法：为你需要的协议安装插件。已安装的插件在「关于」页面显示。

### 高级插件

**不再维护的功能**，于 1.3.9 移除。

### 可以使用的插件

`moe.matsuri.exe.*`

`io.nekohasekai.sagernet.plugin.*`

- [Naive Plugin](https://github.com/klzgrad/naiveproxy/releases)
- [Hysteria Plugin](https://github.com/MatsuriDayo/plugins/releases?q=Hysteria)

## 常见问题：插件已安装，但显示“未找到插件”

中国大陆的互联网监控资本主义垄断巨头制作了很多App（我们称之为“毒瘤”），包含非常差的用户体验和许多恶意功能，包括在后台自启动，互相唤醒和推送广告。当他们开始生产手机时，他们将这些东西与普通应用程序一起禁止，但他们几乎总是有某种应用程序白名单。

标准的 Android 操作系统没有“关联启动”或类似的权限，因为这只是正常的通过Android四大组件之一进行跨应用调用。

这个问题的常见解决方法：

1. 调整 插件 App 的「省电」「关联启动」等相关权限。
2. 调整 Matsuri / NekoBox 的 「读取应用列表」 权限。

## 常见问题：因“...exits too fast”而失败

这是一条没有任何信息的通用消息，很有可能是您配置的有问题。
