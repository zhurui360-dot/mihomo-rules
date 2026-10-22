# mihomo-rules

Mihomo / Nikki（OpenWrt）用的自定义直连规则集，在线托管，**换域名不用再改 YAML**。

## 用法

`raw.githubusercontent.com` 直连不通，所以走 jsDelivr：

```yaml
rule-providers:
  my_direct:
    type: http
    interval: 1800
    proxy: DIRECT
    behavior: domain
    format: text
    url: "https://testingcf.jsdelivr.net/gh/zhurui360-dot/mihomo-rules@main/my_direct.txt"
```

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

改完提交即可。客户端最多个 `interval`（1800 秒）后自动拉取；
想立刻生效：面板 → 规则集 / Providers → `my_direct` → 点更新。

## 注意

- 仓库必须 **public**，否则地址需要鉴权，内核拉不到。
- jsDelivr 对分支文件有约 12 小时缓存，所以改完不一定立刻拉到；
  急用可以在面板点一次更新，或临时换成加速地址（无缓存）：
  `https://gh-proxy.com/https://raw.githubusercontent.com/zhurui360-dot/mihomo-rules/main/my_direct.txt`
- jsDelivr 备用节点：`https://gcore.jsdelivr.net/gh/...`（把 testingcf 换掉即可）。
