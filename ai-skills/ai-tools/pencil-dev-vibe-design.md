# Pencil.dev — TL;DR

> **适合谁**：正在用 Cursor / Vibe Coding 的开发者，想直接在 IDE 里完成 UI 设计
> **前置要求**：Cursor IDE（已安装）
> **阅读时间**：3 分钟

---

## 1. 一句话定义

**Cursor 的设计模式插件——在 IDE 内提供矢量画布，设计文件直接存入 Git 仓库，与代码同步版本控制。**

---

## 2. 解决什么问题？

| 传统方式 | Pencil.dev 方式 |
|---------|----------------|
| Figma 设计 → 导出 → 开发者还原 | 画布即代码，设计直接落地 |
| 设计稿和代码分离，版本不同步 | 设计文件在 Git 仓库中，和代码一起 branch/merge |
| AI 只能生成代码，无法操作视觉 | AI Agent 可同时操作画布和代码 |
| 切换 Figma ↔ IDE 的上下文成本 | 一个 Tab 搞定设计+开发 |

**核心概念**：Vibe Design = 用意图驱动设计，让 AI 处理像素级实现（类比 Vibe Coding）

---

## 3. 适用边界

| 适合 | 不适合 |
|-----|-------|
| 快速原型 → 直接变代码 | 大型设计系统管理（Figma 更成熟） |
| 独立开发者 / 小团队全栈开发 | 纯设计师工作流（不写代码） |
| 需要设计和代码版本一致 | 已有成熟 Figma → 前端交付流程的大团队 |
| Vibe Coding 工作流中需要视觉设计 | 非 Cursor 用户（目前仅支持 Cursor） |

---

## 4. 核心机制（3 点）

1. **IDE 内嵌画布**：像素级矢量画布直接嵌入 Cursor，设计和代码在同一环境
2. **Git 原生**：设计文件存储在仓库中，支持 branch、merge、diff、code review
3. **MCP 协议集成**：通过 Model Context Protocol 连接 Cursor 的 AI Agent，AI 可读取画布并生成对应代码

---

## 5. 快速上手（5 分钟）

### Step 1：下载桌面应用

```
访问 pencil.dev/downloads（或 trypencil.dev）→ 下载 Desktop App
```

> Pencil 是独立桌面应用，不是 Cursor 插件

### Step 2：链接 Chrome 和 Cursor

安装后，按引导将 Pencil 与 Chrome 浏览器和 Cursor IDE 连接，使其能感知你的工作上下文。

### Step 3：在 Cursor 中使用画布

```
Cursor 聊天框输入："Open pencil canvas"
→ 画布即时弹出，可分屏显示
```

### Step 4：设计 → 代码

1. 在画布上绘制 UI 组件（或从 Figma 复制粘贴）
2. AI Agent 自动识别画布元素
3. 生成对应的前端代码
4. 代码和设计文件一起 commit 到 Git

### 从 Figma 导入

```
Figma 中选中 Frame → Ctrl/Cmd+C → Pencil 画布中粘贴
→ 保留矢量、文字、样式
```

---

## 6. 典型工作流

```
构思 → Pencil 画布草图 → AI 生成代码 → 调整画布 → 代码同步更新 → git commit
```

对比传统流程：
```
构思 → Figma 设计 → 导出标注 → 开发还原 → 设计走查 → 修改 → 再还原...
```

**效率参考**：登录页从原型到代码，传统约 1 小时，Pencil 约 20 分钟（来源：Medium 用户测试）

---

## 7. 常见误区

| 误区 | 事实 |
|-----|-----|
| 「能完全替代 Figma」 | 不能。复杂设计系统、多人协作仍需 Figma |
| 「AI 生成的代码可以直接用」 | AI 输出需要人工审查，可能不完整或有误 |
| 「只是个画图工具」 | 核心价值是 Git 版本控制 + AI 代码生成的集成 |
| 「需要设计基础」 | 可以从 Figma 粘贴，或用 AI 辅助设计 |

---

## 8. 当前状态

| 项目 | 状态 |
|-----|-----|
| 定价 | 免费（未来可能推出付费功能） |
| 平台 | 仅 Cursor IDE |
| 公司 | High Agency, Inc.（美国加州） |
| 阶段 | 早期产品，快速迭代中 |

---

## 9. 信息来源

- [Pencil.dev 官网](https://www.pencil.dev/) — 产品主页
- [Abduzeedo 评测](https://abduzeedo.com/pencildev-missing-link-between-design-and-vibe-coding) — 功能深度分析
- [UI UX Showcase](https://uiuxshowcase.com/resources/pencil/) — 使用场景展示
- [Pencil.dev 服务条款](https://pencil.dev/terms-of-use) — 公司和许可信息

---

## 10. 质量反馈（预留）

- 清晰度评分：待评
- 可执行性评分：待评
- 实用反馈：👍 / 👎
