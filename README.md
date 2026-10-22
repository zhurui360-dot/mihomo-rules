# mihomo-rules

Mihomo / Nikki（OpenWrt）用的自定义直连规则集，在线托管，**换域名不用再改 YAML**。

## 用法

在配置的 `rule-providers` 里加：

```yaml
rule-providers:
  my_direct:
    type: http
    interval: 1800
    proxy: DIRECT
    behavior: domain
    format: text
    url: "https://raw.githubusercontent.com/zhurui360-dot/mihomo-rules/main/my_direct.txt"
```

在 `rules` 里引用（建议放在靠前位置）：

```yaml
rules:
  - RULE-SET,my_direct,DIRECT
```

## 修改规则

只改 `my_direct.txt`：

```
# 一行一条，# 注释
+.xdm.us.ci        # +. 开头 = 它和它的所有子域
www.abc.com        # 裸域名 = 只匹配它自己
```

改完提交即可。客户端最多个 `interval`（这里 1800 秒）后自动拉取；
想立刻生效：面板 → 规则集 / Providers → `my_direct` → 点更新。

## 注意

- 仓库必须 **public**，否则 raw 地址需要鉴权，内核拉不到。
- 用的是 `raw.githubusercontent.com`（无缓存，改完立刻可拉到）；
  如果所在网络拉不动 raw，可换成 jsDelivr：
  `https://testingcf.jsdelivr.net/gh/zhurui360-dot/mihomo-rules@main/my_direct.txt`
  （jsDelivr 对分支文件有约 12 小时缓存，改动不会即时生效）
