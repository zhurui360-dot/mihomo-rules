# mihomo-rules

Mihomo / Nikki（OpenWrt）用的自定义直连规则集，在线托管，**换域名不用再改 YAML**。

## 用法（主源 + 备源，双保险）

`raw.githubusercontent.com` 在部分网络下直连不通，所以主源走 `gh-proxy.com` 加速，备源走 jsDelivr：

```yaml
rule-providers:
  my_direct:         {type: http, interval: 1800, proxy: DIRECT, behavior: domain, format: text,
                      url: "https://gh-proxy.com/https://raw.githubusercontent.com/zhurui360-dot/mihomo-rules/main/my_direct.txt"}
  my_direct_bak:     {type: http, interval: 3600, proxy: DIRECT, behavior: domain, format: text,
                      url: "https://testingcf.jsdelivr.net/gh/zhurui360-dot/mihomo-rules@main/my_direct.txt"}
```

```yaml
rules:
  - RULE-SET,my_direct,DIRECT
  - RULE-SET,my_direct_bak,DIRECT
```

两条规则互不干扰：哪条源能拉到就生效，都拉不到才退回正常分流。

## 修改规则

只改 `my_direct.txt`：

```
# 一行一条，# 注释
+.xdm.us.ci        # +. 开头 = 它和它的所有子域
www.abc.com        # 裸域名 = 只匹配它自己
```

改完提交即可。客户端最多个 `interval`（1800 秒）后自动拉取；
想立刻生效：面板 → 规则集 / Providers → `my_direct` → 点更新。

## 注意

- 仓库必须 **public**，否则地址需要鉴权，内核拉不到。
- `gh-proxy.com` 是公共加速，无缓存、改完立刻可拉到；万一哪天它挂了，
  备源 jsDelivr 还在（jsDelivr 对分支文件有约 12 小时缓存，改动不会即时生效）。
- 备选加速地址：`https://ghfast.top/<原始URL>`、`https://ghproxy.net/<原始URL>`。
