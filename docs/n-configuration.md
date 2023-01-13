# 配置

## 窗口界面使用

如图所示

<img src="/assets/images/nekoray1.jpg" />

Ctrl+F 进入搜索， Esc 退出。

其他快捷键右键菜单已标明。

## NekoBox

(1.11+) NekoBox 是试制的 sing-box GUI 客户端。

在 `基本设置 - 内核` 切换到 sing-box 以启用。切换前后变化大，不建议来回切换。

如果已有的某个服务器在切换后无法使用，建议删除后手动添加。

使用注意：

1. 简易路由仍然按照 v2ray v4 格式填写，如 `geoip:cn` `domain:google.com`，这类规则 nekobox 会自动翻译为 sing-box 的规则。
2. 自定义 JSON 配置（路由、出站等）请按照 sing-box 的格式填写。
3. Hysteria 类型服务器将自动转换为 sing-box 内置的 hysteria 出站。
如果遇到转换错误，请改用自定义配置（sing-box）类型。
如果需要使用原版 hysteria 内核，请在 `基本设置 - 内核` 中以其他名称新建内核类型。

## Hook.js

(2.8+) 允许使用 javascript 在外部生成或修改某些配置，面向高级用户，详细请看[技术文档](https://github.com/MatsuriDayo/nekoray/blob/main/docs/HookJS.md)。

## 协议说明

同 [Matsuri 协议说明](/m-configuration/)

## 导入 / 导出 / 订阅 

支持的格式:

- 「Clash格式」一般带有流量信息，本项目支持解析其节点（推荐使用）
- 「V2rayN格式」一般不带流量信息，本项目支持解析
- 「某些苹果应用格式」带有流量信息，本项目不支持
- 「SSR格式」本项目不支持

Neko 格式为程序内部的存储格式，包含的信息最完全，但跨版本兼容性没有保证。链接为 `nekoray://xxxxx`

## VPN / TUN 设置

(1.1+) VPN / TUN 模式可以代理整个系统的流量，适合某些软件“不听话”不走代理的情况。

Nekoray 目前支持在 Windows / Linux / macOS 自动配置 VPN

* sing-box(nekobox_core) 提供 tun2socks 和接口配置支持。
* VPN 模式下建议开启「流量探测」，设置为「探测结果用于路由判断」，有助于匹配域名规则。
* VPN 模式下可以开启「FakeDNS」加速 DNS 查询
* 在 Windows 系统，由于上游问题，vpn 模式有概率启动失败。如连续多次不能启动，请重启并清理多余的网络接口后再试。
* 绕过的 IP CIDR 和 进程名，格式为一行一个。
* sing tun 配置模板在 `config/vpn/sing-box-vpn.json` ，可以参考[默认模板](https://github.com/MatsuriDayo/nekoray/blob/main/res/vpn/sing-box-vpn.json)自行修改。

## 基本设置

### URL 测试

支持 http 或 https 协议

与 Matsuri 一致，如果开头为 `true` 则表示测试真延迟 RTT，否则测试握手延迟。

真延迟 `truehttps://cp.cloudflare.com/`

握手延迟 `https://cp.cloudflare.com/`

### 自定义入站

[v2ray入站语法](https://www.v2fly.org/config/inbounds.html#inbounds)

[sing-box入站语法](https://sing-box.sagernet.org/configuration/inbound/)

示例 (v2ray dokodemo-door)

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

### v2ray 资源文件路径

允许设置 v2ray 资源文件夹。便于使用额外的规则文件，以及自动更新和管理 geo 资源文件（如使用v2rayN、软件包管理器或升级脚本）。

* v2ray 使用的是 .dat 后缀的 geo 资源文件
* sing-box 使用的是 .db 后缀的 geo 资源文件
* Loyalsoldier 版 v2ray dat 下载地址 https://github.com/Loyalsoldier/v2ray-rules-dat

## 路由设置

### 简易路由

* 内置了 `全局` `绕过局域网和大陆` 两种规则
* 以 # 开头的行不生效
* 不支持 `多节点分流` `负载均衡` 这类用法，若有需求请换其他软件或自寻办法。

### 自定义路由规则

路由规则的语法请看 [v2ray路由语法](https://www.v2fly.org/config/routing.html#ruleobject) 或 [sing-box路由语法](https://sing-box.sagernet.org/configuration/route/rule/)

自定义路由示例 (v2ray block QUIC)

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

(2.1+) 通过编写出站 JSON，可以在 sing-box 中使用 wireguard ssh shadowtls 和在 v2ray 中使用 mKCP 等配置。

[v2ray出站语法](https://www.v2fly.org/config/outbounds.html) 目前 v2ray JSON 配置使用的是 v4 版本。

[sing-box出站语法](https://sing-box.sagernet.org/configuration/outbound/)

!!! warning "注意"

    应填写一个 Outbound Object，而非整个配置文件。直接复制已有的 config.json 是错误的。

示例： 在 NekoBox 中使用 ShadowTLS 服务器。

1. 新建一个自定义出站（配置1）

```json
{
  "type": "shadowtls",
  "tag": "shadowtls-out",
  "server": "127.0.0.1",
  "server_port": 4443,
  "tls": {
    "enabled": true,
    "server_name": "google.com"
  }
}
```

2. 新建一个 Shadowsocks 出站（配置2）

3. 按 配置1 配置2 顺序组成链式代理

### 自定义 JSON 配置

适用于普通出站。

!!! note "提示"

    JSON 对象会被合并。

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
