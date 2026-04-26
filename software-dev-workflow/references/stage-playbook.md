# 阶段 Playbook

用于诊断当前软件开发阶段，并决定下一步动作。

## Stage 0：想法

典型信号：

- 用户只有产品想法，没有 repo、spec 或技术形态。
- 请求类似"我想做 X"、"这个项目怎么开始"。

下一步：

- 写一句话定义：用户、系统类型、核心能力、核心问题、P0 非目标。
- 判断项目形态：Web+Backend、Desktop+Local Agent、Python Agent/CLI、Library/SDK、Monorepo。
- 创建 `docs/design/design_doc-v0.0.1-bootstrap.md`。

产物格式：

```text
<Project> 是一个面向 <user> 的 <system type>，
通过 <core capability> 解决 <core problem>，
P0 不做 <non-goal>。
```

停止条件：

- 已选定一个可逆的项目形态，并明确 P0 非目标。

## Stage 1：需求澄清

典型信号：

- 用户、权限、流程、数据来源、平台或成功标准不清楚。
- 用户提供了零散材料，但没有结构化需求。

下一步：

- 抽取 actors、workflow、data objects、integrations、constraints、risks。
- 区分 scope 和 non-scope。
- 把未知点转成 open questions。

最小产物：

- `Background`
- `Goals`
- `Scope`
- `Non-goals`
- `Assumptions`
- `Open questions`

停止条件：

- 下一份 design doc 可以在不编造产品意图的前提下写出来。

## Stage 2：Spec

典型信号：

- 用户要求 PRD、design doc、spec。
- 功能会改变 workflow、state、data、API、UI pattern 或 integration。

下一步：

- 创建或更新一份聚焦的 design doc。
- 必须包含验收标准和回滚点。
- 不写"大而全总集"，除非用户明确要项目 baseline。

最小 design doc：

```md
# Design Doc: <Topic> v0.0.1

## Background
## Goal
## Scope
## Non-goals
## User Flow / System Flow
## Architecture / Data Model / API / UI Changes
## Milestones
## Risks and Rollback
## Acceptance Criteria
## Open Questions
```

停止条件：

- 开发者能实现最小切片，不需要猜行为。

## Stage 3：架构

典型信号：

- 需要选择技术栈、repo 形态、runtime 边界、数据归属、模块边界或集成模式。
- 用户问"怎么拆"、"放哪里"、"选什么结构"。

下一步：

- 定义真相源：数据库、本地文件系统、外部平台或 package API。
- 定义系统边界：frontend、backend、worker、local runtime、CLI、external services。
- 明确 P0 不做什么。
- 更新 `ARCHITECTURE.md` 或等价全局真相源。

规则：

- 外部平台默认是 integration 或 replica，不是真相源，除非文档明确说明。
- router/controller 只做协议适配，不承载核心领域逻辑。
- runtime data 放 repo 外。

停止条件：

- 每个主要职责都有唯一 owning layer 或目录。

## Stage 4：脚手架

典型信号：

- 空 repo、新模块、首次提交。
- 用户要求 starter structure 或 bootstrap plan。

下一步：

- 用 `project-blueprints.md` 选择 starter tree。
- 先创建根级文档：`README.md`、`ARCHITECTURE.md`、`DEVELOPMENT.md`、`AGENTS.md`。
- 添加 `.env.example`、config template、bootstrap script、folder declaration。

首次提交规则：

- 首次提交只放 skeleton、docs、config templates、最小可运行 hello path。
- 不把正式 feature 实现混进 bootstrap commit，除非不可避免。

停止条件：

- 新成员可以 clone、install、run，并找到下一份 design doc。

## Stage 5：Feature 规划

典型信号：

- 功能目标明确，但不知道怎么拆。
- 用户要求拆解、估算、多人/多 agent 并行。

下一步：

- 确认关联 design doc。
- 拆成能产生可观察行为的 vertical slices。
- 标出影响目录、数据模型变化、API 变化、UI 变化、测试、文档。

产物格式：

```text
Milestone 1: schema/API skeleton
Milestone 2: core use case
Milestone 3: UI or CLI path
Milestone 4: tests and docs
```

停止条件：

- 每个 milestone 都有清晰 validation gate。

## Stage 6：实现

典型信号：

- 用户要求 build、fix、add、wire、migrate、implement。

下一步：

- 编辑前先检查 repo。
- 读取 canonical docs 和现有实现模式。
- 实现最小切片。
- 保持在已声明架构边界内。
- 如需偏离 spec，先更新 spec。

收尾必须包含：

- 变更文件
- 已运行验证
- 未运行检查及原因
- 剩余风险

停止条件：

- 代码已实现，并至少完成最小验证，或明确说明验证被什么阻塞。

## Stage 7：验证

典型信号：

- 用户要求 tests、QA、hardening、e2e、prelaunch、smoke test、"是否 ready"。

下一步：

- 把变更映射到检查：typecheck、unit、integration、migration、e2e、visual、smoke。
- 先跑最窄可靠检查。
- bug fix 优先补 regression test。
- 记录验证缺口。

停止条件：

- 风险列表已关闭，或清楚记录在 PR/文档里。

## Stage 8：PR / 发布

典型信号：

- 用户要求 commit、push、open PR、ship、release、deploy。

下一步：

- 确保 design doc 已关联。
- 重要变更已更新 docs/changelog。
- `git diff --check` 通过。
- 总结 why、what、risk、validation。

PR body：

```md
## Why
## What Changed
## Linked Spec
## Validation
## Risks / Rollback
## Docs Updated
```

停止条件：

- Reviewer 不需要重建上下文，就能理解意图、行为变化和风险。

## Stage 9：维护 / 重构

典型信号：

- 用户要求 cleanup、modularize、rename、reduce debt、improve structure、refactor。

下一步：

- 区分行为保持型 refactor 和功能变更。
- 移动代码前先识别或补充测试。
- 若边界变化，更新 folder declaration 或 architecture docs。
- 没有目标架构文档时，避免大范围重写。

停止条件：

- 行为保持，边界更清晰，并有验证证明 touched slice 没有回归。
