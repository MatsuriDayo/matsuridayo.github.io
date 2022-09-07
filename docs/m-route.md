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
* 若启用 FakeDNS 则 远程 DNS 被 FakeDNS 代替，此时不能使用“白名单”等分流方式。

#### 提示

* 建议在 `设置 - 分应用代理` 绕过「中国产 app」，而不是使用路由规则。
* 使用「国际版 app」 如 tiktok 时，如果打不开，可以尝试为其添加「代理」的规则。
* 由于 Google 服务器奇怪的行为，如果节点被 Google 标识为「中国」，可能导致无法下载 Google Play 资源。
