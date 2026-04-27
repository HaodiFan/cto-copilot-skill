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
docs/
├── requirements/
│   └── requirements-v0.0.1.md         # 用户提供的需求文档
├── design/
│   ├── design_doc-v0.0.1-bootstrap.md
│   └── layout-spec-<page>.md          # 页面布局 md（UI 类项目）
└── governance/
    ├── folder-declaration-v0.md
    ├── terminology-glossary.md
    └── changelog.md
```

---

## README.md

````md
# <Project>

<一句话项目定义。>

## Status

<pre-dev | prototype | active | uat | production>

## Start Here

- [Architecture](./ARCHITECTURE.md)
- [Development Rules](./DEVELOPMENT.md)
- [Branching](./BRANCHING.md)
- [Design System](./DESIGN.md)
- [Requirements](./docs/requirements/)
- [Design Docs](./docs/design/)
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

- 大改前先读 `README.md`、`ARCHITECTURE.md`、`DEVELOPMENT.md`、`BRANCHING.md`。
- UI 任务还必须读 `DESIGN.md` 与对应 layout spec。
- 每个正式任务必须关联 `docs/design/*` 与 `docs/requirements/*`。
- 业务需求由用户提供，agent 不编造业务意图。
- 没有 spec，不新增架构层、状态机、存储、权限、全局依赖或公共 API。
- 如果实现必须偏离 spec，先更新 spec，再改代码。
- 收尾必须说明变更文件、已运行验证、剩余风险。
```

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

```md
# Design Doc: <Topic> v0.0.1

- Status: draft
- Owner: <name or team>
- Last updated: <date>
- Linked Requirements: docs/requirements/...
- Linked Layout: docs/design/layout-spec-<page>.md（UI 类）

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
