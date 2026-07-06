# Codex 使用文档

> 适用对象：日常使用 Codex CLI / IDE / App 进行项目开发、代码审查、文档生成和流程自动化的开发者。
>
> 说明：Codex 的 `/` 菜单会根据环境、权限、已安装技能和版本略有不同。本文以常见命令和你截图中出现的功能为主。

---

## 1. Codex 的两类上下文能力

Codex 的长期行为主要由两类文件控制：

| 类型 | 作用 | 适合放什么 |
|---|---|---|
| `AGENTS.md` | 项目级或个人级的长期指导规则 | 构建命令、测试命令、代码规范、Review 要求、目录说明 |
| Skill / 技能 | 可复用的工作流能力 | 发布流程、PR Review 流程、测试验收流程、文档生成流程、团队 SOP |

简单理解：

- **`AGENTS.md`**：告诉 Codex “在这个项目里一直要遵守什么规则”。
- **Skill**：告诉 Codex “遇到某类任务时，按这套步骤做”。

例如：

```text
AGENTS.md：本项目使用 pnpm，提交前必须运行 pnpm lint 和 pnpm test。
Skill：当用户说“帮我发版”时，按固定发布检查清单执行。
```

---

## 2. `/` 菜单命令说明

在 Codex 输入框里键入 `/`，会弹出可用命令。命令会根据当前环境不同而变化。

### 2.1 `/ide`：IDE 上下文

**作用**  
让 Codex 读取当前 IDE 的上下文，例如当前选中的代码、打开的文件、编辑器状态等。

**什么时候用**

- 解释当前选中的代码。
- 基于当前打开文件修 bug。
- 让 Codex 根据当前编辑器上下文补测试。
- 当前问题和“眼前这段代码”强相关。

**示例**

```text
/ide
解释我当前选中的这段代码在做什么
```

```text
/ide
基于当前打开的文件，帮我找出可能导致这个报错的原因
```

---

### 2.2 `/mcp`：MCP 服务器状态

**作用**  
查看 Codex 已连接的 MCP 服务状态。MCP 用来连接外部工具和共享系统，例如 GitHub、Linear、Figma、内部文档系统等。

**什么时候用**

- Codex 需要访问本地仓库之外的外部系统。
- 你怀疑 MCP 没有连接成功。
- Codex 无法读取 issue、设计稿、外部文档或内部工具。

**示例**

```text
/mcp
```

```text
检查 GitHub MCP 是否在线，然后读取这个 PR 的上下文
```

---

### 2.3 `/personality`：个性 / 回答方式

**作用**  
调整 Codex 的协作风格，例如更简洁、更详细、更主动、更严格等。

**什么时候用**

- 你希望 Codex 少解释、多给结论。
- 你希望 Codex 适合学习源码，解释更多原因。
- 你希望 Review 时更严格。
- 你希望架构讨论时多给权衡方案。

**建议**

| 使用场景 | 建议风格 |
|---|---|
| 日常开发 | 简洁、直接 |
| 学习源码 | 解释型 |
| Code Review | 严格、关注风险 |
| 架构设计 | 主动提出方案和权衡 |

---

### 2.4 `/review`：代码审查

**作用**  
让 Codex 审查当前未提交改动，或者与某个 base 分支进行比较。

**什么时候用**

- 提交代码前。
- 发 PR 前。
- 合并分支前。
- 完成一轮重构后。
- 需要检查潜在 bug、边界条件、安全问题或破坏性变更时。

**示例**

```text
/review
审查当前未提交改动，重点关注 bug、边界条件和类型问题
```

```text
/review
和 main 分支比较，按严重程度输出问题
```

**建议输出格式**

```text
请按以下分类输出：
1. 必须修复
2. 建议修复
3. 风格建议
4. 测试建议
```

---

### 2.5 `/init`：初始化 `AGENTS.md`

**作用**  
为当前项目生成 `AGENTS.md` 脚手架。

**什么时候用**

- 第一次在项目中使用 Codex。
- 当前项目还没有 Codex 项目说明文件。
- 想把团队规范、构建命令、测试命令写入 Codex 长期上下文。

**示例**

```text
/init
```

然后继续让 Codex 完善：

```text
帮我完善 AGENTS.md，加入本项目的技术栈、目录结构、测试命令和代码风格
```

**推荐 `AGENTS.md` 内容**

```md
# AGENTS.md

## Project
这是一个 React + TypeScript 项目。

## Commands
- Install: pnpm install
- Dev: pnpm dev
- Lint: pnpm lint
- Test: pnpm test
- Type check: pnpm typecheck

## Rules
- 不要随意新增依赖。
- 修改 UI 时保持现有设计系统。
- 提交前运行 lint 和 test。
- 保持改动最小化。

## Directory notes
- 组件放在 `src/components`。
- API 请求封装在 `src/services`。
- 通用工具放在 `src/lib`。
```

---

### 2.6 `/feedback`：发送反馈

**作用**  
向 Codex 团队提交产品反馈，通常可以选择附带日志。

**什么时候用**

- Codex 无法读取 IDE 上下文。
- `/review` 没有识别改动。
- MCP 明明连接了但无法使用。
- Skill 没有出现或无法触发。
- UI、权限、连接状态异常。

**不适合用在**

- 普通代码 bug。
- 测试失败。
- 编译报错。
- 业务逻辑问题。

这些问题应该直接让 Codex 分析和修复，而不是提交反馈。

---

### 2.7 `/effort`：推理强度

**作用**  
调整 Codex 在任务中投入的推理深度。推理强度越高，越适合复杂任务，但通常会更慢、消耗更多额度。

**什么时候用**

| 任务类型 | 推荐推理强度 |
|---|---|
| 改文案、补小函数 | 低 / 中 |
| 普通 bug 修复 | 中 |
| 跨文件重构 | 高 |
| 架构设计 | 高 |
| 难复现 bug | 高 |
| 安全审查 | 高 |
| 大型 Code Review | 高 |

**建议**  
简单任务不要长期使用最高推理强度；复杂任务再切到高。

---

### 2.8 `/model`：选择模型

**作用**  
切换 Codex 当前使用的模型。

**什么时候用**

- 简单任务：优先选择更快的模型。
- 复杂调试：选择更强的模型。
- 架构设计：选择推理能力更强的模型。
- 大型 Review：选择更强模型，并搭配较高推理强度。

---

### 2.9 `/status`：状态

**作用**  
查看当前 thread ID、上下文使用情况、额度限制等。

**什么时候用**

- 感觉 Codex 变慢。
- 上下文很长，担心它遗漏早期信息。
- 想判断是否需要开启新会话。
- 需要排查当前会话或额度状态。

**示例**

```text
/status
```

```text
根据当前上下文使用情况，是否需要我开启新会话？如果需要，请先生成一份任务交接摘要。
```

---

### 2.10 `/goal`：目标模式

**作用**  
设置一个持续目标，让 Codex 持续推进，直到完成、暂停或需要你补充信息。

**什么时候用**

- 实现一个完整功能。
- 修复一组相关 bug。
- 重构一个模块。
- 补齐测试。
- 按 PRD 完成一批改动。

**示例**

```text
/goal
完成用户登录页重构：保留现有功能，修复移动端样式问题，补充测试，并确保 pnpm test 通过
```

**推荐流程**

先规划：

```text
/plan
先分析这个需求，给出实现计划，不要改代码
```

确认方案后：

```text
/goal
按刚才确认的计划完成实现，并补充测试
```

---

### 2.11 `/plan`：计划模式

**作用**  
让 Codex 先制定方案，而不是马上修改代码。

**什么时候用**

- 任务复杂。
- 涉及多个文件。
- 你还不确定实现路径。
- 可能存在架构风险。
- 需要先评估改动范围。

**示例**

```text
/plan
我想把登录逻辑从组件里抽到 hooks 和 service 层，先给我修改计划，不要直接改代码
```

**建议**  
复杂任务不要一上来就让 Codex 改代码。先 `/plan`，确认后再执行，成功率更高。

---

## 3. Skills / 技能：如何使用与创建

### 3.1 技能是什么

Skill 是一个可复用工作流。它通常是一个文件夹，里面至少包含一个 `SKILL.md` 文件，也可以包含脚本、参考资料、模板和资源。

基本结构：

```text
my-skill/
  SKILL.md          # 必需：技能说明和执行步骤
  scripts/          # 可选：脚本
  references/       # 可选：参考文档
  assets/           # 可选：模板或资源
```

`SKILL.md` 至少需要包含 `name` 和 `description`：

```md
---
name: my-skill
description: 当用户需要执行某个固定工作流时使用。清楚说明什么时候应该触发，什么时候不应该触发。
---

这里写 Codex 应该遵循的具体步骤、约束和输出格式。
```

---

### 3.2 技能怎么被使用

Codex 使用 Skill 有两种方式：

#### 方式一：显式调用

在输入框中输入 `$`，然后选择某个 Skill。

示例：

```text
$release-checklist
帮我检查当前分支是否可以发版
```

或者：

```text
$frontend-review
检查当前页面的可访问性、响应式和控制台错误
```

#### 方式二：自动触发

Codex 会根据 Skill 的 `description` 判断是否应该使用它。

例如某个 Skill 的描述是：

```md
---
name: release-checklist
description: Use when the user wants to prepare a release, validate release readiness, generate changelog, or run pre-release checks.
---
```

当你说：

```text
帮我做发版前检查
```

Codex 就可能自动选择这个 Skill。

**重点**：`description` 写得越清楚，自动触发越稳定。

---

### 3.3 什么时候应该创建 Skill

适合创建 Skill 的场景：

- 某个流程会反复执行。
- 每次都需要相同的步骤、检查项或输出格式。
- 团队有固定 SOP。
- 任务需要引用模板、示例、脚本或参考资料。
- 你希望不同项目、不同会话里都能复用同一套流程。

典型例子：

| Skill | 用途 |
|---|---|
| `frontend-review` | 前端页面验收：样式、响应式、可访问性、控制台错误 |
| `release-checklist` | 发版检查：测试、版本号、changelog、回滚方案 |
| `pr-description` | 生成统一格式的 PR 描述 |
| `api-docs` | 根据接口代码生成 API 文档 |
| `bug-triage` | 按固定流程定位 bug、收集证据、给修复建议 |
| `test-case-writer` | 按团队模板生成测试用例 |

不建议创建 Skill 的场景：

- 只执行一次的临时任务。
- 只是一条简单规则。
- 某个规则只属于当前项目，且应该每次都生效。

这些更适合写进 `AGENTS.md`。

---

## 4. 创建全局技能

全局技能适合“所有项目都可能用到”的个人工作流。

### 4.1 存放位置

全局技能放在：

```text
$HOME/.agents/skills
```

例如：

```text
~/.agents/skills/
  pr-description/
    SKILL.md
  release-checklist/
    SKILL.md
  bug-triage/
    SKILL.md
```

这些技能会对你打开的各个项目可用。

---

### 4.2 创建方式一：使用内置 Skill Creator

在 Codex 中输入：

```text
$skill-creator
```

然后告诉 Codex：

```text
创建一个全局技能，名称为 pr-description。
用途：当我准备发 PR 时，根据当前改动生成统一格式的 PR 描述。
要求：包含 Summary、Changes、Tests、Risks、Rollback Plan。
请把它保存到 ~/.agents/skills/pr-description/SKILL.md。
```

---

### 4.3 创建方式二：手动创建

```bash
mkdir -p ~/.agents/skills/pr-description
cat > ~/.agents/skills/pr-description/SKILL.md <<'EOF'
---
name: pr-description
description: Use when the user wants to create or polish a pull request description based on current changes, commits, or branch diff.
---

# PR Description Skill

When this skill is used:

1. Inspect the current diff or ask the user for the diff if unavailable.
2. Summarize the change in plain language.
3. Group changes by feature, bug fix, refactor, test, or docs.
4. Include testing information if available.
5. Call out risks, migration notes, and rollback plan.
6. Use the following output format:

## Summary

## Changes

## Tests

## Risks

## Rollback Plan
EOF
```

创建后，如果 Codex 没有立刻识别，重启 Codex 或开启新会话。

---

## 5. 创建某个项目的专属技能

项目专属技能适合“只对当前仓库有效”的流程。它应该随代码一起提交，方便团队共享。

### 5.1 存放位置

项目技能放在仓库内：

```text
repo-root/.agents/skills
```

例如：

```text
my-project/
  AGENTS.md
  .agents/
    skills/
      frontend-review/
        SKILL.md
      api-contract-check/
        SKILL.md
      release-checklist/
        SKILL.md
  src/
  package.json
```

---

### 5.2 适合放项目技能的内容

项目技能适合写：

- 本项目专属发布流程。
- 本项目页面验收流程。
- 本项目 API 契约检查流程。
- 本项目测试数据生成流程。
- 本项目设计系统使用规则。
- 本项目代码生成模板。

例如：

```text
my-project/.agents/skills/frontend-review/SKILL.md
```

```md
---
name: frontend-review
description: Use when reviewing UI changes in this repository, especially page layout, responsive behavior, accessibility, and console errors.
---

# Frontend Review Skill

When reviewing UI changes in this repository:

1. Start the dev server with `pnpm dev` if needed.
2. Open the affected page in the browser.
3. Check desktop and mobile layouts.
4. Check keyboard navigation and visible focus states.
5. Check console errors.
6. Verify components use the project design system.
7. Do not introduce new UI libraries without approval.
8. Output findings as:
   - Blocking issues
   - Non-blocking issues
   - Suggested improvements
   - Tests performed
```

---

### 5.3 子目录专属技能

如果一个仓库很大，可以把技能放在更靠近业务模块的目录中。

例如：

```text
repo-root/
  .agents/skills/
    general-review/
      SKILL.md
  services/
    payments/
      .agents/skills/
        payment-release-check/
          SKILL.md
```

当你从 `services/payments` 目录启动 Codex 时，Codex 可以扫描当前目录到仓库根目录之间的 `.agents/skills`。因此，越靠近当前工作目录的技能越适合放模块专属流程。

---

## 6. 全局技能 vs 项目技能：怎么选

| 问题 | 放全局技能 | 放项目技能 |
|---|---:|---:|
| 这个流程是否所有项目都用？ | 是 | 否 |
| 是否包含个人偏好？ | 是 | 否 |
| 是否应该随仓库共享给团队？ | 否 | 是 |
| 是否依赖项目特定命令、目录、业务规则？ | 否 | 是 |
| 是否需要代码评审后提交到仓库？ | 通常不需要 | 需要 |

推荐原则：

- **个人习惯**放全局技能。
- **团队流程**放项目技能。
- **项目规则**写 `AGENTS.md`。
- **重复工作流**写 Skill。

---

## 7. Skill 与 AGENTS.md 的分工

| 内容 | 推荐位置 | 示例 |
|---|---|---|
| 每次都要遵守的项目规则 | `AGENTS.md` | 使用 pnpm、不要新增依赖、测试命令 |
| 某类任务的固定步骤 | Skill | 发版流程、PR 描述、UI 验收 |
| 个人沟通偏好 | `~/.codex/AGENTS.md` | 回答简洁、Review 严格 |
| 团队共享开发规范 | 仓库 `AGENTS.md` | 目录结构、代码风格、测试策略 |
| 需要模板或脚本的流程 | Skill | 生成报告、检查 changelog、跑验证脚本 |

一个常见组合：

```text
~/.codex/AGENTS.md
  个人偏好：回答简洁、先给结论、Review 时严格

repo-root/AGENTS.md
  项目规则：技术栈、命令、目录结构、测试要求

repo-root/.agents/skills/release-checklist/SKILL.md
  项目发版流程：版本号、changelog、测试、回滚
```

---

## 8. 推荐日常使用流程

### 8.1 第一次接入项目

```text
/init
```

然后：

```text
帮我完善 AGENTS.md，加入项目技术栈、目录结构、构建命令、测试命令和代码规范
```

如果团队有固定流程，再创建项目技能：

```text
$skill-creator
创建一个项目专属 release-checklist 技能，保存到 .agents/skills/release-checklist/SKILL.md
```

---

### 8.2 实现复杂功能

```text
/plan
先分析需求和改动范围，给出实现计划，不要直接改代码
```

确认方案后：

```text
/goal
按刚才确认的计划实现功能，补充测试，并确保项目检查命令通过
```

完成后：

```text
/review
审查当前改动，重点关注 bug、边界条件、类型问题和破坏性变更
```

---

### 8.3 发 PR 前

```text
/review
和 main 分支比较，按严重程度输出问题
```

然后调用 PR 描述技能：

```text
$pr-description
根据当前 diff 生成 PR 描述
```

---

### 8.4 发版前

```text
$release-checklist
执行发版前检查，列出阻塞项、风险和回滚方案
```

---

## 9. 常见问题

### Q1：Skill 创建后为什么没有出现？

可能原因：

- `SKILL.md` 路径不对。
- `SKILL.md` 缺少 `name` 或 `description`。
- Codex 尚未重新加载技能。
- 技能描述太长或太模糊，Codex 没有识别。

处理方式：

```text
重启 Codex 或开启新会话。
```

也可以让 Codex 检查：

```text
检查 .agents/skills 下的技能是否符合 Codex Skill 格式
```

---

### Q2：Skill 和 `/` 命令是什么关系？

`/` 命令是 Codex 内置或环境提供的控制命令。  
Skill 是你或系统安装的可复用工作流。

已启用的技能可能会出现在 `/` 菜单里，也可以通过 `$` 显式调用。

---

### Q3：什么时候不要创建 Skill？

以下情况不建议创建 Skill：

- 只是一次性任务。
- 只是临时提示词。
- 只是一个项目长期规则。
- 规则很短，写进 `AGENTS.md` 更合适。

---

### Q4：能不能把所有需求都写进 Skill？

不建议。

推荐拆分：

```text
AGENTS.md              # 稳定规则
PRD.md                 # 产品需求
ARCHITECTURE.md        # 架构说明
.agents/skills/...     # 重复工作流
```

在 `AGENTS.md` 中引用需求文档：

```md
## Required reading
Before making product-level changes, read:
- docs/PRD.md
- docs/ARCHITECTURE.md
```

---

## 10. 快速记忆

| 你想做什么 | 用什么 |
|---|---|
| 让 Codex 看当前 IDE 代码 | `/ide` |
| 检查外部工具连接 | `/mcp` |
| 调整回答风格 | `/personality` |
| 审查代码改动 | `/review` |
| 生成项目说明文件 | `/init` |
| 反馈 Codex 产品问题 | `/feedback` |
| 调整思考深度 | `/effort` |
| 切换模型 | `/model` |
| 查看上下文和额度 | `/status` |
| 持续完成一个目标 | `/goal` |
| 先规划再动手 | `/plan` |
| 复用固定工作流 | Skill |
| 所有项目通用的工作流 | 全局 Skill：`~/.agents/skills` |
| 当前项目专用工作流 | 项目 Skill：`.agents/skills` |
| 项目长期规则 | `AGENTS.md` |

---

## 11. 参考来源

- OpenAI Codex Commands: https://developers.openai.com/codex/app/commands
- OpenAI Codex Agent Skills: https://developers.openai.com/codex/skills
- OpenAI Codex Customization: https://developers.openai.com/codex/concepts/customization
- OpenAI Codex AGENTS.md: https://developers.openai.com/codex/guides/agents-md
