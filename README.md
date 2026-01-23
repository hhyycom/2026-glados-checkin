# 🚀 2026 GLaDOS 自动签到

<div align="center">

**🎯 2026年最简单、门槛最低的 GLaDOS 自动签到方案**

**零代码 · 零服务器 · 完全免费 · 微信推送**

[![Auto Checkin](https://github.com/lankerr/2026-glados-checkin/actions/workflows/checkin.yml/badge.svg)](https://github.com/lankerr/2026-glados-checkin/actions)
[![GitHub Stars](https://img.shields.io/github/stars/lankerr/2026-glados-checkin?style=social)](https://github.com/lankerr/2026-glados-checkin)

**⭐ 如果对你有帮助，请给一个 Star 支持一下！**

</div>

---

> **📢 重要提示**  
> - GLaDOS 官网已迁移至 **glados.cloud**（不再是 glados.rocks）
> - 本项目专为 2026 积分制度优化，每天自动签到两次
> - 完全免费，使用 GitHub Actions，无需自己的服务器
> - 不会的话可以提 Issue，作者很乐意帮助技术新手！

---

## ✨ 功能特点

| 功能 | 说明 |
|------|------|
| 🎯 **精准积分** | 获取真实积分数据 + 每日变化量 |
| 🎁 **兑换提示** | 显示当前可兑换选项及差额 |
| ⏰ **每日两次** | 早上 9:30 + 晚上 21:30 自动签到 |
| 🔄 **失败重试** | 首次失败自动重试一次 |
| 📱 **微信推送** | PushPlus 漂亮 HTML 报告 |
| ☁️ **Cloud优先** | 自动切换可用域名 |

---

## 📚 给小白的科普

> 💡 如果你已经熟悉这些概念，可以跳过这部分直接看 [快速部署](#-快速部署5分钟搞定)

<details>
<summary><b>🍴 什么是 Fork？</b></summary>

**Fork** = 把别人的项目复制一份到你自己的账号下。

就像复印一份文档，原件还在别人那里，你有一份自己的副本可以随便改。Fork 之后这个项目就属于你了，你可以自己配置和使用。

</details>

<details>
<summary><b>🍪 什么是 Cookie？</b></summary>

**Cookie** = 网站记住你是谁的"通行证"。

当你登录 GLaDOS 后，网站会给你的浏览器一个"通行证"（Cookie），下次访问时浏览器出示这个通行证，网站就知道你是谁了。

我们需要把这个通行证告诉签到程序，这样程序就能"假装"是你去签到。

</details>

<details>
<summary><b>🔐 什么是 Secrets？</b></summary>

**Secrets** = 保险箱，用来存放敏感信息。

你的 Cookie 和 Token 都是隐私信息，不能直接写在代码里（否则别人都能看到）。GitHub Secrets 就像一个保险箱，把这些敏感信息锁起来，只有你的程序运行时才能访问。

</details>

<details>
<summary><b>⚙️ 什么是 GitHub Actions？</b></summary>

**GitHub Actions** = 免费的自动化机器人。

它可以按照你设定的时间表（比如每天 9:30）自动运行程序。你不需要自己的服务器，GitHub 会免费帮你跑代码。

</details>

<details>
<summary><b>▶️ 什么是 Run workflow？</b></summary>

**Run workflow** = 手动测试按钮。

| 按钮 | 作用 |
|------|------|
| **Run workflow** | 立即执行一次（不管现在几点），用于测试配置是否正确 |
| **定时任务** | 每天 9:30 和 21:30 自动执行，不需要手动操作 |

简单说：点 Run workflow 是**测试**，以后会**自动运行**。

</details>

---

## 🚀 快速部署（5分钟搞定）

### 第一步：Fork 本仓库

点击页面右上角的 **Fork** 按钮，将项目复制到你的账号下。

---

### 第二步：获取 Cookie 🍪

> ⚠️ **注意**：GLaDOS 官网已迁移到 **https://glados.cloud**，请使用新域名！

#### 2.1 安装 Cookie 扩展

在 **Edge 浏览器** 的扩展商店搜索 `cookie`，安装 **Cookie-Editor** 或类似的 Cookie 管理扩展：

![Cookie 扩展搜索](images/cookie-extension.png)

> 💡 **提示**：只要能显示 `koa:sess` 和 `koa:sess.sig` 这两个 Cookie 的扩展都可以使用！

#### 2.2 登录 GLaDOS 并获取 Cookie

1. 打开 [https://glados.cloud](https://glados.cloud) 并登录
2. 进入 **签到页面**（Console → Checkin）
3. 点击浏览器右上角的 **Cookie-Editor** 扩展图标
4. 找到并复制这两个值：
   - `koa:sess` → 一串很长的字符串
   - `koa:sess.sig` → 一串较短的字符串

![获取 Cookie](images/glados-cookies.png)

#### 2.3 组合 Cookie（重要！）

将两个值按以下格式组合，**注意格式必须完全正确**：

```
koa:sess=你的长字符串; koa:sess.sig=你的短字符串
```

**正确示例**：
```
koa:sess=eyJ1c2VySWQiOjEyMzQ1Njc4OTB9; koa:sess.sig=abcdef123456
```

**常见错误**：
- ❌ 缺少分号 `;`
- ❌ 缺少空格（分号后需要一个空格）
- ❌ 值两边多了引号
- ❌ 复制了多余的空格或换行

#### 2.4 验证你的 Cookie 格式

运行以下 Python 代码验证格式是否正确：

```python
# 将你的 Cookie 粘贴到下面的引号中
cookie = "koa:sess=你的长字符串; koa:sess.sig=你的短字符串"

# 验证
if "koa:sess=" in cookie and "koa:sess.sig=" in cookie and "; " in cookie:
    parts = cookie.split("; ")
    if len(parts) == 2 and parts[0].startswith("koa:sess=") and parts[1].startswith("koa:sess.sig="):
        print("✅ Cookie 格式正确！")
    else:
        print("❌ 格式错误，请检查分号和空格")
else:
    print("❌ Cookie 缺少必要的字段")
```

---

### 第三步：配置 GitHub Secrets 🔐

1. 进入你 Fork 的仓库
2. 点击 **Settings**（设置）
3. 左侧菜单找到 **Secrets and variables** → **Actions**
4. 点击右上角 **New repository secret**

![添加 Secret](images/add-secret.png)

添加以下两个 Secret：

| Name | Value | 必需 |
|------|-------|------|
| `GLADOS_COOKIE` | 第二步组合的 Cookie | ✅ 是 |
| `PUSHPLUS_TOKEN` | 微信推送 Token（见下方） | ❌ 否 |

---

### 第四步：获取 PushPlus Token（可选）📱

如果你希望签到后收到**微信通知**，请配置 PushPlus：

1. 访问 [https://www.pushplus.plus](https://www.pushplus.plus)
2. 点击右上角 **登录**，使用微信扫码登录

![PushPlus 扫码登录](images/pushplus-checkin.png)

3. 登录后点击 **发送消息** → **一对一消息**
4. 复制页面上显示的 **Token**（类似 `05c3****dd36` 的字符串）

![获取 Token](images/pushplus-token.png)

5. 将 Token 添加到 GitHub Secrets，Name 填 `PUSHPLUS_TOKEN`

---

### 第五步：启用 Actions ⚡

1. 进入你 Fork 仓库的 **Actions** 标签页
2. 如果看到黄色提示，点击 **I understand my workflows, go ahead and enable them**
3. 点击左侧的 **GLaDOS 2026 Checkin**
4. 点击右侧 **Run workflow** 按钮手动测试一次

![启用 Actions](images/workflow.png)

**🎉 完成！** 以后每天 9:30 和 21:30 会自动签到。

---

## 📊 推送效果预览

签到成功后，你会在微信收到类似这样的推送：

```
👤 your@email.com

当前积分: 46 (+20)
剩余天数: 353 天
签到结果: Bindweed! Bindweed!

🎁 兑换选项:
❌ 100分→10天 (差54分)
❌ 200分→30天 (差154分)
❌ 500分→100天 (差454分)
```

---

## ⏰ 自动运行时间

| 时间（北京时间） | 说明 |
|------------------|------|
| **09:30** | 早间签到 |
| **21:30** | 晚间签到 |

> 💡 GitHub Actions 可能有 5-30 分钟的延迟，这是正常现象。

---

## ❓ 常见问题

<details>
<summary><b>Q: 显示 "please checkin via https://glados.cloud" 怎么办？</b></summary>

这表示今天已经签到过了，明天会正常显示。

</details>

<details>
<summary><b>Q: Cookie 多久过期？</b></summary>

大约 30 天。过期后重新按第二步获取新 Cookie，更新 Secret 即可。

</details>

<details>
<summary><b>Q: 支持多个账号吗？</b></summary>

支持！用英文符号 `&` 分隔多个 Cookie：
```
cookie1&cookie2&cookie3
```

</details>

<details>
<summary><b>Q: 没有收到微信推送怎么办？</b></summary>

1. 检查 `PUSHPLUS_TOKEN` 是否配置正确
2. 在 PushPlus 网站测试发送功能是否正常
3. 查看 Actions 运行日志是否有错误

</details>

<details>
<summary><b>Q: Actions 运行失败怎么办？</b></summary>

1. 点击失败的运行记录查看详细日志
2. 检查 Cookie 格式是否正确
3. 如果还是不行，欢迎提 Issue！

</details>

---

## 📂 项目文件

| 文件 | 说明 |
|------|------|
| `checkin.py` | 核心签到脚本 |
| `.github/workflows/checkin.yml` | GitHub Actions 配置 |
| `requirements.txt` | Python 依赖 |
| `images/` | 教程截图 |

---

## 🤝 需要帮助？

- 📝 **提 Issue**：遇到问题请提 Issue，作者很乐意帮助技术新手！
- ⭐ **Star**：如果对你有帮助，请点个 Star 支持一下
- 🍴 **Fork**：欢迎 Fork 并贡献代码

---

## 📝 License

MIT

---

<div align="center">

**Made with ❤️ for GLaDOS users in 2026**

**⭐ Star 一下，支持作者持续更新！⭐**

</div>
