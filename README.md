# Mythnote 服务状态（Upptime）

公开监测：独立站与后台公开接口是否在线。掉线会自动开 GitHub Issue，恢复后自动关闭。

**状态页（生成后）：** https://lashtech2026-commits.github.io/mythnote-uptime/

**本仓不进 App 主仓**（与 `lashtech-daemon` 无关）。

## 监测哪些地址

| 名称 | 说明 | 算正常 |
|---|---|---|
| Mythnote Beauty site / www | 独立站 | 200 / 301 / 302 |
| Supabase REST gateway | 后台网关存活（故意不带密钥） | **401** |
| Edge regional-hint | 区域提示（只读） | 200 / 429 |
| Edge payment-return | 支付回跳说明（只读 GET，不写库） | 200 / 429 |

全部只读 GET，不碰支付写接口、不写库。

## 掉线怎么知道

1. **GitHub Issue（已开）** — 掉线自动开 issue，恢复关 issue。订阅本仓 Watch → Custom → Issues 即可收到 GitHub 自带邮件。
2. **发到 mythnotelashes@gmail.com 的专用邮件** — 需要老板再提供 SMTP 发信凭证（见下）。**现在还发不出去专用邮件。**

## 邮件还差什么（老板要提供）

当前仓库的 token **写不进 Actions Secrets**，且还没有 SMTP。要让掉线邮件真正发到 `mythnotelashes@gmail.com`，需要：

1. **有人能在本仓 Settings → Secrets and variables → Actions 里添加 Secrets**（或给运维 token 开「Secrets」写权限）。
2. 添加这些 Secret（名字必须一致，**值不要发到会议室**）：

| Secret 名 | 填什么 |
|---|---|
| `NOTIFICATION_EMAIL` | `true` |
| `NOTIFICATION_EMAIL_FROM` | 发件邮箱（Gmail 建议与 SMTP 用户名相同） |
| `NOTIFICATION_EMAIL_TO` | `mythnotelashes@gmail.com` |
| `NOTIFICATION_EMAIL_SMTP` | `true` |
| `NOTIFICATION_EMAIL_SMTP_HOST` | `smtp.gmail.com` |
| `NOTIFICATION_EMAIL_SMTP_PORT` | `587` |
| `NOTIFICATION_EMAIL_SMTP_USERNAME` | Gmail 账号 |
| `NOTIFICATION_EMAIL_SMTP_PASSWORD` | **Gmail「应用专用密码」**（不是登录密码；需先开两步验证） |

另外建议加：

| Secret 名 | 填什么 |
|---|---|
| `GH_PAT` | 对本仓有 Contents / Issues / Actions / Workflows 写权限的 Personal Access Token（让 Upptime 能提交状态、开 issue） |

没有 `GH_PAT` 时，部分工作流会尝试用默认 `GITHUB_TOKEN`；若 Actions 报权限不足，再补 `GH_PAT`。

## 怎么开 GitHub Pages

Settings → Pages → Source 选 **Deploy from a branch** → 分支 `gh-pages` / `(root)`。第一次 Static Site CI 跑完后会出现 `gh-pages`。

## 运维注意

- 不要把 Supabase service_role / 数据库连接串放进本仓。
- 不要对 `create-payment`、`stripe-webhook` 做监测。
- 同事问题私下沟通；持续掉线再大白话汇报老板。
