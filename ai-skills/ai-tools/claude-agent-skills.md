# Claude Skills 开发者指南 — TL;DR

> **适合谁**：开发者、需要手动编写 SKILL.md 或 API 集成的技术用户
> **前置要求**：了解文件结构、YAML 语法、有 Claude Pro/Max/Team 账户
> **入门内容**：只想用 skill-creator 快速创建 → 看 [非技术用户指南](./claude-projects-custom-instructions.md)

---

## 1. 这是什么？

**一个包含 SKILL.md 文件的文件夹，定义 Claude 执行特定任务的指令、脚本和资源，可通过 Claude Code 或 API 部署。**

---

## 2. 它解决了什么具体问题？

- **问题**：skill-creator 生成的 Skill 无法满足复杂需求（自定义脚本、精确控制触发条件）
- **场景**：需要打包 Python/Bash 脚本、跨平台部署、团队共享标准化能力
- **变化**：从「对话式创建」变成「代码化定义 + 版本控制」

---

## 3. 什么时候该用？什么时候不该用？

### 适合使用：
- 需要在 Skill 中包含可执行脚本
- 需要精确控制 `description` 触发条件
- 通过 Claude Code 或 API 部署
- 团队需要版本控制和代码审查

### 不适合使用：
- 简单的工作流程（用 skill-creator 更快）
- 不熟悉文件结构或 YAML 语法

---

## 4. 核心机制（最多 3 点）

1. **SKILL.md**：入口文件，包含元数据（name、description）和指令
2. **按需加载**：Claude 根据 `description` 判断是否加载，只读取必要内容
3. **脚本执行**：可打包 Python/Bash 脚本，Claude 执行确定性计算

---

## 5. 最小可执行示例

### 前置步骤

```text
Settings → Capabilities → 开启「Code execution and file creation」
```

### 手动创建 Skill（10 分钟）

#### Step 1：创建文件夹结构

```text
my-skill/
├── SKILL.md          # 必须
└── resources/        # 可选
    └── template.md
```

#### Step 2：编写 SKILL.md

```yaml
---
name: weekly-report-generator
description: 当用户要求写周报、生成周报、创建本周总结时使用此技能
---

# 周报生成器

## 何时使用
用户请求包含：周报、本周总结、weekly report

## 执行步骤
1. 询问本周完成的主要工作
2. 询问遇到的问题和解决方案
3. 询问下周计划
4. 按模板生成周报

## 输出格式
- Markdown 格式
- 包含：本周完成、问题与方案、下周计划 三个部分
- 每部分 3-5 条要点

## 限制
- 不编造工作内容
- 不添加用户未提及的信息
```

#### Step 3：部署

**Claude Code**：
```bash
# 放置到项目根目录或全局目录
cp -r my-skill/ ~/.claude/skills/
```

**API**：
```bash
# 打包为 ZIP 上传
zip -r my-skill.zip my-skill/
# 通过 /v1/skills 端点上传
```

#### Step 4：测试

```text
输入："帮我写本周的周报"
观察：Claude 思维过程中应显示正在使用 weekly-report-generator
```

---

### SKILL.md 元数据字段

```yaml
---
name: skill-identifier      # 唯一标识（必填）
description: 触发条件描述    # Claude 用此判断是否加载（必填，关键！）
---
```

**关键**：`description` 决定 Claude 何时使用这个 Skill。写得太模糊 = 永远不会被触发。

---

### 包含脚本的 Skill

```text
my-skill/
├── SKILL.md
└── scripts/
    └── process.py    # Claude 可执行此脚本
```

在 SKILL.md 中引用：
```markdown
## 执行脚本
当需要处理数据时，执行 `scripts/process.py`
```

---

## 6. 常见误区 / 使用失败点

- ❌ **description 写得太模糊**：Claude 无法判断何时使用，Skill 从不被触发
- ❌ **SKILL.md 过于臃肿**：应拆分成多个引用文件，保持主文件精简
- ❌ **安装不信任来源的 Skill**：可能包含恶意脚本，务必审查代码
- ⚠️ **上传失败？**→ 检查 ZIP 内是否有 SKILL.md 文件
- ⚠️ **脚本不执行？**→ 确认 Code execution 已开启

---

## 7. 信息来源

- [Introducing Agent Skills - Anthropic](https://claude.com/blog/skills)
- [Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [Claude Developer Platform - Skills API](https://docs.anthropic.com)

---

## 8. 质量反馈（预留）

- 清晰度评分：⭐️⭐️⭐️⭐️⭐️
- 可执行性评分：⭐️⭐️⭐️⭐️⭐️
- 实用反馈：👍 / 👎
