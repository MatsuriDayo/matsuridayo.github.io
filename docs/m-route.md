# 路由

根据指定的规则将流量发送到不同的出口。

### 用法

不同规则 **从上到下** 匹配，规则内的条件为「并」的关系。

详情请看： https://www.v2fly.org/config/routing.html#ruleobject

#### 反向代理

将远程流量转发到本地（需要 v2ray 服务器支持）

https://www.v2fly.org/config/reverse.html#reverseobject

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

#### 提示

使用「国际版 app」 如 tiktok 时，如果打不开，可以尝试为其添加「代理」的规则。

* 如果知道对应 geosite ，可以改为添加域名规则，如 `geosite:tiktok`。

使用「Google Play Store」时，建议「顺其自然」。由于 Google 服务器奇怪的行为，如果节点被送中，下载 Google Play 资源可能必须使用中国网络。
