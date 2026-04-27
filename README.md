# Software Dev Workflow Skill

中文软件开发工作流 Skill，按"项目形态 / 当前阶段 / 是否接手"判断下一步该做什么。基于 spec-driven 开发与软件工程最佳实践。

## Status

- Version: 0.3.0
- Stage: active

## Skill 内容

- Skill 目录：`software-dev-workflow/`
- 入口：`software-dev-workflow/SKILL.md`
- References：
  - `references/stage-playbook.md` —— Stage 0–9 阶段判断与下一步动作（Stage I「接手盘点」单独成文）
  - `references/project-blueprints.md` —— 5 种项目形态的 starter 目录与首次提交清单
  - `references/architecture-cases.md` —— 通用架构选型 case 库（19 大类，深度档：定位 / 适用 / 反例 / 代价 / 退出成本 / 决策信号 / worked example / 踩坑 / 决策矩阵）
  - `references/architecture-cases-ai.md` —— AI / Agent 专项架构选型 case 库（11 大类，同样深度档）
  - `references/spec-templates.md` —— README、ARCHITECTURE、DEVELOPMENT、BRANCHING、DESIGN、AGENTS、**CONSTITUTION（红线）**、**ADR（决策记录）**、Requirements Doc、Design Doc（含 backlog/active/done lifecycle）、Layout Spec、Folder Declaration、PR Body 模板
  - `references/checklists.md` —— 验证、需求完备性、新项目 / Feature / Bug fix / Refactor / PR Readiness、Constitution 红线触发处理、Memory Bank hygiene、反模式
  - `references/inheriting-projects.md` —— 接手已有项目的盘点流程（含 §1.0 自动识别）、现状报告模板、文档补齐顺序
  - `references/memory-bank-guide.md` —— AI agent 跨会话上下文的"指针 + 增量"模式（brief / tech-context / patterns / active-context）
  - `references/prompts-guide.md` —— 可复用 prompt 模板（scaffold / handover-audit / new-feature / new-design-doc / new-adr / pre-pr / update-active-context / refactor-safely / debug-incident）

## 主要能力

- 按"新项目 vs 接手项目"分流入口
- spec-driven 开发：业务需求由用户提供，agent 不编造
- 支持 Stage 0–4 的真实 8 步顺序：folders → branching → architecture+constitution+ADR → requirements+design lifecycle → layout → components → memory-bank+prompts → AGENTS.md
- 19 大类通用架构决策 + 11 大类 AI 架构决策的可查 case 库
- 项目红线（CONSTITUTION.md）+ 决策记录（ADR）+ 设计文档生命周期（backlog/active/done）三件套
- AI agent 跨会话上下文（Memory Bank）+ 可复用动作模板（Prompts）
- 接手项目专用盘点流程（含自动识别），默认尊重现状
- 7 字段「下一步」统一输出格式

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

或在对话中明确请求："使用 software-dev-workflow 评估我的项目阶段，告诉我下一步具体该做什么。"

## 适用场景

- 新建项目（从想法到首次可运行）
- 接手已有项目（盘点 + 路径选择）
- 已有项目加 feature
- review、救火、"下一步做什么"
- 写开发流程 / 项目规范 / 方法论
