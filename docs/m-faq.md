# Matsuri FAQ

## 插件已安装，但显示“未找到插件”

解决方法请看 [插件](/m-plugin/)

## 如何添加订阅

分组→创建分组→分组类型：订阅

## 为什么没网 / 为什么 xx 应用不能用

1. 节点问题
2. 设置的 远程 或 直连 DNS 不可用
3. 路由规则设置错误
4. 请看 [路由/提示](/m-route/)
5. FakeDNS 等设置改变后，建议关闭相关应用的后台，然后再打开，避免造成玄学问题。
6. 尝试调整「IPv6路由」
7. 请看 [配置/专有协议](/m-configuration/)

## 如何正确反馈问题

如果遇到问题，请通过 GitHub issue 提交完整日志。

汇报问题流程：

1. 认真查看文档和历史 issue 以确认这不是你的配置问题
2. 设置中打开「启用日志」
3. 复现你的问题
4. 发送日志

汇报较为复杂问题时，建议增大日志缓冲区：长按「启用日志」，建议设置为 100~200 kb。（默认为 50 kb）

### 日志显示 InitCore called ......

这是启动信息记录。如果日志只有此类信息，说明未开启日志记录。

## 为什么这么耗电

一些系统会统计出更多的VPN应用的耗电量。

## 为什么没有 xx 功能

* 不实用
* 没人写
* 过于复杂难以维护
* 已有更好的替代方案

### VLESS

* 特性与 v2ray Trojan 重叠，Trojan 的实现和兼容性更好。
* VLESS 已被弃用并且可能被移除。请考虑使用 Trojan 作为替代品。[link](https://www.v2fly.org/v5/config/proxy/vless.html)

### Shadowsocks 2022

* v2fly 没有 Shadowsocks 2022 支持。
* 对于网络中间人，ss 2022 和 ss aead 的 tcp 流没有区别，照封不误。

## 无法启动代理

### 提示 ... pattern string does not conform to Letter-Digit-Hyphen (LDH) subset

请逐条检查路由规则：域名规则中是否包含特殊字符、不可见字符。

### 提示 失败:: invalid rule

请逐条检查路由规则：是否有使用了已删除的服务器。

### 提示 ... geosite.dat: no such file

这是因为缺少路由资源，可能是被非标准系统或“手机管家”删除。

应用会在后台自动解压 geosite，请等待一段时间。

如果问题持续，建议「更新路由资源」或重装应用。

## 特色功能

本项目源于[SagerNet](https://github.com/SagerNet/SagerNet)，并做了很多优化和修改。由于维护者的精力有限，SagerNet的一些新功能可能不会被合并到这个项目中。

### UI 快捷方式

* 搜索配置
* 删除不可用配置
* 移动配置到指定分组
* 更新全部订阅
* 对于某些订阅：显示流量和到期信息

### FakeDNS

* 被 SagerNet 删除的 FakeDNS 在这里保留，并进行了优化。
* 在 VPN 模式下，这是非常实用的功能，可以避免不必要的 DNS 解析，降低延迟（走远程DNS 虽然有缓存，最初的请求也要耗费掉至少1-RTT时间）。
* 对于某些 app 可能有副作用。

### 高级插件
可以管理一些 socks5 代理软件而无需更改 Matsuri App，适合爱折腾的用户。

### DNS 模块优化
与 SagerNet 重写 v2ray dns 模块不同。Matsuri 在 v2ray dns 模块上改善了 DoH KeepAlive 等问题，更加稳定。

### Tun 模块优化
添加了 Tun2Socket，优化了部分实现。节约性能，改善部分手机性能不足的情况。

### 插件加载
在某些系统上不再需要自启动权限。

### Hysteria & TUIC 插件
去掉了 dokodemo 转发，改为由插件直连服务器，保证 QUIC 协议的性能发挥。

### 分享链接格式
可以识别更多常见应用生成的分享链接。
