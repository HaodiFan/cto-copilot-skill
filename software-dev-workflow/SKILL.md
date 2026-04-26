---
name: software-dev-workflow
description: 面向软件开发的中文 spec-driven 工作流 Skill。用于 Codex 需要根据软件项目形态、开发阶段、当前场景或缺失产物判断下一步该做什么时；适用于新建项目、项目脚手架、需求澄清、PRD/design doc、架构设计、目录结构、AGENTS.md、开发计划、实现前检查、验证门禁、PR/release readiness、Web+Backend、Desktop+Local Agent、Python Agent/CLI、Library/SDK、通用全栈 monorepo 等软件开发场景。
---

# 软件开发工作流

用 spec-driven、阶段感知的方式推进软件开发。触发后先判断项目形态和生命周期阶段，再读取对应 reference，输出下一步具体动作，而不是一次性倾倒整套方法论。

## 使用原则

每次使用先给出简短分类：

```text
项目形态: <Web+Backend | Desktop+Local Agent | Python Agent/CLI | Library/SDK | Full-stack Monorepo | Unknown>
当前阶段: <0 想法 | 1 需求澄清 | 2 Spec | 3 架构 | 4 脚手架 | 5 Feature 规划 | 6 实现 | 7 验证 | 8 PR/发布 | 9 维护>
主要产出: <文档 | 计划 | 代码变更 | review | checklist | 迁移方案>
```

然后只输出当前最小可用下一步。除非用户明确要求，不要把所有 reference 全部展开。

## 决策树

### 1. 是新项目吗？

- 如果用户从想法、空目录、新 repo、"怎么搭项目"开始，读取 `references/project-blueprints.md` 和 `references/spec-templates.md`。
- 输出推荐项目形态、初始目录结构、首次提交文件清单、bootstrap 顺序。

### 2. 是已有项目的新功能吗？

- 如果用户要新增 feature、页面、API、workflow、agent 能力、集成、后台任务，读取 `references/stage-playbook.md` 和 `references/spec-templates.md`。
- 先检查是否已有 design doc。没有 design doc 时，除非用户明确要求 quick spike，否则先创建或补齐 design doc。

### 3. 是实现任务吗？

- 如果用户要求写代码、修 bug、接接口、做迁移、实现功能，先检查 repo。
- 定位 canonical docs、folder declaration、scripts、tests、现有模式。
- 如果实现会改变架构边界、状态机、权限、存储、公共 API 或 UI 模式，先更新 spec，再写代码。
- 实现后运行最小相关验证，并说明未运行检查。

### 4. 是 review、救火或"下一步做什么"吗？

- 如果用户问"现在该干嘛"、"帮我 review 计划"、"项目很乱"、"继续开发"，读取 `references/stage-playbook.md` 和 `references/checklists.md`。
- 输出当前阶段、阻塞点、下一步 3 个动作、要创建/更新的文件、验证门禁。

### 5. 是方法论或 Skill/文档建设吗？

- 如果用户在写开发流程、项目规范、模板、方法论或可复用 playbook，按需读取所有 references，并产出简洁可复用的文档。

## 阶段路由

用这张表判断应读取哪个 reference。

| 阶段 | 典型信号 | Reference | 下一步重点 |
|---|---|---|---|
| 0 想法 | 只有模糊产品想法，无 repo | `spec-templates.md` | 一句话产品定义和 P0 非目标 |
| 1 需求澄清 | 用户、流程、边界不清 | `stage-playbook.md` | scope、assumption、open questions |
| 2 Spec | 用户要 PRD/design doc | `spec-templates.md` | 带验收标准的 design doc |
| 3 架构 | 技术栈、边界、真相源决策 | `project-blueprints.md` | 项目形态、真相源、模块边界 |
| 4 脚手架 | 新 repo、新目录、首次提交 | `project-blueprints.md` | starter tree 和 first commit 清单 |
| 5 Feature 规划 | 已有功能目标，缺拆解 | `stage-playbook.md` | milestone 和影响文件 |
| 6 实现 | 需要代码变更 | `checklists.md` | 最小验证和文档同步 |
| 7 验证 | 测试、QA、hardening | `checklists.md` | 验证矩阵和风险关闭 |
| 8 PR/发布 | 合并、发版、部署 | `checklists.md` | PR 内容、changelog、release gate |
| 9 维护 | 重构、清理、技术债 | `stage-playbook.md` | 保持行为、更新架构文档 |

## 项目形态路由

选择或描述项目形态时读取 `references/project-blueprints.md`。

- **Web + Backend**：管理台、Web App、外部平台集成、后端数据库是真相源。
- **Desktop + Local Agent**：本地数据、本地进程编排、离线优先、桌面分发、Agent runtime。
- **Python Agent / CLI**：命令行工具、SDK、自动化、Agent runtime、后台任务。
- **Library / SDK**：可复用包、公共 API、示例、兼容性承诺。
- **Full-stack Monorepo**：多个 app、共享协议、共享 UI、长期跨端演进。

## 输出格式

当用户问"下一步做什么"或要求规划时，使用：

```text
当前阶段:
项目形态:
缺失内容:
下一步 3 个动作:
要创建/更新的文件:
验证门禁:
停止条件:
```

当用户要求实现时，按正常代码工作流执行，但收尾必须包含：

- 已检查或创建的 spec/design doc
- 变更文件
- 已运行验证
- 剩余风险

## 核心规则

- spec/design doc 是意图真相源，代码是意图实现。
- 没有 spec，不新增架构层、状态机、存储、权限、全局依赖或公共 API。
- 全局真相源必须唯一：architecture、development rules、design rules、folder declaration、terminology、changelog。
- 一个 topic 一个分支，避免混入无关变更。
- 优先做最小端到端闭环，不做大面积半成品。
- 信息不足时，优先做可逆的最小假设并明确说明；只有错误假设代价高时才提问。

## References

- `references/stage-playbook.md`：按阶段判断信号、产物和下一步动作。
- `references/project-blueprints.md`：Web+Backend、Desktop+Local Agent、Python Agent/CLI、Library/SDK、Full-stack Monorepo 的 starter 结构。
- `references/spec-templates.md`：README、ARCHITECTURE、DEVELOPMENT、AGENTS、design doc、folder declaration、PR 模板。
- `references/checklists.md`：验证门禁、PR readiness、反模式和场景 checklist。
