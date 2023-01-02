# 自定义核心相关

## 使用说明

自定义核心功能可以管理许多提供 socks5 代理的软件，这里以 [Hysteria](https://github.com/HyNetwork/hysteria) 为例。

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

```json
{
  "server": "127.0.0.1:%mapping_port%",
  "obfs": "fuck me till the daylight",
  "up_mbps": 10,
  "down_mbps": 50,
  "socks5": {
    "listen": "127.0.0.1:%socks_port%"
  }
}
```

替换串 `%socks_port%` 代表 Hysteria 将监听的端口，这个端口每次启动配置时都会自动生成，如果不使用替换串而直接填写固定端口，则 nekoray 无法获取 Hysteria 所监听的端口，导致无法使用分流等功能。

替换串 `127.0.0.1:%mapping_port%` 代表 `地址` `端口` 处填写的服务器端口被映射到本机的端口，这个端口每次启动配置时都会自动生成，如果不使用替换串而直接填写固定地址，则无法使用链式代理等功能，且在VPN模式下，自定义核心不一定知道如何绕过VPN访问网络，可能造成回环无法使用。

其他配置和命令行版本是一致的，nekoray 只是接管了流量的出(mapping_port)和入(socks5_port)，代理协议的实现还是由自定义的核心进行。

!!! note "提示"

    可以点击 "预览" 按钮预览配置结果。

!!! warning "提示"

    不要在自定义核心内配置分流，这不是推荐的用法。如果确实需要，说明你用错软件了。

## 常见核心

常见核心的下载地址和使用方法，欢迎补充。

### Hysteria

https://github.com/HyNetwork/hysteria/releases

使用方法：

1. hysteria:// 分享链接导入
2. 手写 JSON 配置，参考上面说明

### NaïveProxy

https://github.com/klzgrad/naiveproxy/releases

使用方法：

1. 分享链接导入
2. 图形化填写配置

### TUIC

https://github.com/EAimTY/tuic/releases

命令：` -c %config%`

使用方法：手写 JSON 配置

### Trojan-Go

https://github.com/p4gefau1t/trojan-go/releases

https://github.com/gfw-report/trojan-go/releases

命令：` -config %config%`

使用方法：手写 JSON 配置
