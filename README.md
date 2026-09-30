# VPNCheap 国内服务直连规则

在 VPNCheap 的自定义规则中，将下面的 SRS 地址设为**直连**，并在规则更新后重新连接 VPN：

`https://raw.githubusercontent.com/Roger-G/vpn-rules/main/direct-cn.srs`

本规则集以社区维护的 `geolocation-cn` 为基础，保留本仓库原有的 66 个域名后缀及 B 站、百度的资源域名补充。它只处理命中的域名；其余连接继续依照 VPNCheap 内原有规则处理。它不是按 App 身份分流，也不会覆盖没有可用域名信息的连接。

`direct-cn.json` 是可审阅的源规则，`direct-cn.srs` 是供 VPNCheap 导入的二进制规则集。两者均为 sing-box rule-set v2 格式。不要同时启用其他重复的自定义域名直连规则，以免难以判断具体命中来源。

## 微信补充与编译

2026-09-30：参照 [blackmatrix7/WeChat](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Clash/WeChat/WeChat.list) 与 [ACL4SSR/Wechat](https://github.com/ACL4SSR/ACL4SSR/blob/master/Clash/Ruleset/Wechat.list)，补充原规则未覆盖的 10 个域名后缀和 2 个精确域名，包括小程序、头像和支付相关域名。没有加入清单中的宽泛腾讯 ASN；直接使用 IP 且无域名信息的连接，需要另行通过连接日志排查。

使用官方 sing-box 工具生成二进制文件：

```sh
sing-box rule-set compile --output direct-cn.srs direct-cn.json
sing-box rule-set decompile --output decompiled.json direct-cn.srs
sing-box rule-set match --format binary direct-cn.srs weixin.qq.com
```

发布前检查源规则与反编译结果一致，并使用二进制文件核验微信清单中的域名和海外服务反例。规则文件核验不代表已经在 iPhone 上验证网络连接或 App 风控结果。

## 来源与许可

- [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat) 的 `sing` 分支 `geo/geosite/geolocation-cn.json` 与 `.srs`，作为基础集合；其 GPL-3.0 许可文本见 [LICENSE-meta-rules-dat.txt](LICENSE-meta-rules-dat.txt)。这里在基础集合后追加本仓库已有的规则，并保留 JSON 源文件供查看和修改。
- 此基础集合也包含来自 [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community) 的社区域名数据；其 MIT 许可文本见 [LICENSE-v2fly.txt](LICENSE-v2fly.txt)。

该集合按上游当时的内容固定；未来更新需要重新核对上游变更与自定义补充，不能只凭域名的国别后缀或服务器 IP 推断是否适合直连。

