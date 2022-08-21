# Matsuri FAQ

## 插件已安装，但显示“未找到插件”

解决方法请看 [插件](/plugin/)

## 为什么没网 / 为什么 xx 应用不能用

1. 节点问题
2. 设置的 远程 或 直连 DNS 不可用
3. 路由规则设置错误
4. 请看 [路由/提示](/m-route/)

## 为什么这么耗电

一些系统会统计出更多的VPN应用的耗电量。

## 为什么没有 xx 功能

* 不实用
* 没人写
* 过于复杂难以维护
* 已有更好的替代方案

### VLESS

* 使用 Trojan 可以达到相同或更好的效果

### Shadowsocks 2022

* v2fly 没有 Shadowsocks 2022 支持。
* 对于网络中间人，ss 2022 和 ss aead 的 tcp 流没有区别，照封不误。

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

### Tun2Socket
这个TUN实现较为节能。另外可以改善部分手机性能不足的情况。

### 插件加载
在某些系统上不再需要自启动权限。

### Hysteria 插件
去掉了dokodemo转发，改为由hysteria直连服务器，保证性能发挥。
