# NB4A 配置

这是 NekoBox for Android 的详细说明书，跟随软件更新，**阅读前请保证您使用的是最新版**。

## 协议说明

### 自定义 JSON

自定义功能，适用于任何类型的服务器，请在「服务器配置」界面点击右上角的三个点。

需要先保存服务器。否则点击右上角菜单无反应。

参考[这里的说明](/n-configuration/#json)

### 自定义配置

可以填写完整配置，也可以填写一个 outbound

如果填写完整配置，则需注意：

- outbound tag 等属性需要与 GUI 生成的配置保持一致 (如 `proxy` `bypass`)， GUI 路由配置等功能失效
- VPN 相关配置完全由 GUI 控制， tun inbound 只需保持 IP 一致即可 (172.19.0.1/28)

### Shadowsocks

* 如果服务器是 sing-box 则支持 `multiplex` `uot`
* `multiplex` 与 `uot` 冲突，同时只能开一个

### HTTP

部分情况下与 v2ray 等服务器存在兼容性问题。

* 截至 2023 年 6 月 sing-box HTTP 出站未支持 HTTP/2 CONNECT (HTTP+TLS) 代理，此类代理无法使用（表现为 EOF 错误）
* 截至 2023 年 6 月 sing-box 未支持 v2ray "TCP Header 伪装 HTTP" (VMESS+TCP+HTTP+TLS) 类型代理，此类代理无法使用（表现为 EOF 错误）

### VMess

* 对于像 ALPN 这样的列表选项，格式是每行一个。1.1.7 版本后也可以用 `,` 分割。

### Wireguard

* 本地地址 每行一个。 格式如 `10.0.0.1/24` `2001:db8::1/32`
* 支持 reseved 字段，请填 base64 字符串。

### Hysteria1

对于 sing-box 不支持的类型(比如 wechat-video)，需要安装 Matsuri Hysteria 插件 1.3.3+

#### 多端口 / 端口跳跃

1. (1.2.0之前的旧格式) 需要使用多端口时，「服务器」按照 `example.com:1145,5144-10240` 格式填写，「服务器端口」随意填写。
2. (1.2.0新格式) 服务器和端口分开填写，端口可以填写字符串，格式： `11451`（单端口）/  `114-514,1919`（多端口）。旧配置自动升级，但不可再写旧的格式。
3. 不使用多端口时，「服务器」和「服务器端口」按照原来的方式填写。
4. 不能作为链式代理的非入口节点。

### Hysteria2

Hysteria2 使用 sing-box 内置实现，暂不支持使用端口跳跃。

### TUIC

因为一些用户报告了被 GFW 封锁，以及使用与维护方面的不便，Tuic v4 v5 Plugin (基于 https://github.com/EAimTY/tuic) 客户端已在 1.2.4 版本被彻底废弃。

#### v5 协议 (1.2.0之后)

1.2.0+ 支持无需插件的 TUIC，由 sing-box 实现。

如果需要使用 `udp_over_stream` 一项（场景：可能对某些软件的实时UDP视频串流有帮助），请使用 sing-box 作为 TUIC 服务器。

### ShadowTLS

通常需要与 ShadowSocks 类型服务器组成链式代理才能上网。顺序为 ShadowTLS -> ShadowSocks

sing-box ShadowTLS 与原版的兼容性未知。

### sing-box 私有协议

!!! warning "注意"

    * 这是一些私有协议，通常随 VMess Trojan 等协议使用。
    * 如果服务器不支持（通常是因为不是 sing-box 或者版本不同），则可能导致无法上网。

按 sing-box 的出站分：

* vmess/vless/trojan: `V2Ray Transport` (http/ws/grpc/quic)
* vmess/vless: `packetaddr` `xudp`
* ss/vmess/vless/trojan: `multiplex`
* ss: `uot`

!!! note

    * ws 地址支持 `?ed=2048` 这种 earlydata 形式（与 Xray 兼容）

### TLS 安全设置

!!! note "封锁提示"

    2022 年 10 月开始，TLS Tunnel 易受封锁。

* 允许不安全连接：启用后安全性相当于明文。有些节点不开这个无法使用，原因是服务器证书配置有误。
* 证书（链）：应填入证书内容，通常是 PEM 格式。
* 如果 SNI 留空，且 服务器地址 为域名，则使用 服务器地址 填充 SNI。(v2ray & sing-box 行为)
* 本项目不会使用 ws host 等字段填充 SNI，这可能会使一些客户端（V2RayNG）共享的 ws tls 等配置无法使用，请自行检查。

### TLS 伪装设置

* uTLS
* Reality (先决条件是开启 uTLS，不支持 SpiderX，ShortId 可留空)

!!! note

    * sing-box Reality 目前与 Xray-core 兼容性欠佳，建议服务器和客户端使用相同的实现。

## 功能说明

### 导入 / 导出 / 订阅

NekoBox for Android 支持的格式:

- 「Clash格式」一般带有流量信息，本项目支持解析其节点（推荐使用）
- 「Clash Meta格式」 VLESS & Hysteria & TUIC（NB4A 1.1.4+ 支持，推荐使用）
- 「V2rayN格式」一般不带流量信息，本项目支持解析
- 「Shadowsocks格式」本项目支持解析
- 一些通用的分享链接及其 Base64 编码后的格式

NekoBox for Android 不支持的格式：

- 「某些苹果应用自创的格式」
- 「SSR格式」
- 「SIP008/OOCv1」
- 各种较少使用/不标准的格式

另见 [分组和订阅](/m-group/)

#### 导入

目前支持从链接导入和从文件导入。程序会自动识别链接。

Wireguard .conf 配置文件可以导入。

sing-box .json 配置文件可以导入（导入为自定义配置）。

包含分享链接的文件可以导入。

如果需要导入多个文件，可以压缩为 zip 文件，程序会尝试解压并识别其中的配置文件。

#### 导出/分享

导出配置：导出软件生成的 sing-box config.json 等配置文件。

分享链接：`标准` / `SN Link`，不同节点分享格式以分享菜单为准。

SN Link 格式 (`sn://`) 为程序内部的存储格式，包含的信息最完全，但跨版本兼容性没有保证。

**请勿尝试导入其他软件（或本软件较旧版本）的 `sn://` 链接和备份信息。**

### 分享链接说明

vmess vless trojan 的分享链接非常混乱，在 NekoBox for Andoird 0.7+，采用如下的分享链接策略。

导入尽量兼容常用软件的格式。

导出（基本与 nekoray v2rayN 等软件的行为兼容）

- VMess 导出的链接为 v2rayN/G 的格式 `vmess://(base64)`
- VLESS & Trojan 导出的格式参考 https://github.com/XTLS/Xray-core/discussions/716 ，其中：

  1. 遇到 http 1.1 的传输类型时，设置 `type=tcp&headerType=http` 以及 `host=xxxx`
  2. 为 VLESS 设置 `encryption=none` `flow=xxx`, Trojan 不设置
  3. 允许设置 `allowInsecure=1`
  4. v2fly ws earlydata: `ed=2048&eh=xxx`

### 代理链

* 代理链由客户端实现，所以会有一些性能损失
* 流量顺序是从上到下（最后一个配置为流量的出口）
* 列表中的配置可以长按拖动，向左滑动删除

### 多路复用（multiplex）

减少新建连接时因为各种握手而导致的延迟，但对下载等场景会有反效果。

- 目前可以为 vmess vless trojan shadowsocks 等协议启用 multiplex
- 是否能用取决于服务端，请以节点实际为准。若服务端不支持 multiplex（约等于不是 sing-box），则开启后无法上网。

#### padding

(1.1.5)+ 客户端开启 padding 可以缓解 TLS in TLS 包长度分布特征。

请在 mux 协议中勾选 `padding` 一项，服务器版本要求 sing-box 1.3.0+

### 分应用代理 / 路由

分应用代理仅对 VPN 模式生效。此功能决定某应用的流量是否会由 NekoBox VPN 处理，而「路由」功能决定 NekoBox 如何处理。

更多请看 [路由与 DNS](/nb4a-route/)

#### 绕过局域网

可以在 IP 层面（绕过局域网） 或 sing-box 路由层面（在 Core 中绕过局域网）绕过，一般开启前者即可。

LineageOS 等系统的 VPN 热点功能与前者冲突，需要开启后者。

### DNS

可以使用的 DNS 服务器类型请参考[这里](https://sing-box.sagernet.org/configuration/dns/server/#address) （DHCP & FakeIP 除外）

详细的 DNS 配置说明请看 [路由与 DNS](/nb4a-route/)

!!! note

    * 直连 DNS 建议填写 `local` 使用本机的 DNS（一般由运营商提供），如果有问题再换其他 DNS 服务器。[支持的 DNS 服务器类型](https://sing-box.sagernet.org/zh/configuration/dns/server/#address)
    * For some Iran users, you may consider prefer using DoH instead of local DNS, because of the DNS poisoning & MITM attack.

#### FakeDns

减少打开应用时 DNS 查询走代理而造成的延迟，但会有一定副作用，比如开关 VPN 造成域名解析失效，某些应用不兼容等。

* FakeDNS 会劫持 DNS 返回 `198.18` 开头的 FakeIP， NekoBox tun 收到目标 IP 为这种类型的请求后，会把它还原为域名。
* 对于 1.1.8 之前的版本， FakeIP 下无法代理 UDP 流量。
* 对于 1.1.8 及之后的版本， FakeIP + UDP 会抹去远端地址信息，造成类似 v2ray 假 full cone 的效果。
* 对于「绕过」的流量， FakeDNS 不生效，所以没有以上问题。

### Tun 实现 / Stack

主要用于实现 Tun TCP 数据流重组，对应配置中的 `stack` 项。

* 由 sing-box 实现，目前有 `gVisor` `System` `Mixed`
* System 复用内核的 TCP 协议栈。对于 TCP 为主的流量，使用 System 一般比 gVisor 省电
* gVisor 为纯用户空间实现，兼容性更好
* 1.2.x 版本起默认使用 Mixed (SystemTCP + gVisorUDP)

### 追加 HTTP 代理

对于能使用 Socks / HTTP 代理的应用，建议手动为其设置 Socks / HTTP 代理，或开启 `追加 HTTP 代理到 VPN` 选项，不走 Tun 更省电。

这种用法存在的问题请看[白话文-国内外分流](/nb4a-bhw-domestic) 进阶：DNS 部分

### MTU

[最大传输单元](https://zh.wikipedia.org/zh-hans/%E6%9C%80%E5%A4%A7%E4%BC%A0%E8%BE%93%E5%8D%95%E5%85%83)

根据 CFA 的实践经验，默认设置为 9000，可节省CPU并提高速度。如果遇到不兼容的软件（表现为 UDP 包丢失），可以调回 1500。

### sing-box Dashboard

本质是一个 Clash Dashboard Web 界面，目前内置了 yacd-meta，也可以点右上角菜单自行更换 URL 以使用其他面板。

* 需要打开 Clash API 才能使用。
* 可以和 selector 分组配合使用，以获得更好的效果。
* 如果显示白屏，请更新 System Webview 或使用支持的浏览器打开 http://127.0.0.1:9090/ui/

### 分组配置: selector 模式

伪 clash selector 模式，主要优点实现是分组内免重载切换配置。

* 开启时，出站的 tag 将生成人类可读的名字。
* 不建议为包含 **需要插件的服务器** 的分组启用，因为这样会一次性启动所有插件。
* 切换节点时会重置连接，但不会清除旧的 DNS 记录，在节点物理距离过大时可能会影响访问速度。
* 开启时，"重启代理" 按钮无效。

### Root CA 侧加载

1.1.2+支持，用以应对低版本安卓设备不认可已过期的DST Root CA X3证书且系统CA没有ISRG Root X1的情况。

侧加载步骤：

1. 把 CA 证书文件，放到 `/sdcard/Android/data/moe.nb4a/files/ca.pem`
2. 重启，程序启动时将自动加载这个文件中的证书（替换system证书）

检验方法：开启 loglevel ，在日志中可查看是否加载成功。

### NAT 类型测试

此工具基于 go-stun 修改，进行 RFC3489 RFC5389 两种测试。

额外添加：针对 VMess VLESS 协议的 "Fake FullCone" (远端 IP 不变化) 检测。

检测结果仅供参考，结果可能因网络丢包等原因不准确。

Windows 电脑上可以使用此软件进行 NAT 类型测试： https://github.com/HMBSbige/NatTypeTester
