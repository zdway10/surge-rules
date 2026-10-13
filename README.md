# surge-rules

我自己的 Surge 分流规则集镜像。

## 这是什么

这个仓库只做一件事：**每天自动从 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) 抓取我实际用到的规则集，存到这里。**

上游那个仓库有 **4.3 GB**（聚合了上千个来源），而我实际只用到 **85 个文件、约 6 MB**。所以我只镜像这 85 个，仓库小、同步快、完全属于我。

## 目录结构

```
list.txt                        # 需要同步的 85 个 blackmatrix7 规则集路径
extra.txt                       # 其它来源的规则集（路径 + 完整 URL）
rules/                          # 规则集本体（由 Actions 自动更新）
.github/workflows/sync.yml      # 每天自动同步的流水线
```

## 数据来源

| 来源 | 数量 | 说明 |
|---|---|---|
| [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) | 85 | 见 `list.txt`，每 2 天更新 |
| [zxfccmm4/Profiles](https://github.com/zxfccmm4/Profiles) | 1 | `Unbreak.list`，见 `extra.txt`（该仓库 2024-12 后未再更新） |

## 自动更新

- **频率**：每天一次（北京时间凌晨 3 点左右）
- **原理**：`.github/workflows/sync.yml` 里的 GitHub Actions 定时从上游下载最新版并提交
- **手动触发**：仓库页面 → Actions → Sync Surge rule sets → Run workflow

## 在 Surge 里怎么用

把配置里原本的：

```
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Surge/<路径>
```

换成：

```
https://raw.githubusercontent.com/<你的用户名>/surge-rules/main/rules/<路径>
```

例如：

```
https://raw.githubusercontent.com/<你的用户名>/surge-rules/main/rules/Claude/Claude.list
```

## 注意事项

- 仓库**必须是公开（public）**的。`raw.githubusercontent.com` 不服务私有仓库，Surge 拉不到。
- 如果上游改了文件名或删了某个规则集，Actions 会下载失败并打 `warning`，但**不会**删掉本地已有的文件（保持最后一次成功的版本）。
- blackmatrix7 失败超过 10 个、或 `extra.txt` 里任一来源失败，Actions 会**中止本次提交**，避免把残缺镜像推上来。
- 想增删规则集：改 `list.txt`（blackmatrix7）或 `extra.txt`（其它来源），push 后 Actions 会自动处理。
