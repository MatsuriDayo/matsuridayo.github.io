# NekoRay / NekoBox for PC 配置

## 窗口界面使用

如图所示

<img src="/assets/images/nekoray1.jpg" />

* Ctrl+F 进入搜索， Esc 退出。
* 选择节点时按住 Ctrl 可一次性选择多个节点。按住 Shift 选择从当前节点到选中节点之间的全部。

其他快捷键右键菜单已标明。

## NekoBox

sing-box GUI 电脑客户端。目前包含在 nekoray 程序中。

### 我该选择哪个核心

* Xray 使用广泛，兼容性较好
* sing-box 支持的更多的新协议，而且在某些情况下性能更好

### GUI 切换

在 `基本设置 - 核心` 切换核心。切换前后变化大，不建议来回切换。

如果已有的某个服务器在切换后无法使用，建议删除后手动添加。

### NekoBox 使用注意

1. 简易路由按照 v2ray 传统格式填写，如 `geoip:cn` `domain:google.com`，这类规则 nekobox 会自动翻译为 sing-box 的规则。
2. 自定义 JSON 配置（路由、出站等）请按照 sing-box 的格式填写。
3. 2.27版本起，Hysteria GUI 配置将使用 sing-box 核心(如果配置类型支持)。

### Clash API / sing-box dashboard 说明

功能介绍：方便查看流量和连接信息。可选 Web 实现，界面比较美观。

使用上类似手机 NekoBox sing-box dashboard 但有以下区别：

* 没有自带面板，需要自己下载解压，或者联网使用
* 没有 selector 功能，不能显示所有节点

启用方式：

1. 在 NekoBox 基本设置 - 核心 - 核心选项 设置 Enable Clash API
2. 启动代理
3. 解压 Clash 面板至 nekoray/config/dashboard
4. 在浏览器打开 http://127.0.0.1:9090
5. 也可以直接联网使用(跳过3和4步骤)，如直接使用 Yacd-meta: http://yacd.metacubex.one

## 导入 / 导出 / 订阅

支持的格式:

- 「Clash格式」一般带有流量信息，本项目支持解析其节点（推荐使用）
- 「Clash Meta格式」 VLESS & Hysteria（NekoBox 2.26+ 支持，推荐使用）
- 「V2rayN格式」一般不带流量信息，本项目支持解析
- 「Shadowsocks格式」本项目支持解析
- 一些通用的分享链接及其 Base64 编码后的格式

不支持的格式：

- 「某些苹果应用自创的格式」
- 「SSR格式」
- 「SIP008/OOCv1」
- 各种较少使用/不标准的格式

Neko 格式为程序内部的存储格式，包含的信息最完全，但跨版本兼容性没有保证。链接为 `nekoray://xxxxx`

## VPN / TUN 设置

(1.1+) VPN / TUN 模式可以代理整个系统的流量，适合某些软件“不听话”不走代理的情况。

目前支持在 Windows / Linux 自动配置 VPN (包括 tun 接口和路由规则等)

* IP CIDR 和 进程名，格式如 `10.0.0.0/8` `Telegram.exe` ，一行一个。
* Tun 模式下建议开启「流量探测」，设置为「探测结果用于路由判断」，有助于匹配域名规则。
* Tun 模式下可以开启「FakeDNS」加速 DNS 查询
* 在 Windows 系统，由于上游问题，Tun 模式有概率启动失败。如连续多次不能启动，请重启并清理多余的网络接口后再试。
* (仅限外挂模式) 程序会读取配置模板 `config/vpn/sing-box-vpn.json`（如果此文件存在，否则使用默认模板）用于生成 sing-box 设置，可以参考[默认模板](https://github.com/MatsuriDayo/nekoray/blob/main/res/vpn/sing-box-vpn.json)自行修改。

### 内部 Tun

关闭 内部 Tun （外挂模式）： 启动 Tun 时，启动 nekobox_core 执行 "虚拟网卡转 socks 功能"。 NekoRay 只能使用这种模式。

开启 内部 Tun （内置模式）： 无需额外启动 nekobox_core，只需要一个 core 进程，虚拟网卡直接作为入站，不需要转 socks，提高 Tun 工作效率。 NekoBox 默认使用此模式。

隐藏控制台：外挂模式下，隐藏 Windows 系统的控制台窗口。

### 白名单模式

无论是黑名单（默认代理，列表内直连）还是白名单（默认直连，列表内代理），您的流量都将由 nekobox_core (sing-tun) 处理。这不等于某些软件的「进程模式」。

## 基本设置

### URL 测试

支持 http 或 https 协议

与 Matsuri 一致，如果开头为 `true` 则表示测试真延迟 RTT，否则测试握手延迟。

真延迟 `truehttps://cp.cloudflare.com/`

握手延迟 `https://cp.cloudflare.com/`

### 自定义入站

[Xray入站语法](https://xtls.github.io/config/inbound.html)

[sing-box入站语法](https://sing-box.sagernet.org/configuration/inbound/)

示例 (Xray dokodemo-door)

```json
{
    "inbounds": [
        {
            "listen": "127.0.0.1",
            "port": 14514,
            "protocol": "dokodemo-door",
            "settings": {
                "address": "www.google.com",
                "followRedirect": false,
                "network": "tcp",
                "port": 443,
                "timeout": 0
            },
            "tag": "map-www.google.com-443-to-14514"
        }
    ]
}
```

### 资源文件路径

允许设置 Xray / sing-box 资源文件夹。便于使用额外的规则文件，以及自动更新和管理 geo 资源文件（如使用v2rayN、软件包管理器或升级脚本）。

* Xray 使用的是 .dat 后缀的 geo 资源文件
* sing-box 使用的是 .db 后缀的 geo 资源文件
* Loyalsoldier 版 v2ray dat 下载地址 https://github.com/Loyalsoldier/v2ray-rules-dat
* Loyalsoldier 版 sing-box db 下载地址 https://github.com/soffchen/sing-geoip https://github.com/soffchen/sing-geosite

## 路由设置

### 简易路由

* 内置了 `全局` `绕过局域网和大陆` 两种规则
* 以 # 开头的行不生效
* 不支持 `多节点分流` `负载均衡` 这类用法，若有需求请换其他软件或自寻办法。
* 简易路由的匹配顺序是 `block` -> `remote` -> `direct`
* 自定义路由的顺序是 `自定义（全局）` -> `自定义` -> `简易路由`

### DNS

#### 域名策略

如果你需要“准确的域名分流”（遇到不包含在 geosite 的小众域名，解析成 IP 进行再次匹配），请开启 IPIfNonMatch 域名策略。

现在这种分流模式只在 Xray 核心有，开启后可能增加延迟。

#### 远程 DNS

* 只在 Tun Mode 或开启了 "入站域名策略" 时会被使用，其他方式(如系统代理)请求的域名将按原样发送到服务器。
* 开启 FakeDNS 功能以避免 Tun Mode 下的解析，直接发送域名到服务器。

#### 直连 DNS

直连 DNS 用以解析 `服务器地址` 和 `绕过` 域名。

检查当前直连 DNS 是否可用：

1. 设置 loglevel 为 debug，关闭 Tun 等造成干扰日志的功能。
2. 启动一个「服务器地址是域名」的配置。
3. 点击左下角进行延迟测试。
4. 如果测试有延迟，则表示当前节点 和 直连 DNS 均可用。
5. 如果测试失败，但日志中输出了该域名对应的 IP，则表示直连 DNS 可用，节点不可用。
6. 如果日志中没有该域名对应的 IP，则表示直连 DNS 不可用。

如果您遇到 DNS 导致的无法上网等问题，请尝试：

1. 将直连 DNS 改为可用的 DNS 如 `localhost` （本机DNS）/ `https://223.5.5.5/dns-query` / `223.5.5.5`
2. 调整 核心选项 中有关 DNS 的设置

### 自定义路由规则

路由规则的语法请看 [Xray路由语法](https://xtls.github.io/config/routing.html#ruleobject) 或 [sing-box路由语法](https://sing-box.sagernet.org/configuration/route/rule/)

自定义路由示例 (Xray block QUIC)

```json
{
    "rules": [
        {
            "network": "udp",
            "outboundTag": "block",
            "port": "443",
            "type": "field"
        }
    ]
}
```

## 出站 / 服务器

一般情况下， GUI 显示的一个服务器配置对应后端的一个出站 (Outbound Object)

### 自定义出站

(2.1+) 通过编写出站 JSON，可以在 sing-box 中使用 wireguard ssh shadowtls 和在 Xray 中使用 mKCP 等配置。

[Xray出站语法](https://xtls.github.io/config/outbound.html) 

[sing-box出站语法](https://sing-box.sagernet.org/configuration/outbound/)

!!! warning "注意"

    应填写一个 Outbound Object，而非整个配置文件。直接复制已有的 config.json 是错误的。

示例： 在 NekoBox 中使用 ShadowTLS 服务器。

1. 新建一个自定义出站（配置1）

```json
{
  "type": "shadowtls",
  "tag": "shadowtls-out",
  "server": "127.0.0.1", // 你的服务器地址
  "server_port": 4443, // 你的服务器端口
  "tls": {
    "enabled": true,
    "server_name": "microsoft.com" // 伪装域名
  }
}
```

2. 新建一个 Shadowsocks 出站（配置2）

3. 按 配置1 配置2 顺序组成链式代理

### 自定义出站 JSON 配置

JSON 对象会被合并至出站 Object，请看下方例子。

示例：NekoBox新建一个socks服务器，填写服务器地址端口，填写自定义 JSON 配置（配置1），实际运行的出站是wireguard（配置2）

```json
// 配置1
{
    "type": "wireguard",
    "system_interface": false,
    "interface_name": "wg0",
    "local_address": [
        "10.0.0.2/32"
    ],
    "private_key": "YNXtAzepDqRv9H52osJVDQnznT5AM11eCK3ESpwSt04=",
    "peer_public_key": "Z1XXLsKYkYxuiYjJIkRvtIKFepCYHTgON+GwPq7SOV4=",
    "pre_shared_key": "31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=",
    "mtu": 1408
}
// 配置2
{
    "domain_strategy": "",
    "interface_name": "wg0",
    "local_address": [
        "10.0.0.2/32"
    ],
    "mtu": 1408,
    "peer_public_key": "Z1XXLsKYkYxuiYjJIkRvtIKFepCYHTgON+GwPq7SOV4=",
    "pre_shared_key": "31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=",
    "private_key": "YNXtAzepDqRv9H52osJVDQnznT5AM11eCK3ESpwSt04=",
    "server": "服务器地址",
    "server_port": 12345,
    "system_interface": false,
    "tag": "proxy",
    "type": "wireguard"
}
```

### 自定义配置 JSON 配置

同上，但是 JSON 被合并至配置 Object

示例：sing-box NTP

```json
{
    "ntp": {
        // ......
    }
}
```

合并结果：

```json
{
    "ntp": {
        // 你填写的
    },
    "inbounds": [
        // GUI 生成的
    ],
    "outbounds": [
        // GUI 生成的
    ],
    // GUI 生成的 ...
}
```

### 链式代理

右键 -> 手动输入配置 -> 类型选择为 "链式代理"。

点击 "选择配置" 后，开始选择自己需要的服务器。按 `enter` 或 双击 使用这个服务器，按 `Esc` 退出。

单击 "交换" 按钮更换服务器，单击 "垃圾桶" 按钮删除服务器。

使用链式代理后，流量将按照自上到下的顺序转发。第一个服务器作为流量入口，最后一个服务器作为流量出口。

!!! note
    使用 ShadowTLS 时，如果 ShadowTLS 使用原版客户端，则无法使用链式代理连接。请自行在本地开启一个 ShadowTLS 客户端，并将 ShadowTLS 承载的协议目标地址改为 `127.0.0.1:{ShadowTLS 本地端口}`。

## Hook.js

(2.8+) 允许使用 javascript 在外部生成或修改某些配置，面向高级用户，详细请看[技术文档](https://github.com/MatsuriDayo/nekoray/blob/main/docs/HookJS.md)。

## 运行参数

https://github.com/MatsuriDayo/nekoray/blob/main/docs/RunFlags.md
