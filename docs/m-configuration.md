# 配置

这是 Matsuri 的使用文档，对 NekoBox for Android 不适用。

本页的某些信息可能已过期，请注意辨别。

## 协议说明

### Shadowsocks

!!! note "封锁提示"

    2021 年 11 月开始，在热门线路如 Vultr DigitalOcean 部署有**针对随机 TCP 流量的封锁**，Shadowsocks（包括2022）以及 VMess TCP 等协议变得难以使用。

* [Shadowsocks是如何被检测和封锁的](https://gfw.report/talks/imc20/zh/)
* Shadowsocks 流式密码 可以被准确地主动探测、在不需要密码的情况下被审查者解密流量, 且服务端开启 IV 重放过滤器也无法缓解. 了解更多: https://github.com/net4people/bbs/issues/24
* 截至 2023 年 2 月，以上信息可能已经过期。

### ShadowsocksR

* 拥有 Shadowsocks 的几乎所有问题
* 由于众所周知的原因，上游 shadowsocksr-libev 自 2017 年开始 **不再维护**

### VMESS

* VMESS MD5 身份验证（非 AEAD）已被弃用，此类配置文件的 alterId 大于 0，使用 Matsuri 分享时会使用旧式链接。
* 对于像 ALPN 这样的列表选项，格式是每行一个。
* WebSocket 浏览器转发不支持自定义 HTTP 域名或 TLS 设置，请保证在「地址」处填写正确的域名，并且服务器证书有效。
* 包编码: 用于实现 UDP FullCone NAT。有：Packet Encoding (服务端为 v2Ray-core v5.04+)/ XUDP (服务端为 Xray-core v1.3.0+)

### Trojan

* 本质为 "Simple Socks with UDP over TCP" 详见[原始协议文档](https://trojan-gfw.github.io/trojan/protocol)
* 现多配合 v2ray "专有协议" 使用，如 `WS+TLS` `mux.cool`，与 Trojan-Go 不完全兼容。

### VLESS

* 特性与 v2ray Trojan 重叠，Trojan 的实现和兼容性更好。
* VLESS 已被 v2fly 弃用并且可能被移除。请考虑使用 Trojan 作为替代品。[link](https://www.v2fly.org/v5/config/proxy/vless.html)

### Wireguard

经过转发的 Wiregurad 性能很差。建议直接使用 Wireguard 官方 App。

### v2ray 私有协议

!!! warning "注意"

    * 这是一些私有协议，通常随 VMess Trojan 等协议使用。
    * 如果服务器与客户端的版本不同，则可能导致无法上网。

按服务器支持的软件分（Matsuri客户端只支持v2ray的）：

* v2ray/xray: `streamSettings` (ws grpc 等)
* v2ray: `mux.cool`(详见 mux 说明) `PacketAddr`
* xray: `xudp`
* trojan-go: `smux`
* sing-box: `uot` `multiplex(smux)`

### (TLS) 安全设置

!!! note "封锁提示"

    2022 年 10 月开始，TLS Tunnel 易受封锁。详细信息和对策请看 https://github.com/net4people/bbs/issues/129

* 允许不安全连接：启用后安全性相当于明文。有些节点不开这个无法使用，原因是服务器证书配置有误。
* 证书（链）：应填入证书内容，通常是 PEM 格式。
* 如果 SNI 留空，且 address 为域名，则使用 address 填充 SNI。
* 本项目不会使用 ws host 等字段来填写 SNI，这可能会使一些客户端共享的 ws tls 等配置无法使用，请自行检查。

#### uTLS

V2Ray (Matsuri&NekoRay) 和 sing-box(NekoBox) 支持的范围和指纹如下，请勿与其他软件的 uTLS 配置混淆。

https://www.v2fly.org/v5/config/stream.html#utls

https://sing-box.sagernet.org/configuration/shared/tls/#utls

### Hysteria 多端口 / 端口跳跃

0.5.3+ 支持，使用说明如下

1. 需要使用多端口时，「服务器」按照 `example.com:1145,5144-10240` 格式填写，「服务器端口」随意填写。
2. 不使用多端口时，「服务器」和「服务器端口」按照原来的方式填写。
3. 需要安装 Matsuri Hysteria 插件 1.3.0+。
4. 不能作为链式代理的非入口节点。

## 功能说明

### 导入导出节点

另见 [分组和订阅](/m-group/)

#### 导入

目前支持从链接导入和从文件导入。程序会自动识别链接。

Wireguard .conf 配置文件可以导入。

包含分享链接的文件可以导入。

如果需要导入多个文件，可以压缩为 zip 文件，程序会尝试解压并识别其中的配置文件。

#### 导出/分享

导出配置：导出软件生成的 v2ray config.json 等配置文件。

分享链接：标准 / Matsuri，不同节点分享格式以分享菜单为准。

Matsuri 格式为程序内部的存储格式，包含的信息最完全，部分与 SagerNet 通用，但跨版本兼容性没有保证。链接为 `sn://xxxxx`

### 代理链

* 代理链由客户端实现，所以会有一些性能损失
* 流量顺序是从上到下（最后一个配置为流量的出口）
* 列表中的配置可以长按拖动，向左滑动删除

### FakeDns

减少打开应用时 DNS 查询走代理而造成的延迟，但会有一定副作用，比如开关 VPN 造成域名解析失效，某些应用不兼容等。

常见的副作用之 UDP 问题：

* FakeDNS 会劫持 DNS 返回 `198.18` 开头的 FakeIP
* 对于 0.5.8 之前的版本， FakeIP + VPN 下无法代理 UDP 流量。
* 对于 0.5.8 及之后的版本， FakeIP + VPN + UDP 会抹去远端地址信息，造成类似 v2ray 假 full cone 的效果。
* 对于「绕过」的流量， FakeDNS 不生效，所以没有以上问题。

### 多路复用（mux）

减少新建连接时因为各种握手而导致的延迟，但对下载等场景会有反效果。

- 目前可以为 vmess trojan trojan-go 等协议启用 mux

- 是否能用取决于服务端，请以节点实际为准。对于 vmess 和 trojan，若服务端不支持 mux.cool（约等于不是 v2ray），则开启 mux 后无法上网。

### 分应用代理

仅对 VPN 模式生效。

此功能决定某应用的流量是否会由 Matsuri VPN 处理，而「路由」功能决定 Matsuri 如何处理。

Matsuri 本身的流量是会经过 VPN 处理的。

目前在 `Tun 使用 System Nat` 等情况下，会自动绕过 root 用户，代价是新安装的应用必须重启 VPN 服务才能被代理。

### Tun 实现

对于能使用 Socks / HTTP 代理的应用，建议手动为其设置 Socks / HTTP 代理，不走 Tun 更省电。

### MTU

[最大传输单元](https://zh.wikipedia.org/zh-hans/%E6%9C%80%E5%A4%A7%E4%BC%A0%E8%BE%93%E5%8D%95%E5%85%83)

根据 CFA 的实践经验，默认设置为 9000，可节省CPU并提高速度。如果遇到不兼容的软件（表现为 UDP 包丢失），可以调回 1500。

### 增强域名解析

有些用户反馈机场域名经常被 DNS 污染，而机场未必及时更新，故推出此功能。

原理：同时使用数个境外常用 DoH 服务器解析服务器域名，取第一个返回的结果。

有副作用，仅作为备用选项，如果不是全部被 DNS 污染等极端情况，不建议启用。

### UDP 行为

TLDR: 大多数情况能用

UDP 行为类似 Xray ( Shadowsocks Trojan Hysteria Socks 等均支持 FullCone )

UDP 分流只在第一个包进行，后续从同一本地端口发出的包均会走该出站。连接断开不会保持端口。
