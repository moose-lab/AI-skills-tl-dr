# Claude 定制化入门（Projects + Skills）— TL;DR

> **适合谁**：非技术用户、想用浏览器快速定制 Claude 的人
> **前置要求**：Claude Pro/Team 账户、会用浏览器
> **进阶内容**：需要手动编写 SKILL.md 或 API 集成 → 看 [开发者指南](./claude-agent-skills.md)

---

## 1. 这是什么？

**让 Claude 记住你的偏好和工作方式，不用每次重复解释。**

---

## 2. Claude 定制化功能对比

| 功能 | 作用域 | 适合场景 | 难度 |
|-----|-------|---------|------|
| Custom Instructions | 账户级（所有对话） | 通用偏好（语气、格式） | 简单 |
| Projects | 项目级 | 特定项目的上下文和文档 | 简单 |
| Skills | 按需加载 | 可复用的工作流程 | 简单 |

**本文覆盖以上三种功能的基础用法。**

---

## 3. 什么时候该用？什么时候不该用？

### 适合使用：
- 希望 Claude 记住你的写作风格、语气偏好
- 需要 Claude 了解特定项目的背景资料
- 有重复性任务想让 Claude 按固定流程执行

### 不适合使用：
- 一次性简单问题
- 需要手动编写脚本或 API 集成（→ 看[开发者指南](./claude-agent-skills.md)）

---

## 4. 核心机制（3 种功能）

1. **Custom Instructions**：账户级设置，所有对话生效
2. **Projects**：项目级设置，上传文档 + 指令，仅该项目生效
3. **Skills**：可复用能力模块，Claude 自动识别并调用

---

## 5. 最小可执行示例

### 前提条件
- Claude Pro / Team 账户
- 访问 [claude.ai](https://claude.ai)

---

### 功能 A：Custom Instructions（2 分钟）

```text
1. 点击左下角头像 → Settings → Custom instructions

2. 输入你的通用偏好，例如：
   "用简洁的中文回答，避免使用 emoji，代码要有注释"

3. 保存 → 所有新对话都会遵循这些指令
```

---

### 功能 B：Projects（5 分钟）

```text
1. 点击左侧边栏 "Projects" → "Create Project"

2. 输入项目名称，如："Q1 营销方案"

3. 添加 Project Knowledge
   - 点击 "Add content"
   - 上传文件或粘贴文本（最多 200K token ≈ 500 页）

4. 设置 Custom Instructions（项目级）
   - 点击 "Set instructions"
   - 输入项目专属指令

5. 在该 Project 中发起对话，Claude 会自动参考这些资料
```

---

### 功能 C：Skills（5 分钟）

**Skills = 可复用的工作流程，Claude 自动识别并调用。**

#### 前置步骤（首次必做）

```text
Settings → Capabilities → 开启「Code execution and file creation」→ 确认 Skills 区域可见
```

⚠️ Team/Enterprise 用户：需管理员先启用

#### 创建 Skill

```text
1. 新对话输入："帮我创建一个 Skill，用于 [你的用途]"
   示例："帮我创建一个 Skill，用于写符合我公司风格的周报"

2. 回答 Claude 的 2-3 个澄清问题

3. Claude 生成 Skill 后，点击「Enable Skill」直接启用
   └─ 或点击「Download」→ Settings > Skills > Upload skill

4. 测试：新对话中输入相关任务，观察 Claude 是否自动调用

5. 迭代：回到创建对话，说「把 XX 改成 YY」→ 重新启用
```

#### 内置 Skills（无需创建）

Claude 已自带：Excel / Word / PPT / PDF 处理能力，直接描述需求即可。

---

## 6. 常见误区 / 使用失败点

- ❌ **指令过于模糊**：「写得好一点」不如「使用短句，每段不超过 3 句」
- ❌ **上传太多无关文档**：会稀释重要信息，降低回答质量
- ⚠️ **Skills 选项灰显？**→ Code execution 未开启，去 Settings 检查
- ⚠️ **Claude 没调用我的 Skill？**→ 检查 toggle 是否开启

---

## 7. 信息来源

- [Collaborate with Claude on Projects - Anthropic](https://www.anthropic.com/news/projects)
- [Introducing Agent Skills - Anthropic](https://claude.com/blog/skills)
- [Claude 帮助中心](https://support.anthropic.com)

---

## 8. 质量反馈（预留）

- 清晰度评分：⭐️⭐️⭐️⭐️⭐️
- 可执行性评分：⭐️⭐️⭐️⭐️⭐️
- 实用反馈：👍 / 👎
