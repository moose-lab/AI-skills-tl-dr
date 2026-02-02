# OpenClaw 入门 — TL;DR

> **适合谁**：没有编程经验，想在自己的聊天工具里用上 AI 助手
> **你需要**：一台电脑（Mac 或 Linux）+ 一个 AI 服务账号
> **预计时间**：15 分钟完成安装，之后随时在聊天里用

---

## 1. 这是什么？

OpenClaw 是装在你电脑上的**私人 AI 助手**。

装好之后，你可以直接在 **WhatsApp、Telegram、Slack、Discord** 等日常聊天工具里跟 AI 对话——就像跟朋友发消息一样。

**装好后你可以这样用：**
- 在 WhatsApp 里发"帮我查一下明天上海的天气"
- 在 Telegram 里发"帮我把这段话改成正式邮件"
- 让它每天早上自动给你发一条新闻摘要
- 让它帮你打开网页、搜索信息、定时提醒

---

## 2. 和直接用 ChatGPT 网页有什么不同？

| 用 ChatGPT/Claude 网页 | 用 OpenClaw |
|------------------------|-------------|
| 每次都要打开一个网页 | AI 就在你的 WhatsApp/Telegram 里，随时发消息 |
| 只能打字聊天 | 还能帮你控制浏览器、定时提醒、发消息 |
| 对话数据存在别人的服务器 | 数据在你自己的电脑上 |
| 每个 AI 平台单独用 | 一个助手，所有聊天工具都能找到它 |

---

## 3. 什么时候该用？什么时候不该用？

**适合你的情况：**
- 你希望在 WhatsApp/Telegram 等聊天工具里直接和 AI 对话
- 你希望 AI 不只是聊天，还能帮你做事（提醒、搜索、浏览网页）
- 你在意隐私，希望数据留在自己的电脑上

**不适合你的情况：**
- 你只需要偶尔问问题，网页版 ChatGPT/Claude 已经够用
- 你不想用电脑上的终端（命令行窗口）——安装过程需要在终端里粘贴几行指令
- 你不愿意付费订阅 AI 服务——OpenClaw 本身免费，但 AI "大脑"需要付费（Anthropic 或 OpenAI）

---

## 4. 它是怎么工作的？

```
你在 WhatsApp/Telegram/Slack 里发消息
         │
         ▼
   ┌────────────┐
   │ 你电脑上的  │  ← 在后台安静运行，你感觉不到它
   │ OpenClaw   │
   └─────┬──────┘
         │
         ▼
   调用 AI 服务（Claude/ChatGPT）思考回答
         │
         ▼
   回复发回到你的聊天工具里
```

就是这么简单——它在你电脑后台运行，收到消息就调用 AI 思考，再把回复发回来。

---

## 5. 从零开始安装（手把手）

### 准备工作

你需要先注册一个 AI 服务账号（二选一）：

| 选择 | 推荐度 | 怎么注册 |
|------|-------|---------|
| **Anthropic Claude** | 强烈推荐 | 访问 [anthropic.com](https://www.anthropic.com)，注册并订阅 Pro 或 Max |
| **OpenAI ChatGPT** | 也可以 | 访问 [openai.com](https://openai.com)，注册并订阅 Plus 或更高 |

---

### Mac 用户安装

#### 第 1 步：打开终端

按 **Cmd + 空格**，输入"**终端**"，按回车。会弹出一个黑色/白色的文字窗口。

> 接下来所有操作都在这个窗口里完成。每一步都是：复制指令 → 粘贴到窗口 → 按回车。

#### 第 2 步：安装运行底座

复制下面这行，粘贴到终端，按回车：

```
curl -fsSL https://fnm.vercel.app/install | bash
```

等它跑完后，**关闭终端窗口，再重新打开**（同样是 Cmd+空格 → 终端 → 回车）。

然后复制粘贴这两行（一行一行来）：

```
fnm install 22
```

```
fnm use 22
```

**检查是否成功：** 粘贴这行并回车：

```
node --version
```

> 成功的标志：屏幕显示 `v22` 开头的数字，比如 `v22.12.0`。
> 如果显示"command not found"，说明第 2 步没成功，请重新操作一次。

#### 第 3 步：安装 OpenClaw

复制粘贴这行：

```
npm install -g openclaw@latest
```

等 1-2 分钟安装完成。

**检查是否成功：**

```
openclaw --version
```

> 成功的标志：屏幕显示一个版本号，比如 `2025.1.15`。

#### 第 4 步：启动安装向导

复制粘贴这行：

```
openclaw onboard --install-daemon
```

接下来屏幕上会出现一个引导向导，一步步带你完成配置。跟着提示做就行：

1. **登录 AI 服务**：向导会打开浏览器，让你登录 Anthropic 或 OpenAI，授权完成后回到终端
2. **启动后台服务**：向导自动完成，OpenClaw 会设为开机自动运行
3. **连接聊天工具**：见下面"连接聊天工具"部分

---

### Linux 用户安装

操作和 Mac 完全一样——打开终端，按上面 4 个步骤依次操作即可。

---

### Windows 用户

OpenClaw 目前不支持在 Windows 上直接安装。推荐两个方案：

- **方案 A**：使用 Mac 或 Linux 电脑
- **方案 B**：在 Windows 上安装 Linux 子系统（WSL2），然后按 Linux 步骤操作。这需要一定的技术操作，建议请懂电脑的朋友帮忙。官方说明：[微软 WSL 安装指南](https://learn.microsoft.com/zh-cn/windows/wsl/install)

---

## 6. 连接你的聊天工具

安装向导会问你想连接哪些聊天工具。以下是两个最常用的详细步骤：

### 连接 WhatsApp（最简单，扫码即可）

1. 安装向导选择 WhatsApp 后，终端里会显示一个**二维码**
2. 打开手机上的 WhatsApp
3. 点 **设置** → **关联设备** → **关联新设备**
4. 用手机扫描终端里的二维码
5. 等几秒钟，终端显示连接成功

> 成功的标志：你用另一个手机（或让朋友）给你的 WhatsApp 号发一条消息，几秒钟后会收到 AI 的回复。
>
> 注意：WhatsApp 连接使用的是非官方方式，有小概率被限制。建议使用备用手机号。

### 连接 Telegram

1. 打开 Telegram，搜索 **@BotFather**
2. 给 BotFather 发送 `/newbot`
3. 按提示给你的 AI 助手取个名字（比如"我的AI助手"）
4. BotFather 会回复你一串字符（Bot 令牌），复制它
5. 回到终端的安装向导，粘贴这串字符

> 成功的标志：在 Telegram 里找到你刚创建的 Bot，给它发一条消息，几秒钟后收到 AI 回复。

### 其他聊天工具

向导也支持 Slack、Discord、Signal、iMessage 等。选择后按屏幕提示操作即可。如果想以后再添加，随时运行：

```
openclaw onboard
```

---

## 7. 开始对话

一切就绪！打开你连接的聊天工具，给 AI 发一条消息：

> 你好，你能帮我做什么？

AI 会回复你。以后想找它，直接在聊天里发消息就行。

### 聊天里的快捷指令

在对话中直接发送：

| 发送 | 效果 |
|------|------|
| `/new` | 开始新对话（让 AI 忘掉之前的内容） |
| `/status` | 查看当前状态 |
| `/think high` | 让 AI 更认真地思考（回复稍慢，但更准确） |
| `/think off` | 恢复普通模式（回复更快） |

### 使用场景举例

- "帮我查一下今天人民币兑美元的汇率"
- "把下面这段话翻译成英文：……"
- "每天早上 8 点提醒我吃药"
- "帮我打开 xxx.com，总结一下页面内容"
- "帮我写一封给客户的道歉邮件"

---

## 8. 常见问题

**终端里粘贴命令后报错"command not found"？**
→ 回到第 2 步，确认那一步执行成功了。如果还是不行，关闭终端重新打开再试。

**WhatsApp 二维码扫不了？**
→ 确认手机和电脑连着同一个 Wi-Fi。WhatsApp 更新到最新版本。

**AI 不回复消息了？**
→ 在终端运行 `openclaw doctor`（复制粘贴即可），它会自动检查哪里出了问题。
→ 确认你的 AI 账号（Anthropic/OpenAI）还有余额。

**想更新到最新版？**
→ 在终端运行 `npm install -g openclaw@latest`，然后运行 `openclaw doctor`。

**陌生人会不会触发我的 AI？**
→ 不会。默认情况下，陌生人发消息需要先输入配对码才能使用你的 AI 助手。

**支持哪些聊天工具？**
→ WhatsApp、Telegram、Slack、Discord、Google Chat、Signal、iMessage、网页聊天，以及通过插件支持 Microsoft Teams、Matrix 等更多平台。

---

## 9. 信息来源

- 官方入门教程：[docs.openclaw.ai/start/getting-started](https://docs.openclaw.ai/start/getting-started)
- 安装向导详解：[docs.openclaw.ai/start/wizard](https://docs.openclaw.ai/start/wizard)
- 常见问题合集：[docs.openclaw.ai/start/faq](https://docs.openclaw.ai/start/faq)
- 社区求助：[discord.gg/clawd](https://discord.gg/clawd)
- 官方 GitHub：[github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)

---

## 10. 质量反馈（预留）

- 清晰度评分：⭐️⭐️⭐️⭐️⭐️
- 可执行性评分：⭐️⭐️⭐️⭐️⭐️
- 实用反馈：👍 / 👎
