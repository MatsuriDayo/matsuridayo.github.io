# 配置

## 窗口界面使用

如图所示

<img src="/assets/images/nekoray1.jpg" />

Ctrl+F 进入搜索， Esc 退出。

Ctrl+V 从剪辑版导入。(0.14+)

## 协议说明

同 [Matsuri 协议说明](/m-configuration/)

## 订阅说明

- 「Clash格式」一般带有流量信息，本项目支持解析其节点（推荐使用）
- 「V2rayN格式」一般不带流量信息，本项目支持解析
- 「某些苹果应用格式」带有流量信息，本项目不支持
- 「SSR格式」本项目不支持

## VPN 设置

VPN 模式可以代理整个系统的流量，适合某些软件“不听话”不走代理的情况。

Nekoray 目前没有 VPN (Tun/Tap) 接口管理、路由管理、自动设置等功能。

在 Windows 平台，可以尝试配合 [TunMax](https://github.com/TunMax/tun2socks_gui_for_windows) 使用。也可以直接用 [Netch](https://github.com/netchx/netch)

在 Linux 平台，可以尝试 examples 目录下面的脚本：基于 tun2socks，目前有 netns 和 全局 两种模式，分别适用不同的情况。

VPN 模式下建议开启「流量探测」，设置为「探测结果用于路由判断」，有助于域名规则的的匹配。

VPN 模式下也可以开启「FakeDNS」

## 基本设置

### 入站 & 系统代理

- 系统代理需要启用 HTTP 入站方可使用（点击窗口下方 socks 标签打开设置）
- 入站地址填写 0.0.0.0 相当于某些软件的 “允许局域网连接”

### URL 测试

支持 http 或 https 协议

与 Matsuri 一致，如果开头为 `true` 则表示测试真延迟 RTT，否则测试握手延迟。

真延迟 `truehttps://cp.cloudflare.com/`

握手延迟 `https://cp.cloudflare.com/`

### 自定义入站

[语法规则](https://www.v2fly.org/config/inbounds.html#inbounds)

示例 (dokodemo-door)

```
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

## v2ray 资源文件路径

允许设置 v2ray 资源文件夹。便于使用额外的规则文件，以及自动更新和管理 geo 资源文件（如使用v2rayN、软件包管理器或升级脚本）。

## 路由设置

### 简易路由

* 内置了 `全局` `绕过局域网和大陆` 两种规则
* 以 # 开头的行不生效
* 不支持 `多节点分流` `负载均衡` 这类用法，若有需求请换其他软件或自寻办法。

### 自定义路由

[语法规则](https://www.v2fly.org/config/routing.html#ruleobject)

示例 (block QUIC)

```
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

## 自定义配置

可以管理许多提供 socks5 代理的软件，这里以 [Hysteria](https://github.com/HyNetwork/hysteria) 为例

### 核心

核心名称为 `hysteria`

下载对应的核心程序（这里是 `hysteria.exe`）后，请在 `基本设置-核心-其他核心` 处填写核心的路径。

### 命令

`-c %config%`

替换串 `%config%` 代表下方配置文件，每次启动时会自动写出并填写路径。

目前只支持简单的字符串替换。

运行起来相当于这个命令： `/path/to/hysteria -c /path/to/temporary/config`

程序工作目录： config 目录

### 配置文件

```
{
  "server": "127.0.0.1:%mapping_port%",
  "obfs": "fuck me till the daylight",
  "up_mbps": 10,
  "down_mbps": 50,
  "server_name": "real.name.com",
  "socks5": {
    "listen": "127.0.0.1:%socks_port%"
  }
}
```

替换串 `%socks_port%` 代表 Hysteria 将监听的端口，这个端口每次启动配置时都会自动生成，如果不使用替换串而直接填写固定端口，则 nekoray 无法获取 Hysteria 所监听的端口，导致无法使用分流等功能。

替换串 `127.0.0.1:%mapping_port%` 代表 `地址` `端口` 处填写的服务器端口被映射到本机的端口，这个端口每次启动配置时都会自动生成，如果不使用替换串而直接填写固定地址，则无法使用链式代理等功能。

其他配置和命令行版本是一致的，nekoray 只是接管了流量的出(mapping_port)和入(socks5_port)，代理协议的实现还是由自定义的核心进行。
