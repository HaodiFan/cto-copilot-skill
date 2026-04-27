# Software Dev Workflow Skill

中文软件开发工作流 Skill，按"项目形态 / 当前阶段 / 是否接手"判断下一步该做什么。基于 spec-driven 开发与软件工程最佳实践。

## Status

- Version: 0.2.0
- Stage: active

## Skill 内容

- Skill 目录：`software-dev-workflow/`
- 入口：`software-dev-workflow/SKILL.md`
- References：
  - `references/stage-playbook.md` —— Stage 0–9 阶段判断与下一步动作（Stage I「接手盘点」单独成文）
  - `references/project-blueprints.md` —— 5 种项目形态的 starter 目录与首次提交清单
  - `references/architecture-cases.md` —— 通用架构选型 case 库（19 大类，深度档：定位 / 适用 / 反例 / 代价 / 退出成本 / 决策信号 / worked example / 踩坑 / 决策矩阵）
  - `references/architecture-cases-ai.md` —— AI / Agent 专项架构选型 case 库（11 大类，同样深度档）
  - `references/spec-templates.md` —— README、ARCHITECTURE、DEVELOPMENT、BRANCHING、DESIGN、AGENTS、Requirements Doc、Design Doc、Layout Spec、Folder Declaration、PR Body 模板
  - `references/checklists.md` —— 验证、需求完备性、新项目 / Feature / Bug fix / Refactor / PR Readiness、反模式
  - `references/inheriting-projects.md` —— 接手已有项目的盘点流程、现状报告模板、文档补齐顺序

## 主要能力

- 按"新项目 vs 接手项目"分流入口
- spec-driven 开发：业务需求由用户提供，agent 不编造
- 支持 Stage 0–4 的真实 6 步顺序：folders → branching → architecture → requirements → layout → components
- 19 大类通用架构决策 + 11 大类 AI 架构决策的可查 case 库
- 接手项目专用盘点流程，默认尊重现状
- 7 字段「下一步」统一输出格式

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
