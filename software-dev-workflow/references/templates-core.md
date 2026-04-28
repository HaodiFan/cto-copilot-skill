# 核心项目文档模板

用于创建根级项目文档：`README.md`、`ARCHITECTURE.md`、`DEVELOPMENT.md`、`BRANCHING.md`、`DESIGN.md`、`AGENTS.md`。

只在需要生成对应文件时读取本文件。模板里的占位符保留给 agent 按项目事实填写；不知道的内容标 `TODO（需用户/owner 确认）`。

## README.md

````md
# <Project>

<一句话项目定义：面向谁、解决什么、当前核心能力。>

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

| 维度 | 选定方案 | 主要理由 | 主要代价 | 退出成本 | ADR |
|---|---|---|---|---|---|
| 项目形态 | | | | | |
| Repo 组织 | | | | | |
| 客户端形态 | | | | | |
| 前端渲染 | | | | | |
| 前后端关系 | | | | | |
| 后端架构 | | | | | |
| 数据架构 | | | | | |
| 通信协议 | | | | | |
| 鉴权与身份 | | | | | |
| 部署环境 | | | | | |
| 异步任务 | | | | | |
| 可观测性 | | | | | |
| 测试策略 | | | | | |
| CI/CD | | | | | |
| 配置与密钥 | | | | | |

## Data Truth Source

| 数据/状态 | 真相源 | 写入者 | 读取者 |
|---|---|---|---|

## Key Flows

## Risks and Future Phases
```

## DEVELOPMENT.md

````md
# Development Rules

## Spec-driven Development

- 业务需求由用户/owner 提供，agent 不编造。
- 改动影响架构、状态机、存储、权限、公共 API 或 UI 模式时，先更新 design doc / ADR。
- 每个任务只做一个 topic。

## Local Setup

```bash
<bootstrap command>
```

## Testing Rules

- 最小相关验证优先。
- 无法运行检查时，说明原因、风险和后续动作。

## Definition of Done

- 关联 requirements / design doc。
- 文档、测试、changelog 按影响范围同步。
- `CONSTITUTION.md` 红线 0 触发，或已走豁免流程。
````

## BRANCHING.md

```md
# Branching & Commit Rules

## 策略

<GitHub flow | trunk-based | git-flow>

## 长期分支

- `main`: production baseline
- `uat`: acceptance / release verification（如适用）
- `dev`: daily integration（如适用）

## 短期分支命名

- `feature/<topic>`
- `fix/<topic>`
- `docs/<topic>`
- `refactor/<topic>`
- `codex/<type>/<topic>`（AI agent 分支）

## 一个分支一个意图

不要混合 feature、refactor、docs、依赖升级和格式化。

## Commit Rules

优先 Conventional Commits：

- `feat:`
- `fix:`
- `docs:`
- `refactor:`
- `test:`
- `chore:`

## PR 规则

- PR body 必须包含 why / what / validation / risk。
- 触发架构维度变更时关联 ADR。
```

## DESIGN.md

```md
# Design System

- Status: active
- Last updated: <date>

## 设计原则

## Design Tokens

### Color

### Typography

### Spacing

### Radius / Shadow / Motion

## 组件清单

| Component | Variants | Usage | Notes |
|---|---|---|---|

## 交互模式

### Loading
### Empty
### Error
### Success / Confirmation
### Navigation
### Form
### Accessibility

## 命名与文件

## 变更规则

- 新组件先进本文件再写代码。
- token 变更必须列影响范围。
- 弃用组件标 `@deprecated` + 替代项 + 期限。
```

## AGENTS.md

```md
# Agent Rules

- 大改前先读 `README.md`、`ARCHITECTURE.md`、`DEVELOPMENT.md`、`BRANCHING.md`、`CONSTITUTION.md`。
- 开工先读 `docs/memory-bank/active-context.md` + `docs/memory-bank/patterns.md`。
- UI 任务还必须读 `DESIGN.md` 与对应 layout spec。
- 每个正式任务必须关联 `docs/design/active/*` 与 `docs/requirements/*`。
- 业务需求由用户提供，agent 不编造业务意图。
- 没有 spec，不新增架构层、状态机、存储、权限、全局依赖或公共 API。
- 如果实现必须偏离 spec，先更新 spec，再改代码。
- 触动 `CONSTITUTION.md` 红线的改动 → 立刻停下，提示用户而非自行突破。
- 会话结束更新 `docs/memory-bank/active-context.md`。
- 收尾必须说明变更文件、已运行验证、剩余风险。
```
