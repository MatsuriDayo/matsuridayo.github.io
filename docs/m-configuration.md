# 配置

## 协议说明

#### Shadowsocks

2021 年 11 月开始，在热门线路如 Vultr DigitalOcean 部署有**针对随机 TCP 流量的封锁**，Shadowsocks（包括2022）以及 VMess TCP 等协议变得难以使用。

* [Shadowsocks是如何被检测和封锁的](https://gfw.report/talks/imc20/zh/)
* Shadowsocks 流式密码 可以被准确地主动探测、在不需要密码的情况下被审查者解密流量, 且服务端开启 IV 重放过滤器也无法缓解. 了解更多: https://github.com/net4people/bbs/issues/24

#### ShadowsocksR

* 拥有 Shadowsocks 的几乎所有问题
* 由于众所周知的原因，上游 shadowsocksr-libev 自 2017 年开始 **不再维护**

#### VMESS

* VMESS MD5 身份验证（非 AEAD）已被弃用，并且对于具有 alterId 的此类配置文件大于0，分享时会使用旧式链接
* 对于像 ALPN 这样的列表选项，格式是每行一个
* WebSocket浏览器转发不支持自定义 HTTP 域名或 TLS 设置，请保证在「地址」处填写正确的域名，并且服务器证书有效。
* Packet Encoding / XUDP: 支持 FullCone Nat （需要服务器支持）

#### Trojan

* 本质为 "Simple Socks with UDP over TCP" 详见官方文档
* 当服务端为 v2ray 时，可以使用 mux.cool，详见 mux 说明
* 支持 v2ray 功能，如 WS+TLS。与 Trojan-Go 不完全兼容。

#### (TLS) 安全设置

* 允许不安全连接：启用后安全性相当于明文。有些节点不开这个无法使用，原因是服务器证书配置有误。
* 证书（链）：应填入证书内容，通常是 PEM 格式。

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

### 日志

汇报问题时， **必须先在 设置-启用日志 中打开日志** 。
 
汇报较为复杂问题时，建议增大日志缓冲区：长按「启用日志」，建议设置为 100~200 kb。（默认为 50 kb）

### 代理链

* 代理链由客户端实现，所以会有一些性能损失
* 流量顺序是从上到下（最后一个配置为流量的出口）
* 列表中的配置可以长按拖动，向左滑动删除

### FakeDns

减少打开应用时 DNS 查询走代理而造成的延迟，但会有一定副作用，比如开关 VPN 造成域名解析失效，某些应用不兼容等。

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

* gVisor 兼容性较好，比较耗费 CPU 资源。

* System Nat 在处理 TCP 流量时利用了内核的 TCP 实现，比较节省 CPU 资源。然而，这可能会导致安卓系统的流量和速度成为实际使用的两倍或以上。

* Tun2Socket 借鉴自 Clash For Andorid，原理和 System 差不多，一般比 System 快一些。

推荐使用 Tun2Socket

### MTU

[最大传输单元](https://zh.wikipedia.org/zh-hans/%E6%9C%80%E5%A4%A7%E4%BC%A0%E8%BE%93%E5%8D%95%E5%85%83)

根据 CFA 的实践经验，默认设置为 9000，可节省CPU并提高速度。如果遇到不兼容的软件（表现为 UDP 包丢失），可以调回 1500。

### 增强域名解析

有些用户反馈机场域名经常被 DNS 污染，而机场未必及时更新，故推出此功能。

原理：同时使用数个境外常用 DoH 服务器解析服务器域名，取第一个返回的结果。

小声喵喵：如果把 DoH 服务器全墙了，或者把 Go TLS 指纹墙了，死一大片，这种方法自然就失效了。

如果不是节点不可用，不建议启用。

### UDP 行为

TLDR: 大多数情况能用

UDP 行为类似 Xray ( Shadowsocks Trojan Hysteria Socks 等均支持 FullCone )

UDP 分流只在第一个包进行，后续从同一本地端口发出的包均会走该出站。连接断开不会保持端口。
