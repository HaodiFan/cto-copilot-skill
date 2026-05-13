# CTO Copilot Skill（CTO 分身）

一个中文软件研发 **CTO 分身 Skill**：用户一句话进来时，先判断它属于哪个研发场景，再路由到正确的产品、架构、执行、验证、review、发布、治理、业务场景或经验沉淀知识。

它不是“先写代码”的提示词，而是让 agent 先像 CTO 一样判断方向、风险、真相源和下一步产物：该问需求就问需求，该补设计就补设计，该写代码就按最小切片写，该停下做合规判断就停下。

> 对外推荐名称：`CTO Copilot Skill` / `CTO 分身 Skill`。推荐 GitHub repo name：`cto-copilot-skill`。本次仍保留兼容 Skill ID 与目录：`software-dev-workflow/`，所以已有 `$software-dev-workflow` 触发方式不受影响。

## Status

- Version: 0.8.0
- Stage: active

## 它适合什么

- 从 0 做软件项目：想法、PRD、design doc、架构选型、脚手架。
- 接手旧项目：先盘点现状、尊重 as-is、补文档和第一周行动路径。
- 拆 feature：从需求到 implementation plan、vertical slices、validation gates。
- 做实现和修 bug：先判断 checkout/worktree、source-driven gate、最小切片和验证门禁。
- 做 review / PR / release：按角色化 review pipeline 和 PR readiness 收口。
- 做业务技术选型：RPA/电商数据采集、OCR、CV、LLM/Agent、数据分析、浏览器自动化。
- 建工程治理：Constitution、ADR、Memory Bank、Prompts、AGENTS.md、分支规范。
- 把用户纠偏沉淀成可复用 knowhow：lessons → patterns → reference。

## 使用场景地图

| 用户问法 | CTO 路由 | 优先读取 | 典型输出 |
|---|---|---|---|
| “我有个想法 / 想做个系统” | Product → Stage 0-2 | `stage-playbook.md`、`checklists.md`、`spec-templates.md` | 需求缺口、非目标、澄清问题、下一步产物 |
| “从 0 搭项目 / 技术栈怎么选” | Architecture → Stage 3-4 | `architecture-cases.md`、AI 项目加 `architecture-cases-ai.md`、`project-blueprints.md` | 项目形态、技术基线、starter tree、ADR 候选 |
| “接手这个老项目 / 继续迭代” | Inherit → Stage I | `inheriting-projects.md` | 自动识别、现状报告、第一周做与不做 |
| “这个功能怎么拆” | Execution Plan → Stage 5 | `execution-pipeline.md`、`checklists.md` | implementation plan、vertical slices、validation gates |
| “帮我实现 / 修 bug / 迁移” | Build → Stage 6 | `stage-playbook.md`、`execution-pipeline.md`、`agent-operating-standards.md` | 最小切片实现、验证结果、剩余风险 |
| “怎么测 / 是否 ready” | Validation → Stage 7 | `checklists.md`、`execution-pipeline.md` | 最小相关验证、缺口、release 风险 |
| “review / PR / release” | Review/Release → Stage 7-8 | `code-review-standards.md`、`checklists.md`、`execution-pipeline.md` | findings、PR readiness、rollback、learn 收口 |
| “RPA / 电商数据 / OCR / CV / LLM / 数据分析” | Scenario → 场景路线 | `scenario-playbooks.md`，必要时加 `architecture-cases-ai.md` | 默认选型、合规边界、最小切片、场景 checklist |
| “项目规范 / 文档 / agent 协作” | Governance | `templates-core.md`、`templates-governance.md`、`memory-bank-guide.md`、`prompts-guide.md` | 文档骨架、红线、ADR、Memory Bank |
| “这次纠偏记住 / 方法论升级” | Learn | `lessons.md`、`patterns-skill.md` | L1 lesson、L2 pattern、reference 升级建议 |

## CTO 路由模板

规划类回答先输出路由，再给计划：

```text
CTO路由: <主路由 | 命中场景 | 决策层>
当前阶段: <P POC/Spike | 0 想法 | 1 需求澄清 | 2 Spec | 3 架构 | 4 脚手架 | 5 Feature 规划 | 6 实现 | 7 验证 | 8 PR/发布 | 9 维护 | I 接手盘点>
项目形态: <Web+Backend | Desktop+Local Agent | Python Agent/CLI | Library/SDK | Full-stack Monorepo | Unknown>（如有场景，追加：场景 <RPA/OCR/LLM/...>）
缺失内容:
下一步 3 个动作:
要创建/更新的文件:
验证门禁:
停止条件:
```

执行类回答不强制完整模板，但收尾必须包含：变更文件 / 已运行验证 / 未运行检查及原因 / 剩余风险。

## Skill 内容

- Skill 目录：`software-dev-workflow/`
- 入口：`software-dev-workflow/SKILL.md`
- References：
  - `references/cto-scenario-map.md` —— CTO 分身入口地图：按生命周期、项目形态、业务场景、决策层做知识路由
  - `references/stage-playbook.md` —— Stage P 与 Stage 0–9 阶段判断与下一步动作（Stage I「接手盘点」单独成文）
  - `references/execution-pipeline.md` —— Stage 5–8 的硬 gate、checkout/worktree 策略、implementation plan 质量标准、角色化 review pipeline、后端优先验证、browser QA 触发边界与 learn 边界
  - `references/agent-operating-standards.md` —— agent 执行纪律：Skill 编写结构、common rationalizations、red flags、source-driven gate、change size rules、lifecycle prompt aliases
  - `references/scenario-playbooks.md` —— RPA/数据采集、OCR/文档智能、视觉质检、数据治理、LLM 生产链路、浏览器自动化、POC/spike 的场景化 playbook
  - `references/project-blueprints.md` —— 5 种项目形态的 starter 目录与首次提交清单
  - `references/architecture-cases.md` —— 通用架构选型 case 库（20 大类，深度档：定位 / 适用 / 反例 / 代价 / 退出成本 / 决策信号 / worked example / 踩坑 / 决策矩阵）
  - `references/architecture-cases-ai.md` —— AI / Agent 专项架构选型 case 库（11 大类，同样深度档）
  - `references/spec-templates.md` —— 模板索引，按需路由到 `templates-core.md` / `templates-governance.md` / `templates-specs.md`
  - `references/templates-core.md` —— README、ARCHITECTURE、DEVELOPMENT、BRANCHING、DESIGN、AGENTS 模板
  - `references/templates-governance.md` —— **CONSTITUTION（红线）**、**ADR（决策记录）**、Folder Declaration、glossary、changelog 模板
  - `references/templates-specs.md` —— Requirements Doc、Design Doc（含 backlog/active/done lifecycle）、Layout Spec、PR Body 模板
  - `references/checklists.md` —— 验证、需求完备性、新项目 / Feature / Bug fix / Refactor / PR Readiness、Constitution 红线触发处理、Memory Bank hygiene、反模式
  - `references/code-review-standards.md` —— 代码审查口径：代码坏味道、潜在行为风险、测试重点、密钥/硬编码风险、真人测试真源
  - `references/inheriting-projects.md` —— 接手已有项目的盘点流程（含 §1.0 自动识别）、现状报告模板、文档补齐顺序
  - `references/memory-bank-guide.md` —— AI agent 跨会话上下文的"指针 + 增量"模式（brief / tech-context / patterns / active-context）
  - `references/prompts-guide.md` —— 可复用 prompt 模板（lifecycle-aliases / scaffold / spike-start / handover-audit / new-feature / scenario-routing / new-design-doc / new-adr / pre-pr / update-active-context / refactor-safely / debug-incident / **capture-lesson** / **promote-pattern**）
  - `references/lessons.md` —— **Skill 级 L1 纠偏日志**（每次用户纠正方案 agent 主动追加；连续编号 L-NNNN）
  - `references/patterns-skill.md` —— **Skill 级 L2 验证过的 pattern 库**（来自 lessons 的合并；连续编号 P-NNNN）

## 主要能力

- 先判断 CTO 路由，再读取对应 reference，不把所有知识域一次性倾倒给用户
- 按"生命周期 / 项目形态 / 业务场景 / 决策层"四轴分流
- 按"新项目 vs 接手项目"分流入口
- 按业务自动化场景补充路由：RPA/采集、OCR/文档智能、视觉质检、数据治理、LLM 生产链路、浏览器自动化、POC/spike
- spec-driven 开发：业务需求由用户提供，agent 不编造
- 支持 Stage 0–4 的真实 8 步顺序：folders → branching → architecture+constitution+ADR → requirements+design lifecycle → layout → components → memory-bank+prompts → AGENTS.md
- 20 大类通用架构决策 + 11 大类 AI 架构决策的可查 case 库
- 项目红线（CONSTITUTION.md）+ 决策记录（ADR）+ 设计文档生命周期（backlog/active/done）三件套
- 执行管线：先判断 checkout/worktree 策略与 source-driven gate；多步 feature 必须有 implementation plan，Stage 7/8 按角色化 Review Pipeline 与后端优先验证收口
- Agent 执行纪律：反 rationalization、red flags、change size rules、lifecycle aliases，降低跳过 spec/test/source check 的概率
- AI agent 跨会话上下文（Memory Bank）+ 可复用动作模板（Prompts）
- 接手项目专用盘点流程（含自动识别），默认尊重现状；支持 legacy / pre-vibecoding 文档迁移
- 代码审查口径：优先检查《重构》坏味道、潜在行为风险与测试重点、secret / hardcode 风险，并保留真人验收为业务真源
- CTO 路由模板统一规划类输出格式

## v0.8.0 新增（CTO 分身定位 + 使用场景地图）

- 新增 `references/cto-scenario-map.md`，把 Skill 从“软件开发工作流集合”重定位为“CTO 分身”：先路由，再读取知识。
- `SKILL.md` description 改为 CTO 分身型中文软件研发决策 Skill，入口标题改为 `CTO Copilot / 软件研发 CTO 分身`。
- 规划类输出从原 7 字段升级为 CTO 路由模板，新增 `CTO路由: <主路由 | 命中场景 | 决策层>`。
- README 改为可传播的对外说明：先讲定位、适用场景、场景地图和使用方式，再列 references。
- `agents/openai.yaml` 同步对外显示名为 `CTO 分身`，默认 prompt 要求先判断场景路由。
- 对外推荐 repo name：`cto-copilot-skill`；兼容 Skill ID 仍保留 `software-dev-workflow`，避免破坏已有安装和触发路径。

### 版本号

- `SKILL.md` `0.7.0` → `0.8.0`
- `agents/openai.yaml` 同步 `0.8.0`
- `README.md` Version `0.8.0`

## v0.7.0 新增（Agent 执行纪律 + Source-driven Gate）

- 新增 `references/agent-operating-standards.md`，把从通用工程 skill 套件中适合吸收的部分落成稳定规则：Skill/Reference 编写标准、common rationalizations、red flags、source-driven gate、change size rules、lifecycle prompt aliases。
- `execution-pipeline.md` 增加 Source Gate：新依赖、外部 API、框架升级、平台规则、安全/合规事实必须用官方或项目内来源确认。
- `checklists.md` 增加 source-driven 验证、change size 自检和大 PR 拆分规则；PR readiness 要求说明大 diff 例外。
- `prompts-guide.md` 增加宿主无关的 lifecycle aliases：`/spec`、`/plan`、`/build`、`/test`、`/review`、`/ship`、`/learn`。
- `AGENTS.md` 模板补充 source-driven、PR 拆分、checkout/worktree、后端优先验证等 agent 规则。
- Owner 已确认默认策略：不自动逐 slice commit；不默认 fan-out 多 agent review；change size 使用启发式 + 大 PR 硬门禁；只在需求不清、高风险或架构边界变化时强制人工确认。

### 版本号

- `SKILL.md` `0.6.1` → `0.7.0`
- `agents/openai.yaml` 同步 `0.7.0`
- `README.md` Version `0.7.0`

## v0.6.1 新增（Checkout / Worktree 策略）

- 在 `references/execution-pipeline.md` 中新增 checkout/worktree 决策 gate：开工前必须识别当前分支、上游、脏工作区、任务类型和并行状态。
- 明确必须使用 worktree 的场景：共享分支开新任务、脏工作区重叠或来源不明、多 agent / 多任务并行、高风险改动、需要保留现场或用户要求隔离。
- 明确允许当前 checkout 直接开发的场景：当前分支已是目标任务/PR 分支，工作区干净或只含本任务改动，小修、文档、配置小改、单 PR follow-up，且无并行任务。
- 明确脏工作区规则：不得擅自 revert / reset / clean / stash 用户改动；重叠或来源不清时先停下让 owner 选择处理路径。
- `BRANCHING.md` 模板、Stage 5/6 路由、Implementation Plan checklist、PR Readiness checklist 同步加入该策略。

### 版本号

- `SKILL.md` `0.6.0` → `0.6.1`
- `agents/openai.yaml` 同步 `0.6.1`
- `README.md` Version `0.6.1`

## v0.6.0 新增（执行管线 + Review Pipeline）

- 新增 `references/execution-pipeline.md`，把 Stage 5–8 串成 Requirements → Spec → Architecture → Implementation Plan → Vertical Slice → Review Pipeline → QA → Release → Learn。
- 引入硬 gate：Requirements / Design / Architecture / Plan / Implementation / Review / Validation / Learn。POC 和事故救火有明确例外，但必须补证据。
- 引入 Implementation Plan 质量标准：目标、source docs、file map、vertical slices、validation gates、review roles、rollback、learn updates，禁止 TODO/TBD 式计划。
- 引入角色化 Review Pipeline：Product / Eng / Design / DX / Code / QA / Security / Release，按变更类型选择需要的 review 视角。
- 测试默认后端/API/service 自测优先；浏览器模拟只在用户明确要求、任务本身是浏览器/RPA能力、浏览器特有 bug 或 release gate 命中时运行。
- 明确 Learn 边界：项目级 memory-bank、Skill L1 lessons、Skill L2 patterns、reference canon 各写什么，避免双份真相源。

### 版本号

- `SKILL.md` `0.5.1` → `0.6.0`
- `agents/openai.yaml` 同步 `0.6.0`
- `README.md` Version `0.6.0`

## v0.5.1 新增（代码审查标准）

- 新增 `references/code-review-standards.md`，把代码审查优先级明确为：代码优雅与坏味道、潜在问题与测试重点、密钥/密码/硬编码风险。
- Review / PR review / 实现方法评审场景会额外读取代码审查标准，避免只检查编译错误或把产品约定误报成缺陷。
- PR Readiness checklist 增加 code review standards 自检项。

### 版本号

- `SKILL.md` `0.5.0` → `0.5.1`
- `agents/openai.yaml` 同步 `0.5.1`
- `README.md` Version `0.5.1`

## v0.5.0 新增（Skill 自我进化机制）

让 skill 真正能从用户纠偏中学习。每次用户说"不对/应该是/错了"，agent 主动捕获结构化 lesson；多条同主题 lesson 合并成 pattern；pattern 验证够了再升级到 reference。三层 knowhow 模型，每层都有连续编号、回溯链、反模式护栏。

### 新增机制

- **三层 knowhow 模型**：
  - L1 `references/lessons.md`：单条纠偏追加日志，agent 自己加，立即生效。编号 L-NNNN。
  - L2 `references/patterns-skill.md`：lessons 合并验证后的 pattern 库。编号 P-NNNN。
  - L3 Reference 升级：pattern 影响 reference 核心结论时，owner 开 PR 修改。
- **触发信号清单**：用户回复包含「不对/应该是/错了/我之前是/不要 X 要 Y/这一步不该现在做」等 5 类信号 → agent **必须**提议捕获 lesson。SKILL.md 与 lessons.md 都有完整词表。
- **Tag 词表**：覆盖阶段（stage-0–9 / P / I）、形态（form-*）、场景（scenario-*）、架构维度（arch-*）、工程实践（practice-*）、治理（gov-*）。多标签可组合检索。
- **Promotion 流程**：L1 → L2 触发条件（同主题 ≥ 2 条 / 用户确认通用）；L2 → L3 触发条件（与 reference 冲突 / 多项目验证 / ≥ 3 次引用且无反例）。每次 promotion 留下完整回溯链。
- **反模式护栏**：拒绝个人偏好、reference 重复内容、项目特定写法、敏感数据、多意图、缺适用条件的 lesson；拒绝缺反例 / 与 reference 冲突未标的 pattern。

### 集成点

- `SKILL.md` 加「Knowhow 沉淀规则」一节：会话开始检索 lessons / patterns，会话进行中识别捕获信号，会话结束时捕获 + 提议 promotion。
- `prompts-guide.md` 扩到 13 份基线模板：新增 `capture-lesson` 与 `promote-pattern`。
- `checklists.md` 加「Skill Knowhow Hygiene Checklist」：会话开始 / 进行中 / 结束三段自检。
- `stage-playbook.md` Stage 6 / 8 / 9 收尾步骤都加上「检查是否要 capture lesson」。
- `spec-templates.md` 路由表新增 lessons / patterns-skill 条目。

### 版本号

- `SKILL.md` `0.4.1` → `0.5.0`（新增机制层）
- `agents/openai.yaml` 同步 `0.5.0`
- `README.md` Version `0.5.0`

## v0.4.1 新增（fact-check + 可增长性优化）

- 三本 case / playbook 边界明文化：`architecture-cases.md`（通用架构）/ `architecture-cases-ai.md`（AI 架构）/ `scenario-playbooks.md`（业务场景落地），三者正交，不复制只指向。
- Stage 3 master checklist：`stage-playbook.md` Stage 3 直接指向 `architecture-cases.md` 文末「综合决策清单」+ AI 项目再跑 `architecture-cases-ai.md` 文末「AI Stage 3 决策清单」，不再跨文件拼装。
- 场景 checklist 补齐到 6 类：新增视觉 / 多媒体质检、数据治理 / 分析报表、浏览器自动化 / WebAgent 三组 checklist。
- 基线 prompt 扩到 11 份：新增 `spike-start`（POC/Spike 启动）与 `scenario-routing`（业务场景路由），回流 v0.4 的 POC 与场景层。
- POC 升级触发单一真相源：集中在 `scenario-playbooks.md` §G，`checklists.md` 仅指针。
- SKILL.md 增长规则：明确"路由器、不放内容、软上限 200 行、决策树分支上限 8 条、新 reference 必须三处登记"。
- 治理小修：`agents/openai.yaml` 加 `version` 字段联动 SKILL.md；`architecture-cases-ai.md` 编号约定与跨文件分工写在文件顶部；Design Doc `superseded by <slug>` vs ADR `superseded by ADR-MMMM` 风格差异显式说明。

## v0.4.0 新增

- 场景层 playbook：RPA/数据采集、OCR/文档智能、视觉/多媒体质检、数据治理/分析报表、LLM 生产链路、浏览器自动化、POC/spike
- POC / Spike 轻量治理模式：短期验证不强行套完整治理骨架，命中升级触发条件后切回正式项目流程
- Legacy 项目迁移路径：把旧式 PRD、refact_plans、零散 design docs 映射到 requirements/design/ADR/governance
- 桌面 + 本地 Agent 进阶蓝图：local-first、resource registry、managed/connected agent、MCP/CLI/exec 接口
- AI/LLM 工程化补充：prompt registry、trace、streaming timeout、replay、失败用例库
- 模板拆分：`spec-templates.md` 改为索引，具体模板拆到 core / governance / specs 三个 reference
- 修正 Stage 0–4 “真实 8 步顺序”的入口文案一致性

## v0.3.0 新增

- `CONSTITUTION.md` 模板（项目红线，触线立刻停下提示用户）
- ADR 模板（架构决策记录，与 ARCHITECTURE.md 配合）
- Design doc lifecycle：`docs/design/{backlog,active,done}/` 三目录 + Status 字段 + Validation Results 回填
- Memory Bank reference（`memory-bank-guide.md`）：四件套指针 + 增量模式
- Prompts reference（`prompts-guide.md`）：9 份基线 prompt 模板
- 接手项目 §1.0 自动识别清单：机器可判断的语言 / 形态 / 工具栈 / 框架 / AI 痕迹 / git 画像

## Install

按你使用的 agent / launcher 把 `software-dev-workflow/` 放到对应 skills 目录。常见路径示例：

```bash
# Codex CLI
cp -R software-dev-workflow ~/.codex/skills/

# 其他 agent：根据该 agent 的 skills 加载约定调整
```

> 不同 agent 的 skills 目录约定不同。本 Skill 的核心内容是 `SKILL.md` + `references/`，可被任何支持 system prompt / context 注入的 agent 加载。

## Usage

在新会话中触发：

```
$software-dev-workflow
```

或在对话中明确请求："使用 software-dev-workflow 作为 CTO 分身，先判断场景路由，再告诉我下一步具体该做什么。"

## 适用场景

- 新建项目（从想法到首次可运行）
- 接手已有项目（盘点 + 路径选择）
- 已有项目加 feature
- review、救火、"下一步做什么"
- RPA / OCR / CV / LLM / 数据分析等业务技术选型
- 写开发流程 / 项目规范 / 方法论
