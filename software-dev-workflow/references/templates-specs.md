# 需求、设计与交付模板

用于创建 Requirements Doc、Design Doc、Layout Spec 和 PR Body。业务需求由用户/owner 提供；agent 只能结构化、校验、提问，不替用户编造业务意图。

## Requirements Doc（用户提供）

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

## 业务目标

- 一句话目标：
- 可量化的成功指标：
  - <指标 1：当前值 → 目标值 → 截止时间>

## P0 功能列表

1. **<功能名>**
   - 用户价值：
   - 验收标准：

## P1 / Later

## 非目标

## 约束

- 合规：
- 性能：
- 预算：
- 时间：
- 技术约束：

## 已知风险

## Open Questions（用户回答）

- [ ] Q1:
```

## Design Doc

生命周期用目录表达：

| Status | 文件位置 | 含义 |
|---|---|---|
| draft | `docs/design/backlog/` | 起草中，未启动 |
| active | `docs/design/active/` | 当前在做 |
| done | `docs/design/done/` | 已完成、已上线 |
| archived | `docs/design/done/` | 放弃或历史保留 |

`AGENTS.md` 默认只读 `active/`；查 backlog/done 需要用户明确要求。

```md
# Design Doc: <Topic> v0.0.1

- Status: <draft | active | done | archived | superseded by <design-doc-slug>>
  <!-- design doc 用文件 slug 引用（无连续编号）；ADR 用 ADR-MMMM。两者风格不同是有意为之。 -->
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

| Milestone | 行为闭环 | 影响文件 | Validation Gate |
|---|---|---|---|

## Risks and Rollback

## Acceptance Criteria

## Open Questions

## Validation Results（done 时回填）
```

## Layout Spec（页面布局，md 形式）

在写代码和拉 Figma 前先用 md 定布局。ASCII 图给人看，结构化列表给 agent / diff 用。

````md
# Layout Spec: <Page Name>

- Status: draft
- Linked Requirements: docs/requirements/...
- Linked Design: ../../DESIGN.md
- Last updated: <date>

## 页面意图

<一句话：这个页面让什么用户在什么场景完成什么任务。>

## 关键 KPI

## 视觉框图（ASCII，桌面优先）

```text
┌────────────────────────────────────────────────────┐
│ [Logo]   Nav1  Nav2  Nav3              [User] [⚙] │
├──────────┬─────────────────────────────────────────┤
│  Side    │  Breadcrumb > Page Title          [CTA] │
│  Nav     ├─────────────────────────────────────────┤
│          │  Filter  Search              [Refresh]  │
│          ├─────────────────────────────────────────┤
│          │              Main Content               │
└──────────┴─────────────────────────────────────────┘
```

## 区域结构

- Header
- Sidebar
- Main
  - PageHeader
  - Toolbar
  - Content
  - FooterBar

## 区域 → 组件映射

| 区域 | 组件 | 变体 | 备注 |
|---|---|---|---|

## 状态矩阵

| 状态 | 触发 | 主区域表现 | 周边表现 |
|---|---|---|---|
| 初次空 | | | |
| 过滤空 | | | |
| 加载 | | | |
| 错误 | | | |

## 响应式断点

| 断点 | 行为 |
|---|---|
| ≥ 1280 | |
| 768–1279 | |
| < 768 | |

## 交互细节

## 可访问性

## Open Questions
````

## PR Body

```md
## Why

## What Changed

## Linked Spec

- Requirements: docs/requirements/...
- Design Doc: docs/design/...
- Layout Spec: docs/design/layout-spec-...（UI 改动）
- ADR: docs/decisions/...

## Validation

## Risks / Rollback

## Docs Updated
```
