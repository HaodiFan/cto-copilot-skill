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

## 新项目 Checklist

- 已写项目一句话定义。
- 已选择项目形态。
- `README.md` 存在。
- `ARCHITECTURE.md` 存在。
- `DEVELOPMENT.md` 存在。
- `AGENTS.md` 存在。
- `docs/design/design_doc-v0.0.1-bootstrap.md` 存在。
- `docs/governance/folder-declaration-v0.md` 存在。
- `docs/governance/changelog.md` 存在。
- `.env.example` 存在，真实 secret 已 ignore。
- bootstrap script 存在。
- 最小 app/service/CLI 可以运行。

## Feature Checklist

- 已有关联 design doc。
- Scope 和 non-goals 明确。
- 已识别影响目录。
- 数据、状态、API、UI 变化已写清。
- 有测试或验证计划。
- 已判断是否需要 changelog。
- 实现已拆成 vertical slices。

## Bug Fix Checklist

- 已理解复现路径。
- 已识别 root cause 或明确假设。
- 可行时已添加 regression test。
- 修复足够小且局部。
- 用户可见行为变化已记录。

## Refactor Checklist

- 明确不改变行为。
- 已有测试或 smoke checks 覆盖被移动代码。
- 如果边界变化，已更新 architecture 或 folder declaration。
- 公共 API 保持兼容；不兼容时写 migration notes。

## PR Readiness

- Diff 聚焦一个 topic。
- 没有无关格式化噪音。
- 没有提交生成文件或运行时文件。
- Design doc 和架构文档已同步。
- 验证命令结果明确。
- 风险和回滚方式已说明。

## 常见反模式

- 行为不明确时先写代码、后补 spec。
- 多份互相竞争的真相源文档。
- router/controller 堆核心业务逻辑。
- 前端页面各自发明局部设计系统。
- 前后端重复定义不兼容类型。
- 未明确决策就把外部平台当产品数据库。
- 把 runtime data 提交进 repo。
- 一个分支混合 feature、refactor、docs、依赖升级、格式化。
- AI 生成代码后不验证。
- PR 不写风险和验证方式。

## "下一步做什么"输出 Checklist

当用户问下一步时，包含：

```text
当前阶段:
项目形态:
缺失内容:
下一步 3 个动作:
要创建/更新的文件:
验证门禁:
停止条件:
```
