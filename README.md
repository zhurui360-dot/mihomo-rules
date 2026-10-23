# mihomo-rules

Mihomo / Nikki（OpenWrt）用的自定义直连规则集，在线托管，**换域名不用再改 YAML**。

## 用法（mrs 格式）

仓库里的 `my_direct.mrs` 由 `my_direct.txt` 自动编译生成，客户端直接用 mrs：

```yaml
rule-providers:
  my_direct:
    type: http
    interval: 1800
    proxy: DIRECT
    behavior: domain
    format: mrs
    url: "https://cdn.jsdelivr.net/gh/zhurui360-dot/mihomo-rules@main/my_direct.mrs"
```

```yaml
rules:
  - RULE-SET,my_direct,DIRECT
```

> 地址用 `cdn.jsdelivr.net` 而不是 `testingcf.jsdelivr.net`：后者对分支文件有约 12 小时边缘缓存，改完拉不到最新内容。

## 修改规则

**只改 `my_direct.txt`**：

```
# 一行一条，# 注释
+.xdm.us.ci        # +. 开头 = 它和它的所有子域
www.abc.com        # 裸域名 = 只匹配它自己
```

提交后 `build-mrs` workflow 会自动执行
`mihomo convert-ruleset domain text my_direct.txt my_direct.mrs`，
把编译结果提交回仓库并刷新 jsDelivr 缓存。**不用手动编译，也不用碰 mrs 文件。**

## 文件说明

| 文件 | 说明 |
|---|---|
| `my_direct.txt` | **源文件，只改这个** |
| `my_direct.mrs` | 自动编译产物，勿手改 |
| `.github/workflows/build-mrs.yml` | 推送 txt 时自动编译 mrs |

## 注意

- 仓库必须 **public**，否则地址需要鉴权，内核拉不到。
- 自动编译有几十秒延迟；改完后客户端最多个 `interval`（1800 秒）拉取。
  想立刻生效：面板 → 规则集 → `my_direct` → 点更新。
