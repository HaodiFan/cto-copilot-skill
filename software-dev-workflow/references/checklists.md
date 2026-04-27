# Checklists

用于验证、PR readiness 和反模式检查。

## 通用验证

优先运行最小相关验证。

```bash
git diff --check
```

前端：

```bash
pnpm typecheck
pnpm build
pnpm test
```

Python 后端：

```bash
ruff check .
pytest
alembic upgrade head
```

桌面端：

```bash
pnpm --dir apps/desktop typecheck
pnpm --dir apps/desktop build
```

Python Agent / CLI：

```bash
ruff check .
pytest
python -m <package> --help
```

Library / SDK：

```bash
pytest
npm test
npm run build
```

如果命令不可用，报告：

- 尝试或跳过的命令
- 为什么无法运行
- 不运行造成的风险
- 建议后续动作

---

## 需求文档完备性 Checklist（Stage 1 用）

> Agent 收到用户给的需求文档后，按这套清单校验。**全过才进 Stage 2**。

### 必须项（任何一项缺失 → 退回用户补）

- [ ] 业务背景：能解释"为什么做"
- [ ] 目标用户：至少 1 类用户的场景与痛点
- [ ] 业务目标：1 句话目标 + 至少 1 个可量化成功指标
- [ ] P0 功能列表：每条带验收标准
- [ ] 非目标：明确这一版不做什么
- [ ] 关键约束：合规 / 性能 / 时间 / 预算 至少一项明确
- [ ] Owner：可对接的负责人

### 应有项（缺失但可推迟）

- [ ] P1 / Later 列表
- [ ] 已知风险
- [ ] 与现有系统的关系

### 危险信号（命中任一 → 立即提问，不要往下做）

- [ ] 出现"差不多就行 / 你看着办 / 行业标准"等模糊词，且不肯具体化
- [ ] 验收标准全是主观词（好用、流畅、漂亮）
- [ ] P0 列表超过 10 条且无优先级
- [ ] 非目标为空
- [ ] 成功指标不可量化或无截止时间
- [ ] 性能/合规约束与所选架构明显冲突

### Agent 输出格式

```text
需求文档完备性: <PASS | NEEDS-USER-INPUT | BLOCKED>
缺失必须项: <逐条列出>
建议补充项: <逐条列出>
需用户回答的问题: <编号列表>
可继续推进的部分: <如可先做架构选型部分>
```

---

## 新项目 Checklist（Stage 4 完成判定）

- [ ] 已写项目一句话定义。
- [ ] 已选择项目形态。
- [ ] `README.md` 存在。
- [ ] `ARCHITECTURE.md` 存在，记录所有关键架构决策。
- [ ] `DEVELOPMENT.md` 存在。
- [ ] `BRANCHING.md` 存在并已生效（团队已知）。
- [ ] `DESIGN.md` 存在（UI 类项目必备）。
- [ ] `AGENTS.md` 存在。
- [ ] `docs/requirements/requirements-v0.0.1.md` 存在（来自用户）。
- [ ] `docs/design/design_doc-v0.0.1-bootstrap.md` 存在。
- [ ] `docs/design/layout-spec-<page>.md` 存在（UI 类项目首页或核心页）。
- [ ] `docs/governance/folder-declaration-v0.md` 存在。
- [ ] `docs/governance/changelog.md` 存在。
- [ ] `.env.example` 存在，真实 secret 已 ignore。
- [ ] bootstrap script 存在。
- [ ] 最小 app/service/CLI 可以运行。

---

## Feature Checklist

- [ ] 已有关联 design doc。
- [ ] 已有关联 requirements doc。
- [ ] Scope 和 non-goals 明确。
- [ ] 已识别影响目录。
- [ ] 数据、状态、API、UI 变化已写清。
- [ ] UI 改动：layout spec 已更新或新增。
- [ ] UI 改动：所选组件全部在 `DESIGN.md` 内；新组件已先入 `DESIGN.md`。
- [ ] 有测试或验证计划。
- [ ] 已判断是否需要 changelog。
- [ ] 实现已拆成 vertical slices。

---

## Bug Fix Checklist

- [ ] 已理解复现路径。
- [ ] 已识别 root cause 或明确假设。
- [ ] 可行时已添加 regression test。
- [ ] 修复足够小且局部。
- [ ] 用户可见行为变化已记录。

---

## Refactor Checklist

- [ ] 明确不改变行为。
- [ ] 已有测试或 smoke checks 覆盖被移动代码。
- [ ] 如果边界变化，已更新 architecture 或 folder declaration。
- [ ] 公共 API 保持兼容；不兼容时写 migration notes。

---

## PR Readiness

- [ ] Diff 聚焦一个 topic。
- [ ] 没有无关格式化噪音。
- [ ] 没有提交生成文件或运行时文件。
- [ ] Design doc、requirements doc、架构文档已同步。
- [ ] UI 改动：DESIGN.md / layout spec 已同步。
- [ ] 验证命令结果明确。
- [ ] 风险和回滚方式已说明。
- [ ] 分支与提交信息符合 `BRANCHING.md`。

---

## 常见反模式

- 行为不明确时先写代码、后补 spec。
- 多份互相竞争的真相源文档。
- agent 替用户编造业务需求，没有 Requirements Doc 就直接做 design doc。
- 跳过架构决策直接搭脚手架，导致后续选型被既成事实绑架。
- 分支规范延后到第一次 PR 才讨论，已有 commit 不符合规范。
- 写代码再补 layout，组件各自发明而不查 DESIGN.md。
- router/controller 堆核心业务逻辑。
- 前端页面各自发明局部设计系统。
- 前后端重复定义不兼容类型。
- 未明确决策就把外部平台当产品数据库。
- 把 runtime data 提交进 repo。
- 一个分支混合 feature、refactor、docs、依赖升级、格式化。
- AI 生成代码后不验证。
- PR 不写风险和验证方式。
- 接手项目立刻"重构整理"，破坏现有约定。

---

## "下一步做什么"输出 Checklist

当用户问下一步时，输出 7 字段模板（与 SKILL.md 统一）：

```text
当前阶段:
项目形态:
缺失内容:
下一步 3 个动作:
要创建/更新的文件:
验证门禁:
停止条件:
```
