# Spec 模板

用于创建或修复项目文档。所有模板按 Stage 0–4 出现的顺序排列。

文档清单（典型新项目首次提交后的真相源结构）：

```text
README.md                              # 项目入口
ARCHITECTURE.md                        # 架构真相源
DEVELOPMENT.md                         # 开发规则
BRANCHING.md                           # 分支与提交规范（独立产物）
DESIGN.md                              # 设计系统真相源（UI 类项目必备）
AGENTS.md                              # AI agent 项目级约束
CONSTITUTION.md                        # 项目红线（不可妥协规则）
docs/
├── requirements/
│   └── requirements-v0.0.1.md         # 用户提供的需求文档
├── design/
│   ├── active/                        # 当前在做的 design doc
│   │   └── design_doc-v0.0.1-bootstrap.md
│   ├── backlog/                       # 已起草但未启动
│   ├── done/                          # 已完成并归档
│   └── layout-spec-<page>.md          # 页面布局 md（UI 类项目）
├── decisions/                         # ADR（架构决策记录）
│   └── ADR-0001-record-architecture-decisions.md
├── memory-bank/                       # AI agent 跨会话上下文（指针 + 增量）
│   ├── brief.md
│   ├── tech-context.md
│   ├── patterns.md
│   └── active-context.md
├── prompts/                           # 可复用 prompt 模板
│   └── README.md
└── governance/
    ├── folder-declaration-v0.md
    ├── terminology-glossary.md
    └── changelog.md
```

> Memory Bank 详见 `memory-bank-guide.md`；prompts 详见 `prompts-guide.md`。

---

## README.md

````md
# <Project>

<一句话项目定义。>

## Status

<pre-dev | prototype | active | uat | production>

## Start Here

- [Architecture](./ARCHITECTURE.md)
- [Constitution](./CONSTITUTION.md)
- [Development Rules](./DEVELOPMENT.md)
- [Branching](./BRANCHING.md)
- [Design System](./DESIGN.md)
- [Requirements](./docs/requirements/)
- [Design Docs](./docs/design/) · [Decisions (ADR)](./docs/decisions/)
- [Memory Bank](./docs/memory-bank/) · [Prompts](./docs/prompts/)
- [Folder Declaration](./docs/governance/folder-declaration-v0.md)
- [Changelog](./docs/governance/changelog.md)

## Current Structure

| Directory | Purpose |
|---|---|
| `apps/` | 用户可运行的应用 |
| `services/` | 后端服务 |
| `packages/` | 共享代码 |
| `docs/` | 项目文档 |
| `scripts/` | 开发和运维脚本 |
| `configs/` | 配置模板 |

## Local Run

```bash
<install command>
<dev command>
```
````

---

## ARCHITECTURE.md

```md
# Architecture

- Status: active
- Last updated: <date>

## 一句话架构结论

<P0 架构决策和核心非目标。>

## Goals

## Non-goals

## System Boundaries

### <Frontend/App>
负责：
不负责：

### <Backend/Runtime>
负责：
不负责：

### <External Platform>
负责：
不负责：

## Technical Baseline

逐项记录关键架构决策（来自 `architecture-cases.md` 19 大类 + AI 项目的 `architecture-cases-ai.md`）：

| 维度 | 选定方案 | 主要理由 | 主要代价 | 退出成本 |
|---|---|---|---|---|
| Repo 组织 | | | | |
| 渲染模式 | | | | |
| 前后端关系 | | | | |
| 后端架构 | | | | |
| 数据架构 | | | | |
| 通信协议 | | | | |
| 鉴权 | | | | |
| 部署 | | | | |
| 异步任务 | | | | |
| 可观测性 | | | | |
| ... | | | | |

## Data Truth Source

## Key Flows

## Risks and Future Phases
```

---

## DEVELOPMENT.md

```md
# Development Rules

## Spec-driven Development

- 每个正式开发任务都必须关联 design doc。
- 业务需求文档由用户提供，agent 不编造。
- 如果实现偏离 design doc，先更新 design doc，再继续实现。
- 架构边界变化必须更新 `ARCHITECTURE.md`。
- 重要变更必须更新 `docs/governance/changelog.md`。
- UI 变更必须更新 `DESIGN.md` 与 layout spec。

## Branch / Commit / PR

详见 `BRANCHING.md`（真相源）。本文件不重复定义。

## Testing Rules

运行最小相关检查。如果无法运行，说明原因和风险。

## Definition of Done

- 已关联 design doc 与需求文档
- 代码落在正确架构边界内
- 已运行验证或明确阻塞原因
- 必要文档/changelog 已更新
- 分支与提交符合 `BRANCHING.md`
```

---

## BRANCHING.md

> 分支规范是独立真相源。**Stage 3 架构决策时落地，先于脚手架代码**。

```md
# Branching & Commit Rules

- Status: active
- Last updated: <date>

## 策略

选用：<trunk-based | git-flow | GitHub flow | 自定义>

理由：<团队规模、发布频率、回滚成本>

## 长期分支

| 分支 | 用途 | 谁能 push | 部署目标 |
|---|---|---|---|
| `main` | 生产基线 | 仅 PR merge | production |
| `uat` | 验收 | 仅 PR merge | uat env |
| `dev` | 日常集成（可选） | PR merge | staging |

## 短期分支命名

`<type>/<topic>` 或 `codex/<type>/<topic>` 或 `<user>/<type>/<topic>`

`<type>` ∈ { feat, fix, refactor, docs, test, chore, perf, build, ci, hotfix, spike }

`<topic>` 用 kebab-case，体现单一意图。

## 一个分支一个意图

- 不混合 feature + refactor + 依赖升级 + 大范围格式化。
- spike 分支默认不 merge，验证完丢弃或拆成正式 PR。

## Commit Rules

Conventional Commits：`<type>(<scope>): <subject>`

- subject 用现在时祈使句，不超过 72 字符。
- body 解释 why 与 what，不写 how（how 在 diff 里）。
- footer 引用 issue / design doc：`Refs: docs/design/...`。

## Merge 策略

| 场景 | 策略 |
|---|---|
| feature → 集成分支 | <squash | rebase | merge> |
| hotfix → main | <fast-forward | merge> |

理由：<可追溯性 vs 历史清洁度>

## PR 规则

- PR 必须填写完整 PR body 模板（见下文）。
- 必须 link design doc 与 requirements doc。
- 至少 1 名 reviewer，UI 改动需设计 reviewer。
- CI 全绿前不合并。

## Tag / Release

- 版本号：<SemVer | CalVer>
- Tag 命名：`v<major>.<minor>.<patch>`
- Release notes 来源：`docs/governance/changelog.md`

## Hotfix 流程

1. 从 `main` 拉 `hotfix/<topic>`
2. 修复 + 最小回归测试
3. PR → `main`，合并后 cherry-pick 回 `dev`/`uat`
4. 立刻打 tag 与发布
```

---

## DESIGN.md

> UI 类项目必备。是组件选择与交互模式的真相源。**Stage 4 在写 layout spec 之前必须先有 v0**。

```md
# Design System

- Status: active
- Last updated: <date>

## 设计原则

- <如：信息密度优先 / 减少认知成本 / 移动优先 / 桌面优先>
- <一致性、反馈、可恢复性、可访问性>

## Design Tokens

### Color

| Token | Hex | 用途 |
|---|---|---|
| `--color-bg` | | 主背景 |
| `--color-fg` | | 主文字 |
| `--color-primary` | | 主色 / CTA |
| `--color-success` | | |
| `--color-warning` | | |
| `--color-danger` | | |
| `--color-muted` | | 次要文字 |

### Typography

| Token | 字号 | 字重 | 行高 | 用途 |
|---|---|---|---|---|
| `--text-display` | | | | 大标题 |
| `--text-h1` | | | | |
| `--text-body` | | | | 正文 |
| `--text-caption` | | | | 辅助 |

### Spacing

| Token | 值 | 用途 |
|---|---|---|
| `--space-1` | 4px | |
| `--space-2` | 8px | |
| `--space-3` | 12px | |
| `--space-4` | 16px | |
| `--space-6` | 24px | |
| `--space-8` | 32px | |

### Radius / Shadow / Motion

| Token | 值 |
|---|---|
| `--radius-sm` | |
| `--radius-md` | |
| `--shadow-1` | |
| `--motion-fast` | 150ms |

## 组件清单

每个组件给：用途 / 何时用 / 何时不用 / 变体。

| 组件 | 用途 | 变体 | 状态 |
|---|---|---|---|
| `Button` | CTA / 次要操作 / 危险操作 | primary / secondary / ghost / danger | default / hover / active / disabled / loading |
| `Input` | 单行文本输入 | default / with-icon / with-addon | default / focus / error / disabled |
| `Select` | 单选下拉 | | |
| `Modal` | 阻断式确认或编辑 | sm / md / lg / fullscreen | |
| `Toast` | 非阻断反馈 | info / success / warning / danger | |
| `Table` | 结构化数据展示 | | |
| `Empty` | 空状态 | | |
| `Skeleton` | 加载占位 | | |
| ... | | | |

## 交互模式（重要）

每个模式定义：触发条件 + 视觉表现 + 用什么组件 + 反例。

### Loading

- 短任务（<300ms）：不显示任何反馈。
- 中任务（300ms–2s）：组件原地 loading（按钮 spinner、表格 skeleton）。
- 长任务（>2s）：进度条或可取消 modal。
- **反例**：全屏 spinner 用于短任务。

### Empty

- 首次空：插画 + 主 CTA 引导第一次行为。
- 过滤后空：提示当前筛选 + "清除筛选" 链接。
- 无权限空：提示原因 + 申请入口或返回链接。

### Error

- 字段级：紧贴输入下方红字 + 红边框。
- 表单级：表单顶部错误条，列出可点击锚点。
- 页面级：错误页 + 错误码 + 重试按钮 + 反馈入口。
- 网络级：toast + 自动重试策略提示。

### Success / Confirmation

- 轻量：toast 2–4s 自动消失。
- 重要不可逆操作：modal 二次确认，CTA 用 danger 色。

### Navigation

- 顶部主导航 + 侧边次导航 + 面包屑的使用边界。
- 深层路径用面包屑，扁平结构不用。

### Form

- 必填标记位置、行内校验时机（blur vs change）、提交按钮位置、loading 状态。

### Accessibility

- 颜色对比度 ≥ AA。
- 所有交互可键盘操作。
- 所有图标按钮必有 aria-label。

## 命名与文件

- 组件目录：`components/ui/`（无业务）+ `components/blocks/`（带业务）。
- 组件文件 PascalCase，story 同名 `.stories.tsx`。

## 变更规则

- 新增组件必须先进本文件再写代码。
- token 变更必须列影响范围。
- 弃用组件标 `@deprecated` + 替代项 + 期限。
```

---

## AGENTS.md

```md
# Agent Rules

- 大改前先读 `README.md`、`ARCHITECTURE.md`、`DEVELOPMENT.md`、`BRANCHING.md`、`CONSTITUTION.md`。
- 进入项目先读 `docs/memory-bank/active-context.md` + `docs/memory-bank/patterns.md`。
- UI 任务还必须读 `DESIGN.md` 与对应 layout spec。
- 每个正式任务必须关联 `docs/design/active/*` 与 `docs/requirements/*`。
- 业务需求由用户提供，agent 不编造业务意图。
- 没有 spec，不新增架构层、状态机、存储、权限、全局依赖或公共 API。
- 如果实现必须偏离 spec，先更新 spec，再改代码。
- 触动 `CONSTITUTION.md` 红线的改动 → 立刻停下，提示用户而非自行突破。
- 会话结束更新 `docs/memory-bank/active-context.md`。
- 收尾必须说明变更文件、已运行验证、剩余风险。
```

---

## CONSTITUTION.md

> **项目红线**——不可妥协的规则。代码、PR、agent 行为如果触线，**直接拒绝**，不讨论。
>
> Constitution **只放红线**。一般技术品味、风格偏好放 `DEVELOPMENT.md` 或 lint 配置。判断标准：违反这条会导致**安全问题、数据丢失、合规违反、架构腐烂或无法回滚**——才是红线。

```md
# Project Constitution

- Status: active
- Last updated: <date>
- Owner: <tech lead / team>

> 红线规则。修改本文件 = 修改项目契约，需团队共识 + PR review。

## 0. 修改本文件的规则

- 新增/修改条款必须开 PR，至少 2 名维护者批准。
- 删除条款必须写明原因，并入 changelog。
- 临时豁免必须有明确截止时间和 owner。

## 1. 安全与合规

- [必须] 任何 secret 不入 repo（连 `.env.example` 也不能放真值）。
- [必须] 用户数据访问必须有审计日志。
- [禁止] 在日志/异常/前端 console 打印 PII / 凭证 / token。
- [禁止] 绕过鉴权层直连数据库或外部数据源。

## 2. 架构边界

- [必须] router/controller 只做协议适配，不承载领域逻辑。
- [必须] 全局真相源唯一：`ARCHITECTURE.md` / `BRANCHING.md` / `DESIGN.md` / `folder-declaration-v0.md`。
- [禁止] 跳过 service/repository 层，从 UI 直连存储。
- [禁止] 在 `packages/protocol`（或等价契约层）写业务逻辑。

## 3. 数据与状态

- [必须] runtime data（output / logs / uploads / runtime）放 repo 外。
- [必须] schema 变更走 migration，不直接改库。
- [禁止] 把外部平台当产品真相源，除非 ARCHITECTURE.md 明确声明。

## 4. 代码与变更

- [必须] 一个分支一个意图（feature / fix / refactor / docs，不混合）。
- [必须] 涉及架构 / 状态机 / 存储 / 权限 / 公共 API / UI 模式的改动，先更新 spec 再写代码。
- [必须] PR 关联 design doc 与 requirements doc（hotfix 例外，需补 post-mortem）。
- [禁止] 直接 push 到 protected 分支（main / uat / dev）。
- [禁止] 提交未验证的 AI 生成代码。

## 5. UI（UI 类项目）

- [必须] 新组件先入 `DESIGN.md` 再写代码。
- [必须] 所有交互状态（loading / empty / error / success）按 `DESIGN.md` 模式实现。
- [禁止] 局部发明设计 token 或组件样式，绕过 design system。

## 6. AI / Agent（AI 类项目）

- [必须] LLM 调用走统一 client，禁止直连 SDK。
- [必须] prompt 从 `prompts/` 加载，不内联硬编码。
- [必须] 所有 LLM 调用有 trace（provider / model / cost / latency / token）。
- [禁止] 把模型/provider 名硬编码到业务逻辑里（必须可配置）。
- [禁止] 没有 eval baseline 就上生产。

## 7. 可观测性

- [必须] 关键路径有结构化日志（含 request_id / user_id / latency）。
- [必须] 错误必须上报到错误追踪系统。
- [禁止] 用 `print` / `console.log` 作为生产日志。

## 8. 红线触发处理

agent / 开发者发现自己的改动**会**触线时：

1. 立即停下，不要"绕路实现"。
2. 在 PR / 对话中明示"触发红线 §X.Y"。
3. 给出三种路径：(a) 改方案规避，(b) 提议放宽该红线（走修改本文件流程），(c) 申请临时豁免。
4. 由 owner 决策。

## 9. 红线之外的规范

- 代码风格 / 命名 / 文件组织 / 一般最佳实践 → 见 `DEVELOPMENT.md` 与 lint/format 配置。
- 项目特有写法（"我们这里习惯怎么做"）→ 见 `docs/memory-bank/patterns.md`。
```

---

## ADR（架构决策记录）

> 路径：`docs/decisions/ADR-<NNNN>-<kebab-title>.md`，编号连续。
>
> 何时写 ADR：选了某个**有后果、未来要回溯**的技术方向（选库、改边界、引入新模式、放弃某条路）。决策本身写在 ARCHITECTURE.md，**为什么这么选**写在 ADR。

第一份 ADR 建议是 `ADR-0001-record-architecture-decisions.md`，宣布"本项目用 ADR 记录架构决策"。

```md
# ADR-<NNNN>: <一句话决策标题>

- Status: <proposed | accepted | superseded by ADR-MMMM | deprecated>
- Date: <YYYY-MM-DD>
- Deciders: <names / roles>
- Linked Design Doc: <docs/design/active/...>
- Linked ARCHITECTURE 行: <Technical Baseline 中的对应维度>

## Context（上下文）

<决策时面临什么问题？什么约束？什么不能改？>

## Options Considered（候选方案）

### 选项 A：<方案名>
- 优点：
- 缺点：
- 代价：
- 退出成本：

### 选项 B：<方案名>
- 优点：
- 缺点：
- 代价：
- 退出成本：

### 选项 C：<方案名>（如有）
- ...

## Decision（决策）

选择 **<方案 X>**。

## Rationale（理由）

<为什么是 X，不是 A/B/C？关键 trade-off 是什么？>

## Consequences（后果）

正面：
- <例：未来 6 个月内不会撞到性能墙>

负面：
- <例：团队需要学习 X 框架，初期 velocity 下降>

中性：
- <例：从此 X 维度不可在不更新本 ADR 的情况下变更>

## Validation（如何验证决策正确）

- <例：3 个月后重看：QPS 是否达到 P0 指标？>
- <例：上线 1 个月内若错误率 > 0.5%，触发 ADR 重审>

## References

- <相关链接、benchmark、社区讨论>
```

> 标记 `Status: superseded by ADR-MMMM`，保留旧文件，不删除。这是历史。

---

## Requirements Doc（用户提供）

> **这是用户输入**。Agent 的角色：协助结构化、校验完备性、提问澄清。**不替用户编造业务**。

```md
# Requirements: <Product/Feature> v0.0.1

- Status: <draft | reviewed | locked>
- Owner: <用户名>
- Last updated: <date>

## 业务背景

<为什么做、当前问题、机会。由用户填写。>

## 目标用户与场景

| 用户类型 | 核心场景 | 频率 | 关键痛点 |
|---|---|---|---|
| | | | |

## 业务目标

- 一句话目标：
- 可量化的成功指标（KPI / OKR）：
  - <指标 1：当前值 → 目标值 → 截止时间>

## P0 功能列表

按优先级编号，每条给：用户价值 + 验收标准。

1. **<功能名>**
   - 用户价值：
   - 验收标准：
2. ...

## P1 / Later

## 非目标（明确不做）

- <这一版不做什么、为什么>

## 约束

- 合规：<GDPR / 等保 / HIPAA / 行业规则>
- 性能：<延迟、并发、可用性 SLA>
- 预算：<云成本上限、人力上限>
- 时间：<里程碑日期>
- 技术约束：<必须用 X、不能用 Y>

## 已知风险

## Open Questions（用户回答）

- [ ] Q1:
- [ ] Q2:
```

---

## Design Doc

> **生命周期**：design doc 有明确状态，并通过目录位置体现：
>
> | Status | 含义 | 文件位置 |
> |---|---|---|
> | `draft` | 起草中，未启动 | `docs/design/backlog/` |
> | `active` | 当前在做 | `docs/design/active/` |
> | `done` | 已完成、已上线 | `docs/design/done/` |
> | `archived` | 放弃 / 被 superseded | `docs/design/done/`（文件名加 `-archived` 后缀） |
> | `superseded by <slug>` | 被新 doc 替代 | 留在原位置，header 注明替代者 |
>
> **转换规则**：
> - `draft → active`：用户/owner 决定开做，移动到 `active/`，写明 owner 与 milestone 起始日期。
> - `active → done`：feature 上线 + 验证通过 + memory-bank 已更新 → 移动到 `done/`，加 `Completion date` 行。
> - 不要原地改 status 不动文件：目录位置就是状态。
>
> **AGENTS.md 默认只读 `active/`**；查 backlog/done 需用户明确说"看一下 backlog / 历史"。

```md
# Design Doc: <Topic> v0.0.1

- Status: <draft | active | done | archived | superseded by <slug>>
- Owner: <name or team>
- Last updated: <date>
- Started: <YYYY-MM-DD>（active 时填）
- Completion date: <YYYY-MM-DD>（done 时填）
- Linked Requirements: docs/requirements/...
- Linked Layout: docs/design/layout-spec-<page>.md（UI 类）
- Linked ADR(s): docs/decisions/ADR-NNNN-...（如有架构决策）

## Background

## Goal

## Scope

## Non-goals

## User Flow / System Flow

## Data Model / State Changes

## API / CLI / UI Changes

## Milestones

## Risks and Rollback

## Acceptance Criteria

## Open Questions

## Validation Results（done 时回填）

<上线后实际的验证情况：哪些 acceptance criteria 通过，哪些被调整，遗留风险>
```

---

## Layout Spec（页面布局，md 形式）

> **Stage 4 第 5 步**：在写代码和拉 Figma 之前先用 md 把布局定下来。同时给 ASCII 框图（人看）和嵌套列表（agent / diff 用）。

```md
# Layout Spec: <Page Name>

- Status: draft
- Linked Requirements: docs/requirements/...
- Linked Design: ../../DESIGN.md
- Last updated: <date>

## 页面意图

<一句话：这个页面让什么用户在什么场景完成什么任务。>

## 关键 KPI

<这个页面要驱动的可量化结果。>

## 视觉框图（ASCII，桌面优先）

```
┌────────────────────────────────────────────────────┐
│ [Logo]   Nav1  Nav2  Nav3              [User] [⚙] │  ← Header (h=56)
├──────────┬─────────────────────────────────────────┤
│          │  Breadcrumb > Page Title          [CTA] │  ← PageHeader
│  Side    ├─────────────────────────────────────────┤
│  Nav     │  ┌──────────┐  ┌──────────┐             │
│          │  │ Filter   │  │ Search   │  [Refresh]  │  ← Toolbar
│  (w=240) │  └──────────┘  └──────────┘             │
│          ├─────────────────────────────────────────┤
│          │                                         │
│          │              Main Table                 │  ← Content
│          │              （空态：Empty 组件）       │
│          │                                         │
│          ├─────────────────────────────────────────┤
│          │           Pagination          [Export]  │  ← Footer bar
└──────────┴─────────────────────────────────────────┘
```

## 区域结构（嵌套列表，结构化）

- Header
  - Logo（点击回首页）
  - PrimaryNav（来源：路由配置）
  - UserMenu
  - SettingsButton
- Sidebar
  - SectionA
    - Item1
    - Item2
  - SectionB
- Main
  - PageHeader
    - Breadcrumb
    - Title
    - PrimaryCTA（"新建 X"）
  - Toolbar
    - FilterDropdown × N
    - SearchInput
    - RefreshButton
  - Content
    - Default: DataTable
    - Empty: EmptyState（来自 DESIGN.md → Empty 模式）
    - Loading: TableSkeleton
    - Error: ErrorBlock
  - FooterBar
    - Pagination
    - ExportButton

## 区域 → 组件映射（对照 DESIGN.md）

| 区域 | 组件 | 变体 | 备注 |
|---|---|---|---|
| Header | `AppHeader` | default | |
| PrimaryCTA | `Button` | primary | 图标 + 文案 |
| FilterDropdown | `Select` | with-icon | 多选 |
| SearchInput | `Input` | with-icon | 防抖 300ms |
| DataTable | `Table` | default | 排序、勾选 |
| EmptyState | `Empty` | first-time | 引导文案与 CTA |
| TableSkeleton | `Skeleton` | rows=10 | |
| ErrorBlock | `ErrorBlock` | inline | 重试按钮 |

## 状态矩阵

| 状态 | 触发 | 主区域表现 | 周边表现 |
|---|---|---|---|
| 初次空 | 用户从未创建 | `Empty` first-time | CTA 高亮 |
| 过滤空 | 筛选无结果 | `Empty` filtered + 清除筛选 | 保留 toolbar |
| 加载 | 请求中 | `TableSkeleton` | toolbar 禁用 |
| 错误 | 请求失败 | `ErrorBlock` + 重试 | toolbar 可用 |
| 部分加载 | 分页中 | 表格 + 底部 spinner | |

## 响应式断点

| 断点 | 行为 |
|---|---|
| ≥ 1280 | 三栏（侧栏常驻） |
| 768–1279 | 侧栏收起为抽屉 |
| < 768 | 单栏，toolbar 折叠为汉堡 |

## 交互细节

- 搜索：blur 提交 + 回车提交 + 防抖；URL query 同步。
- 排序：列头点击；多列排序在 P1。
- 选中：行勾选 → 顶部出现批量操作条。

## 可访问性

- 表头使用 `<th scope="col">`；行操作有 aria-label。
- 所有图标按钮带 aria-label；颜色对比度 ≥ AA。

## Open Questions

- [ ]
```

---

## Folder Declaration

```md
# Folder Declaration v0

## 顶层目录职责

- `apps/`: 用户可运行的应用
- `services/`: 后端服务
- `packages/`: 共享代码
- `agent/`: 默认 agent runtime，如适用
- `skills/`: 可复用 agent skills，如适用
- `docs/`: 文档
- `scripts/`: 开发和运维脚本
- `configs/`: 仅存配置模板

## 运行时目录：禁止提交

- `output/`
- `_reference_repo/`
- `node_modules/`
- `.venv/`
- `dist/`
- `build/`
- `runtime/`
- `logs/`
- `uploads/`

## 用户级目录

- `~/.<product>/data/`
- `~/.<product>/runtime/`
- `~/.<product>/logs/`

## 变更规则

- 新增顶层目录必须更新本文件。
- 重命名顶层目录必须先写 design doc。
```

---

## PR Body

```md
## Why

## What Changed

## Linked Spec

- Requirements: docs/requirements/...
- Design Doc: docs/design/...
- Layout Spec: docs/design/layout-spec-...（UI 改动）

## Validation

## Risks / Rollback

## Docs Updated
```
