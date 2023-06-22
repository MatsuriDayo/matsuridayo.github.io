# NB4A 路由

根据指定的规则将流量发送到不同的出口。

### 用法

不同规则 **从上到下** 匹配，规则内的条件为「并」的关系。

详情请看： https://sing-box.sagernet.org/configuration/route/rule/

### 语法

#### 端口 (版本 0.6+)

port / sourcePort 如果有多个请以英文逗号 `,` 分割，端口段用英文冒号 `:` 表示。

示例： `1234,:1024,2000:2999,60000:`

#### IP (版本 0.5+)

ip / source 一行一个。 格式如 `1.2.3.4/8` 或 `geoip:private`

#### 域名 (版本 0.5+)

domain 一行一个，语法如下

- **纯字符串**：当此域名完整匹配目标域名时，该规则生效。例如 `v2ray.com` 匹配 `v2ray.com` 但不匹配 `www.v2ray.com`。
- **正则表达式**：由 `regexp:` 开始，余下部分是一个正则表达式。当此正则表达式匹配目标域名时，该规则生效。例如 `regexp:\.goo.*\.com$` 匹配 `www.google.com`、`fonts.googleapis.com`，但不匹配 `google.com`。
- **子域名 (推荐)**：由 `domain:` 开始，余下部分是一个域名。当此域名是目标域名或其子域名时，该规则生效。例如 `domain:v2ray.com` 匹配 `www.v2ray.com`、`v2ray.com`，但不匹配 `xv2ray.com`。
- **子串**：由 `keyword:` 开始，余下部分是一个字符串。当此字符串匹配目标域名中任意部分，该规则生效。比如 `keyword:sina.com` 可以匹配 `sina.com`、`sina.com.cn`、`www.sina.com` 和 `www.sina.company`，但不匹配 `sina.cn`。
- **预定义域名列表**：由 `geosite:` 开头，余下部分是一个名称，如 `geosite:google` 或者 `geosite:cn`。名称及域名列表参考 [预定义域名列表](https://www.v2fly.org/config/routing.html#%E9%A2%84%E5%AE%9A%E4%B9%89%E5%9F%9F%E5%90%8D%E5%88%97%E8%A1%A8)。

### DNS 行为

直连DNS：一般用于解析国内域名

远程DNS：一般用于解析国外域名

#### 关闭「DNS路由」时

所有 DNS 将发送到「远程DNS」进行解析。

#### 开启「DNS路由」时

以下 DNS 请求会被发到「直连DNS」进行解析。

- 路由规则中 outbound 为 「绕过」 的域名 （如默认规则中的 geosite:cn） 
- 路由规则中 outbound 为 「绕过」 的应用 

以下 DNS 请求会被发到「远程DNS」进行解析。

- 未匹配到上面规则的域名
- 路由规则中 outbound 为 「代理」 的域名
- 路由规则中 outbound 为 「代理」 的应用

#### 例外

* 当代理节点的地址为域名时，该域名的 DNS 请求会被发到「直连DNS」进行解析。
* 不支持的 DNS 查询，会被当作 UDP 流量经过代理发送至 8.8.8.8:53
* 进行 Ping / URL Test 时，始终使用系统 DNS 解析节点的域名。
* 通过 Android netd 查询的 DNS，无法匹配到 APP 规则。

#### 提示

* 建议在 `设置 - 分应用代理` 绕过「中国产 app」，而不是使用路由规则。
* 建议开启 `Block QUIC 规则` 加速网站访问
* 使用「国际版 app」 如 tiktok 时，如果打不开，可以尝试为其添加「代理」的规则。
* 由于 Google 服务器奇怪的行为，如果节点被 Google 标识为「中国」，可能导致无法下载 Google Play 资源。
* 如果为 VPN 设置 HTTP 代理，建议打开 `在 Core 中绕过 LAN` 以在浏览器中访问局域网站点
