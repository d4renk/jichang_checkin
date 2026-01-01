# SSPANEL 机场自动签到

<div align="center">

基于 GitHub Actions 的 SSPANEL 机场自动签到工具，支持多账号批量签到，签到失败时通过 Server 酱推送提醒。

[![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-blue?logo=github-actions)](https://github.com/features/actions)
[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

</div>

---

## 项目简介

本项目是一个轻量级的自动签到脚本，专为基于 **SSPANEL** 框架搭建的机场网站设计。通过 GitHub Actions 实现每日自动签到，无需服务器即可稳定运行。

### 主要功能

- ✅ 每日自动签到，获取流量奖励
- ✅ 支持多账号批量管理
- ✅ 签到失败时通过 Server 酱推送提醒
- ✅ 基于 GitHub Actions，完全免费
- ✅ 随机延迟执行，避免请求集中

### 适用范围

适用于所有基于 **SSPANEL** 框架的机场网站。

**如何确认机场是否使用 SSPANEL？**
- 在机场网站首页滑到最底部
- 查看是否有 "Powered by SSPANEL" 字样

示例：
![SSPANEL 标识](https://user-images.githubusercontent.com/21276183/214764546-4f66333a-cb9b-420e-8260-697d26fb4547.png)

---

## 快速开始

### 1. Fork 仓库

点击本仓库右上角的 **Fork** 按钮，将项目 Fork 到你的 GitHub 账户。

### 2. 启用 GitHub Actions

**重要提示：** Fork 后，GitHub Actions 默认是禁用状态，必须手动启用。

1. 进入你 Fork 后的仓库
2. 点击顶部导航栏的 **Actions** 标签
3. 如果看到提示，点击 **"I understand my workflows, go ahead and enable them"** 按钮

### 3. 配置仓库机密

进入仓库的 **Settings** → **Secrets and variables** → **Actions**，点击 **New repository secret** 添加以下机密信息：

#### 必需配置

| 变量名 | 说明 | 示例 |
|--------|------|------|
| `URL` | 机场网站地址 | `https://example.com` |
| `CONFIG` | 账号密码配置 | 见下方说明 |

#### 可选配置

| 变量名 | 说明 | 获取方式 |
|--------|------|----------|
| `SCKEY` | Server 酱推送密钥（失败提醒） | [访问 Server 酱官网](https://sct.ftqq.com/) 注册获取 |

#### 配置格式说明

**URL 格式：**
```
https://example.com
```
- **注意事项：**
  - 必须包含完整的协议头（`https://` 或 `http://`）
  - 末尾**不要**添加斜杠 `/`
  - 示例正确：`https://example.com` ✅
  - 示例错误：`https://example.com/` ❌

**CONFIG 格式：**

支持多账号配置，格式为：**一行账号，一行密码**

单账号示例：
```
user@example.com
your_password
```

多账号示例：
```
user1@example.com
password1
user2@example.com
password2
user3@example.com
password3
```

**SCKEY 格式：**
```
SCT123456ABCDEFGxxxxxxxxxxxxxxx
```
- 如不需要推送通知，可留空此配置项

### 4. 手动触发首次运行

配置完成后，建议手动触发一次运行以验证配置是否正确：

1. 进入 **Actions** 标签页
2. 点击左侧的 **"Common airport Checkin"** 工作流
3. 点击右侧的 **"Run workflow"** → **"Run workflow"** 按钮

### 5. 查看运行结果

- **GitHub Actions 日志：** 在 **Actions** → **"Common airport Checkin"** → 选择运行记录 → 点击 **"Run sign"** 查看详细日志
- **微信推送：** 如配置了 `SCKEY`，签到失败时会自动推送提醒到你的微信

---

## 运行时间

项目默认运行时间为：

- **定时任务：** 每天 UTC 14:00（北京时间 22:00）
- **随机延迟：** 10-300 秒（避免请求集中）
- **手动触发：** 随时可在 Actions 页面手动运行

如需修改运行时间，可编辑 `.github/workflows/main.yml` 文件中的 cron 表达式：

```yaml
schedule:
  - cron: "0 14 * * *"  # UTC 14:00 = 北京时间 22:00
```

---

## 本地运行

如需在本地测试，可按以下步骤操作：

### 1. 克隆仓库
```bash
git clone https://github.com/你的用户名/jichang_checkin.git
cd jichang_checkin
```

### 2. 安装依赖
```bash
pip install -r requirements.txt
```

### 3. 设置环境变量
```bash
export URL="https://your-airport.com"
export CONFIG="user@example.com
your_password"
export SCKEY="your_server_chan_key"  # 可选
```

### 4. 运行脚本
```bash
python3 main.py
```

---

## 常见问题

### 1. Fork 后为什么没有自动运行？

**答：** Fork 后 GitHub Actions 默认禁用，需要手动启用。请查看上方「启用 GitHub Actions」章节。

### 2. 如何确认签到是否成功？

**答：** 有两种方式查看：
- 在 GitHub Actions 运行日志中查看详细信息
- 配置 Server 酱后，签到失败时会推送提醒到微信（成功则不推送）

### 3. 提示"配置文件格式错误"怎么办？

**答：** 检查 `CONFIG` 配置：
- 确保账号和密码成对出现（一行账号，一行密码）
- 不要有多余的空行
- 总行数必须是偶数

### 4. Server 酱推送不成功？

**答：** 请检查：
- `SCKEY` 是否正确配置
- Server 酱账号是否已绑定微信
- 访问 [Server 酱官网](https://sct.ftqq.com/) 确认服务状态

### 5. 如何修改签到时间？

**答：** 编辑 `.github/workflows/main.yml` 文件中的 cron 表达式。

在线工具：[Crontab Guru](https://crontab.guru/)

### 6. 支持哪些机场？

**答：** 仅支持基于 **SSPANEL** 框架的机场。可在机场网站底部查看是否有 "Powered by SSPANEL" 标识。

---

## 推送方式

本项目使用 [**Server 酱**](https://sct.ftqq.com/) 进行微信推送。

**推送策略：** 仅在签到失败时推送提醒，签到成功则不推送（避免打扰）。

### 如何获取 SCKEY？

1. 访问 [Server 酱官网](https://sct.ftqq.com/)
2. 使用微信扫码登录
3. 绑定微信号
4. 在「SendKey」页面复制你的 SendKey（即 `SCKEY`）
5. 将 SendKey 添加到仓库的 Secrets 中

**不需要推送？** 留空 `SCKEY` 配置即可。

---

## 注意事项

⚠️ **安全提示：**
- 所有敏感信息（URL、账号密码、密钥）务必通过 GitHub Secrets 配置
- 切勿在代码中硬编码任何凭证信息
- 切勿将包含敏感信息的配置文件提交到仓库

⚠️ **使用须知：**
- 本项目仅供学习交流使用
- 请遵守机场服务条款
- 频繁签到可能触发风控，建议使用默认时间

---

## 技术栈

- **语言：** Python 3.10+
- **依赖：** requests >= 2.23.0
- **CI/CD：** GitHub Actions
- **推送：** Server 酱

---

## 贡献指南

欢迎提交 Issue 和 Pull Request！

在提交 PR 前，请确保：
- 代码符合项目规范
- 测试通过
- 描述清晰明了

---

## 许可证

本项目采用 MIT 许可证，详见 [LICENSE](LICENSE) 文件。

---

<div align="center">

**Made with ❤️ by the community**

</div>
