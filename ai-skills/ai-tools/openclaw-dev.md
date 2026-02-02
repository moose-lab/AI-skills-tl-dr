# OpenClaw 技术指南

> **面向**：开发者、有命令行经验的技术用户
> **目标**：10 分钟理解架构 + 完成安装配置 + 知道如何扩展
> **前置**：Node.js ≥22、pnpm、至少一个 AI 模型订阅（推荐 Anthropic Claude Pro/Max）

---

## 1. 一句话定义

**本地运行的 WebSocket 控制面（Gateway）+ Pi Agent 运行时，通过 14+ 消息渠道（WhatsApp/Telegram/Slack/Discord 等）暴露 AI 助手能力，支持工具调用、技能扩展和多设备协同。**

---

## 2. 架构

```
WhatsApp / Telegram / Slack / Discord / Signal / iMessage / Google Chat / ...
               │
               ▼
      ┌─────────────────────┐
      │      Gateway         │  ← WebSocket 控制面 (ws://127.0.0.1:18789)
      │   sessions / routing │
      │   channels / cron    │
      └──────────┬──────────┘
                 │
    ┌────────────┼────────────┐
    │            │            │
    ▼            ▼            ▼
 Pi Agent      CLI        WebChat UI
 (RPC mode)  (openclaw …)   (内置)
    │
    ├─ AI Provider (Anthropic Claude / OpenAI)
    ├─ Tool Streaming + Block Streaming
    ├─ Skills (Bundled / Managed / Workspace)
    └─ Nodes (macOS/iOS/Android 设备能力)
```

### 核心组件

| 组件 | 职责 | 代码位置 |
|------|------|---------|
| **Gateway** | WebSocket 控制面：session 管理、渠道路由、事件分发、cron、webhook | `src/infra/` |
| **Pi Agent** | RPC 模式 Agent 运行时，tool streaming + block streaming | `src/` (agent 相关) |
| **Channels** | 消息渠道适配器（核心 8 个 + 扩展插件） | `src/telegram/`, `src/discord/`, `src/slack/`, `src/signal/`, `src/imessage/`, `src/web/`, `src/channels/` |
| **Extensions** | 渠道插件（MS Teams, Matrix, Zalo 等），workspace packages | `extensions/` |
| **Skills** | Agent 工具系统：bundled / managed (ClawHub) / workspace | `skills/` |
| **Nodes** | 设备能力暴露（camera, screen record, system.run, Canvas） | `apps/macos/`, `apps/ios/`, `apps/android/` |
| **CLI** | 命令行界面 | `src/cli/`, `src/commands/` |
| **Web UI** | Gateway 控制面板 + WebChat | `ui/` |

---

## 3. 安装

### 方式 A：npm 全局安装（推荐快速使用）

```bash
npm install -g openclaw@latest
# 或 pnpm add -g openclaw@latest

openclaw onboard --install-daemon
```

### 方式 B：从源码（开发/贡献）

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw

pnpm install
pnpm ui:build    # 构建 Web UI（首次自动安装 UI 依赖）
pnpm build       # TypeScript → dist/

# 引导配置
pnpm openclaw onboard --install-daemon

# 开发模式（TS 变更自动重载）
pnpm gateway:watch

# 发送测试消息
pnpm openclaw agent --message "test" --thinking high
```

### 关键命令速查

```bash
openclaw onboard             # 引导式配置向导
openclaw gateway run         # 手动启动 Gateway
openclaw channels status --probe  # 查看所有渠道状态
openclaw doctor              # 诊断和修复配置问题
openclaw config set <key> <value>  # 设置配置项
openclaw message send --to <target> --message "text"  # 发消息
openclaw agent --message "..." --thinking high  # 调用 Agent
openclaw update --channel stable|beta|dev  # 切换发布频道
```

---

## 4. 消息渠道配置

### 核心渠道

| 渠道 | 库 | 配置方式 |
|------|---|---------|
| WhatsApp | Baileys (非官方) | 扫码配对，`openclaw onboard` 引导 |
| Telegram | grammY | `openclaw config set channels.telegram.token <BOT_TOKEN>` |
| Slack | Bolt | `openclaw config set channels.slack.token <BOT_TOKEN>` + signing secret |
| Discord | discord.js | `openclaw config set channels.discord.token <BOT_TOKEN>` |
| Google Chat | Chat API | 服务账号配置 |
| Signal | signal-cli | 关联 signal-cli 实例 |
| iMessage | imsg | 仅 macOS，直接连接 |
| WebChat | 内置 | Gateway 启动后自动可用 |

### 扩展渠道（插件）

| 渠道 | 插件位置 |
|------|---------|
| Microsoft Teams | `extensions/msteams/` |
| Matrix | `extensions/matrix/` |
| Zalo | `extensions/zalo/` |
| Zalo Personal | `extensions/zalouser/` |
| BlueBubbles | (扩展) |
| Voice Call | `extensions/voice-call/` |

插件机制：
- 插件是 workspace packages，安装依赖用 `npm install --omit=dev`
- 运行时依赖放 `dependencies`，`openclaw` 放 `devDependencies` 或 `peerDependencies`
- 避免 `workspace:*` 在 `dependencies` 中（npm install 会失败）

### DM 安全策略

```bash
# 默认策略：pairing（陌生人需配对码）
openclaw config set channels.telegram.dm.policy pairing

# 批准配对
openclaw pairing approve telegram <code>

# 危险：开放模式（不推荐暴露到公网）
openclaw config set channels.telegram.dm.policy open
```

---

## 5. 技能系统 (Skills)

三种来源：

| 类型 | 说明 | 管理方式 |
|------|------|---------|
| **Bundled** | 内置技能（浏览器控制、Canvas、camera 等） | 随 OpenClaw 安装 |
| **Managed** | 社区技能，通过 ClawHub 注册表 | Agent 自动搜索和安装 |
| **Workspace** | 本地自定义技能 | 放在项目 skills/ 目录 |

### ClawHub

[clawhub.com](https://clawhub.com) 是 OpenClaw 的技能注册表。启用后，Agent 可以自动搜索、下载、安装社区发布的技能。

### 内置工具能力

| 工具 | 说明 | 文档 |
|------|------|------|
| Browser Control | 独立 Chrome/Chromium 实例，CDP 控制 | [docs.openclaw.ai/tools/browser](https://docs.openclaw.ai/tools/browser) |
| Canvas + A2UI | Agent 驱动的可视化画布 | [docs.openclaw.ai/platforms/mac/canvas](https://docs.openclaw.ai/platforms/mac/canvas) |
| Cron Jobs | 定时任务 | [docs.openclaw.ai/automation/cron-jobs](https://docs.openclaw.ai/automation/cron-jobs) |
| Webhooks | 外部事件触发 | [docs.openclaw.ai/automation/webhook](https://docs.openclaw.ai/automation/webhook) |
| Gmail Pub/Sub | Gmail 事件订阅 | [docs.openclaw.ai/automation/gmail-pubsub](https://docs.openclaw.ai/automation/gmail-pubsub) |
| Node 设备能力 | camera snap/clip, screen record, location, notifications | [docs.openclaw.ai/nodes](https://docs.openclaw.ai/nodes) |
| Session Tools | 跨 session 协调（sessions_list / sessions_send / sessions_history） | [docs.openclaw.ai/concepts/session-tool](https://docs.openclaw.ai/concepts/session-tool) |

---

## 6. 远程访问与集成

### Tailscale 暴露 Gateway

```bash
# 仅 Tailnet 内部访问
openclaw config set gateway.tailscale.mode serve

# 公网访问（必须设置密码）
openclaw config set gateway.tailscale.mode funnel
openclaw config set gateway.auth.mode password
```

注意：`gateway.bind` 必须保持 `loopback`。

### SSH Tunnel

```bash
# 远程 Gateway 连接
openclaw config set gateway.remote.url wss://gateway-host:18789
openclaw config set gateway.remote.token <token>
```

### ACP (Agent Client Protocol) — IDE 集成

OpenClaw 支持 ACP，可作为 Zed 等 IDE 的 AI Agent 后端：

```json
// ~/.config/zed/settings.json
{
  "agent_servers": {
    "OpenClaw ACP": {
      "type": "custom",
      "command": "openclaw",
      "args": ["acp", "--session", "agent:main:main"]
    }
  }
}
```

ACP 桥接 stdio ↔ Gateway WebSocket，支持 session 映射、cancel、listSessions。

---

## 7. 项目结构

```
openclaw/
├── src/                    # 核心源码 (TypeScript ESM)
│   ├── cli/                # CLI 入口
│   ├── commands/           # 命令实现
│   ├── telegram/           # Telegram 渠道
│   ├── discord/            # Discord 渠道
│   ├── slack/              # Slack 渠道
│   ├── signal/             # Signal 渠道
│   ├── imessage/           # iMessage 渠道
│   ├── web/                # WhatsApp (Baileys) + WebChat
│   ├── channels/           # 渠道通用逻辑
│   ├── routing/            # 消息路由
│   ├── infra/              # 基础设施（Gateway 核心）
│   ├── media/              # 媒体处理管线（图片/音频/视频/转录）
│   ├── acp/                # Agent Client Protocol 桥接
│   ├── canvas-host/        # Canvas + A2UI 宿主
│   └── provider-web.ts     # Web Provider
├── extensions/             # 渠道插件 (workspace packages)
│   ├── msteams/            # Microsoft Teams
│   ├── matrix/             # Matrix
│   ├── zalo/               # Zalo
│   ├── zalouser/           # Zalo Personal
│   └── voice-call/         # 语音通话
├── skills/                 # 技能目录
├── apps/                   # 配套原生应用
│   ├── macos/              # macOS 菜单栏 App (Swift)
│   ├── ios/                # iOS App (Swift)
│   └── android/            # Android App (Kotlin)
├── ui/                     # Gateway Web UI
├── docs/                   # 文档 (Mintlify → docs.openclaw.ai)
├── scripts/                # 构建/发布/工具脚本
├── test/                   # 测试
├── vendor/                 # 第三方 vendored 代码
├── patches/                # pnpm 补丁
├── dist/                   # 编译输出
├── package.json            # 根配置
├── pnpm-workspace.yaml     # monorepo workspace 定义
├── tsconfig.json           # TypeScript 配置
└── vitest.config.ts        # 测试配置（多个 vitest config）
```

---

## 8. 开发指南

### 技术栈

- **语言**：TypeScript (ESM)，严格类型，避免 `any`
- **运行时**：Node ≥22（Bun 可用于执行 TS）
- **包管理**：pnpm workspaces（也兼容 bun install）
- **Lint/Format**：Oxlint + Oxfmt → `pnpm check`
- **测试**：Vitest，V8 coverage 阈值 70%
- **代码风格**：文件 ~500-700 LOC 为宜，复杂逻辑加注释

### 常用开发命令

```bash
pnpm install              # 安装依赖
pnpm build                # 类型检查 + 编译
pnpm check                # Lint + Format
pnpm test                 # 运行测试 (vitest)
pnpm test:coverage        # 带覆盖率
pnpm test:live            # 真实 API Key 测试 (LIVE=1)
pnpm gateway:watch        # 开发模式自动重载
pnpm openclaw ...         # 通过 tsx 直接运行 TS
pnpm ui:build             # 构建 Web UI
```

### 提交规范

```bash
# 推荐用项目自带的提交工具
scripts/committer "<msg>" <file...>

# 消息风格：简洁、动作导向
# 例如：CLI: add verbose flag to send
```

### 发布频道

| 频道 | 标签格式 | npm dist-tag |
|------|---------|-------------|
| stable | `vYYYY.M.D` | `latest` |
| beta | `vYYYY.M.D-beta.N` | `beta` |
| dev | main HEAD | `dev` |

---

## 9. 常见坑点

- **Node < 22 不支持** — 不要尝试，核心功能依赖 Node 22+ 的特性
- **WhatsApp 使用 Baileys（非官方库）** — 有封号风险，用备用号
- **DM 策略不要随意设为 open** — 默认 pairing 策略是有意为之的安全措施
- **插件不要用 `workspace:*` 在 dependencies 中** — npm install 会报错，用 `devDependencies` 或 `peerDependencies`
- **macOS App 不能通过 SSH 构建** — 必须在 Mac 本地直接运行
- **不要编辑 `node_modules`** — 更新会覆盖一切
- **有 `pnpm.patchedDependencies` 的包必须用精确版本** — 不允许 `^`/`~`
- **工具 schema 不要用 `Type.Union`** — Google Antigravity 限制，用 `stringEnum` 代替
- **多 Agent 并发** — 不要创建/应用/删除 `git stash`，不要切换分支，除非明确要求

---

## 10. 信息来源

| 来源 | 链接 |
|------|------|
| GitHub | [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw) |
| 官方文档 | [docs.openclaw.ai](https://docs.openclaw.ai) |
| 入门教程 | [docs.openclaw.ai/start/getting-started](https://docs.openclaw.ai/start/getting-started) |
| 架构说明 | [docs.openclaw.ai/concepts/architecture](https://docs.openclaw.ai/concepts/architecture) |
| 技能文档 | [docs.openclaw.ai/tools/skills](https://docs.openclaw.ai/tools/skills) |
| ClawHub 技能商店 | [clawhub.com](https://clawhub.com) |
| DeepWiki | [deepwiki.com/openclaw/openclaw](https://deepwiki.com/openclaw/openclaw) |
| Discord 社区 | [discord.gg/clawd](https://discord.gg/clawd) |
| 发布文档 | [docs.openclaw.ai/install/updating](https://docs.openclaw.ai/install/updating) |
