# 配置

## 窗口界面使用

如图所示

<img src="/assets/images/nekoray1.jpg" />

Ctrl+F 进入搜索， Esc 退出。

## 协议说明

同 [Matsuri 协议说明](/m-configuration/)

## 订阅说明

- 暂不支持识别“小火箭”等格式
- 推荐使用 clash 格式导入

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


## 路由设置

### 简易路由

内置了 `全局` `绕过局域网和大陆` 两种规则

以 # 开头的行不生效

不支持 `多节点分流` `负载均衡` 这类用法，若有需求请换其他软件或自寻办法。

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

可以管理许多提供 socks5 代理的软件，这里以 Hysteria 为例

### 核心

核心名称为 `hysteria`

下载对应的核心程序后，请在 `基本设置-核心-其他核心/自定义` 处填写核心的路径。

### 命令

`-c %config%`

替换串 `%config%` 代表下方配置文件，每次启动时会自动写出并填写路径。

目前只支持简单的字符串替换。

运行起来相当于这个命令： `/path/to/hysteria -c /path/to/temporary/config`

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
