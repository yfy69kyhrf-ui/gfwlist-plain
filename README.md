# gfwlist-plain

明文版 GFWList 规则，由 GitHub Actions 每日自动从上游同步生成，供只认明文规则的浏览器扩展直接订阅。

## 原理

上游 [gfwlist/gfwlist](https://github.com/gfwlist/gfwlist) 发布的 `gfwlist.txt` 是 base64 编码的 AutoProxy 格式，部分工具只接受明文。本仓库的 Actions 工作流每天自动完成：

1. 拉取上游最新 `gfwlist.txt`
2. base64 解码为明文
3. 与 `custom-rules.txt` 中的自定义规则按行合并去重（自定义规则优先保留）
4. 内容有变化才提交，无变化不产生新 commit

## 订阅地址

国内网络直接可用（jsDelivr 镜像，缓存约 12 小时）：

```
https://cdn.jsdelivr.net/gh/yfy69kyhrf-ui/gfwlist-plain@main/gfwlist.txt
```

备用域名（主站不通时替换主机名即可）：

```
https://fastly.jsdelivr.net/gh/yfy69kyhrf-ui/gfwlist-plain@main/gfwlist.txt
https://testingcf.jsdelivr.net/gh/yfy69kyhrf-ui/gfwlist-plain@main/gfwlist.txt
```

原始地址（国内直连不可达，需自备网络环境）：

```
https://raw.githubusercontent.com/yfy69kyhrf-ui/gfwlist-plain/main/gfwlist.txt
```

## 添加自定义规则

直接在网页上编辑 `custom-rules.txt` 并提交即可，规则为 AutoProxy / ABP 语法，下个更新周期自动并入 `gfwlist.txt`：

```
||example-blocked.example^      屏蔽
@@||example-ok.example^         例外（放行）
```

## 文件说明

| 文件 | 说明 |
|---|---|
| `gfwlist.txt` | 生成物，**请勿手动编辑**，每次更新会被覆盖 |
| `custom-rules.txt` | 自定义规则，手工维护 |
| `.github/workflows/update.yml` | 自动化工作流 |

## 更新频率

每天一次（cron `0 3 * * *`，UTC 3 点 = 北京时间 11 点）。需要调整频率就修改 workflow 中的 cron 表达式，最小间隔 5 分钟（`*/5 * * * *`，注意 GitHub 定时任务在高负载时会有几分钟延迟）。

## 许可

上游规则遵循 [LGPL-2.1](https://github.com/gfwlist/gfwlist/blob/master/COPYING.txt)，本仓库与其保持一致。
