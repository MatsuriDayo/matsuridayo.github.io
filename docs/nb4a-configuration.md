# NB4A 配置

## 协议说明

### 自定义 JSON

自定义功能，适用于任何类型的服务器，请在「服务器配置」界面点击右上角的三个点。

参考[这里的说明](/n-configuration/#json-custom-outbound-json)

### 自定义配置

可以填写完整配置，也可以填写一个 outbound

如果填写完整配置，则需注意：

- tag 等属性需要与 GUI 生成的配置保持一致 (如 `proxy` `bypass`)， GUI 路由配置等功能失效
- VPN 相关配置完全由 GUI 控制， tun inbound 只需保持 IP 一致即可 (172.19.0.1/28)

### Shadowsocks

* 如果服务器是 sing-box 则支持 `smux` `uot`
* `smux` 与 `uot` 冲突，同时只能开一个

### VMESS

* VMESS MD5 身份验证（非 AEAD）已被弃用，此类配置文件的 alterId 大于 0，使用 NB4A 分享时会使用旧式链接。
* 对于像 ALPN 这样的列表选项，格式是每行一个。

### Wireguard

* 对于像 本地地址 这样的列表选项，格式是每行一个。
- 支持 reseved 字段，请填字符串。

### Hysteria

对于 sing-box 不支持的类型(比如多端口)，需要安装 Matsuri Hysteria 插件 1.3.3+

#### 多端口 / 端口跳跃

1. 需要使用多端口时，「服务器」按照 `example.com:1145,5144-10240` 格式填写，「服务器端口」随意填写。
2. 不使用多端口时，「服务器」和「服务器端口」按照原来的方式填写。
4. 不能作为链式代理的非入口节点。

### sing-box 专有协议

!!! warning "注意"

    * 这是一些专有的协议，通常随 VMess Trojan 等协议使用。
    * 如果服务器不支持（版本不同），则可能导致无法上网。


* vmess/vless/trojan: `V2Ray Transport` (http/ws/grpc/quic)
* vmess/vless: `packetaddr` `xudp`
* ss/vmess/vless/trojan: `smux`
* ss: `uot`

TLS 更改：

* uTLS
* Reality (先决条件是开启 uTLS)

### (TLS) 安全设置

!!! note "封锁提示"

    2022 年 10 月开始，TLS Tunnel 易受封锁。详细信息和对策请看 https://github.com/net4people/bbs/issues/129

* 允许不安全连接：启用后安全性相当于明文。有些节点不开这个无法使用，原因是服务器证书配置有误。
* 证书（链）：应填入证书内容，通常是 PEM 格式。
* 如果 SNI 留空，且 address 为域名，则使用 address 填充 SNI。
* 本项目不会使用 ws host 等字段来填写 SNI，这可能会使一些客户端共享的 ws tls 等配置无法使用，请自行检查。

#### uTLS

sing-box(NekoBox) 支持的范围和指纹如下，请勿与其他软件的 uTLS 配置混淆。

https://sing-box.sagernet.org/configuration/shared/tls/#utls

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

* FakeDNS 会劫持 DNS 返回 `198.18` 开头的 FakeIP， NekoBox tun 收到目标 IP 为这种类型的请求后，会把它还原为域名。
* **FakeIP 只支持 TCP 链接，不支持 UDP。**
* 对于「绕过」的流量， FakeDNS 不生效，所以没有以上问题。

### 多路复用（smux）

减少新建连接时因为各种握手而导致的延迟，但对下载等场景会有反效果。

- 目前可以为 vmess vless trojan shadowsocks 等协议启用 smux
- 是否能用取决于服务端，请以节点实际为准。若服务端不支持 smux（约等于不是 sing-box），则开启后无法上网。

### 分应用代理

仅对 VPN 模式生效。

此功能决定某应用的流量是否会由 NekoBox VPN 处理，而「路由」功能决定 NekoBox 如何处理。

### Tun 实现

由 sing-box 实现，目前有 `gVisor` `System` 两种。

对于能使用 Socks / HTTP 代理的应用，建议手动为其设置 Socks / HTTP 代理，不走 Tun 更省电。

### MTU

[最大传输单元](https://zh.wikipedia.org/zh-hans/%E6%9C%80%E5%A4%A7%E4%BC%A0%E8%BE%93%E5%8D%95%E5%85%83)

根据 CFA 的实践经验，默认设置为 9000，可节省CPU并提高速度。如果遇到不兼容的软件（表现为 UDP 包丢失），可以调回 1500。
