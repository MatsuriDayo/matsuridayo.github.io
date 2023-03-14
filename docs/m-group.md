# 分组和订阅

## 用户界面使用

1. 使用工具栏（右上角）上的添加按钮创建分组
2. 长按排序
3. 向左滑动删除群组（或在编辑页面点击删除按钮）
4. 点击“更新”按钮更新您的订阅

## 订阅

请新建一个分组，并选择组类型为“订阅”以使用订阅。

### 订阅类型

#### 原始

常见的格式，如 base64 shadowsocks v2rayN clash yml 配置、任何包含代理链接的文本。

部分格式（如某些 iOS 应用自创的格式）由于本身没有文档且标准混乱，其他应用经常无法识别或者识别后信息丢失。遇到此类链接可以尝试用专门的订阅转换器转为 clash 格式再导入。

!!! note

    支持 "Subscription-Userinfo" 流量显示，由于「某些机场面板」只给特定的客户端返回流量信息，遇到 Clash 可以显示而本软件不能显示的，请设置 User-Agent: `ClashForAndroid/2.5.9.premium`

#### OOCv1

!!! note
    很少使用，仅 Matsuri 支持。

https://github.com/Shadowsocks-NET/OpenOnlineConfig

特征：

* 显示用户名和流量使用情况
* 强制 TLS1.3 支持固定证书
* 按组和标签过滤配置文件

#### SIP008

!!! note "流量显示"
    很少使用，仅 Matsuri 支持。

一种很少使用的 shadowsocks 订阅格式。

功能：显示流量使用情况

### 强制解析

更新时使用 系统 DNS 将所有配置的域名解析为 IP 地址。

### 重复数据删除

删除重复的配置文件。

规则：地址和端口相同即视为重复。

### 仅在连接时更新

检查设备是否连接到代理，否则将跳过更新。

手动更新时，还会显示一个确认对话框。

### User-Agent

User-Agent HTTP 请求标头是一个特征字符串，它允许服务器和网络对等方识别请求用户代理的应用程序、操作系统、供应商和/或版本。

了解更多：[用户代理 | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)

### 自动更新

由于 Android API 限制，最短更新间隔为 15 分钟。
