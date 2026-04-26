# Spec 模板

用于创建或修复项目文档。

## README.md

````md
# <Project>

<一句话项目定义。>

## Status

<pre-dev | prototype | active | uat | production>

## Start Here

- [Architecture](./ARCHITECTURE.md)
- [Development Rules](./DEVELOPMENT.md)
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

## Data Truth Source

## Key Flows

## Risks and Future Phases
```

## DEVELOPMENT.md

```md
# Development Rules

## Spec-driven Development

- 每个正式开发任务都必须关联 design doc。
- 如果实现偏离 design doc，先更新 design doc，再继续实现。
- 架构边界变化必须更新 `ARCHITECTURE.md`。
- 重要变更必须更新 `docs/governance/changelog.md`。

## Branch Rules

- `main`: 生产基线
- `uat`: 验收分支
- `dev`: 日常集成
- `<type>/<topic>` 或 `codex/<type>/<topic>`: 短期主题分支

## Commit Rules

使用 Conventional Commits：`feat`、`fix`、`refactor`、`docs`、`test`、`chore`。

## PR Rules

每个 PR 写清楚 why、what、linked spec、risk、validation、docs updated。

## Testing Rules

运行最小相关检查。如果无法运行，说明原因和风险。

## Definition of Done

- 已关联 design doc
- 代码落在正确架构边界内
- 已运行验证或明确阻塞原因
- 必要文档/changelog 已更新
```

## AGENTS.md

```md
# Agent Rules

- 大改前先读 `README.md`、`ARCHITECTURE.md`、`DEVELOPMENT.md`。
- UI 任务还必须读 `DESIGN.md` 或前端设计规则。
- 每个正式任务必须关联 `docs/design/*`。
- 没有 spec，不新增架构层、状态机、存储、权限、全局依赖或公共 API。
- 如果实现必须偏离 spec，先更新 spec，再改代码。
- 收尾必须说明变更文件、已运行验证、剩余风险。
```

## Design Doc

```md
# Design Doc: <Topic> v0.0.1

- Status: draft
- Owner: <name or team>
- Last updated: <date>

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

## PR Body

```md
## Why

## What Changed

## Linked Spec

## Validation

## Risks / Rollback

## Docs Updated
```
