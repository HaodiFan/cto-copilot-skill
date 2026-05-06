# 执行管线与 Review Pipeline

用于 Stage 5-8：把已经确认的需求、spec 和架构，推进到可验证、可审查、可发布、可沉淀的实现。它吸收两类经验：

- **硬 gate**：没有足够意图真相源时，不进入实现；没有验证证据时，不宣布完成。
- **角色化 review**：不同风险由不同视角检查，不把所有问题都压到普通代码 review。

本文件不替代 `stage-playbook.md`。Stage 判断仍在 `stage-playbook.md`；本文件回答“进入执行后怎么拆、怎么审、怎么验收”。

---

## 0. 总管线

```text
Requirements
  -> Design Doc / Spec
  -> Architecture / ADR / Constitution
  -> Implementation Plan
  -> Vertical Slice Implementation
  -> Review Pipeline
  -> QA / Validation
  -> PR / Release
  -> Learn / Memory / Lesson
```

默认顺序是单向的。只有 POC / Spike 和线上事故可以临时跳过部分治理，但必须记录原因，并在升级或收尾时补回证据。

---

## 1. 硬 Gate

| Gate | 进入条件 | 未满足时 |
|---|---|---|
| Requirements Gate | 用户已提供业务目标、P0、非目标、约束 | 退回 Stage 1，不编造需求 |
| Design Gate | Design doc 有 scope、flow、data/API/UI change、验收标准 | 退回 Stage 2，不直接实现 |
| Architecture Gate | 触动边界、状态机、权限、存储、公共 API、运行时依赖时，ARCHITECTURE/ADR 已同步 | 退回 Stage 3 |
| Plan Gate | 多步 feature 已有实施计划，切成 vertical slices | 退回 Stage 5 |
| Implementation Gate | 每个 slice 有明确验证命令或人工验收步骤 | 不开写或先补验证方案 |
| Review Gate | 命中的 review 角色已跑完，阻塞项关闭 | 不进入 PR readiness |
| Validation Gate | 后端/服务端自测优先，验证证据可复现 | 不宣布完成 |
| Learn Gate | 项目记忆和 skill 级 lesson 候选已处理 | 不结束长期项目会话 |

### 例外

- **单点小改**：纯文案、注释、单行配置、小型 bug 可跳过完整 Implementation Plan，但仍要说明验证。
- **POC / Spike**：走 `scenario-playbooks.md` §G 的轻量模式；命中升级触发条件后补正式 gate。
- **事故救火**：先恢复服务，再补 root cause、regression test、post-mortem 或 active-context。

---

## 2. Implementation Plan 质量标准

多步 feature 的 Stage 5 输出必须能让另一个 agent 或工程师按任务执行，不需要重建上下文。

### 必须包含

| 字段 | 要求 |
|---|---|
| Goal | 一句话说明交付的用户可观察行为 |
| Source docs | 关联 requirements、design doc、ADR、layout spec |
| Scope / Non-goals | 明确本轮做什么、不做什么 |
| Files map | 每个新建/修改文件的责任边界 |
| Vertical slices | 每个 slice 都能产生可观察行为 |
| Validation gate | 每个 slice 有后端/服务端自测命令、样本、契约测试、人工验收或 replay/eval |
| Review roles | 每个 slice 标出需要哪些 review 视角 |
| Rollback / fallback | 失败时如何关闭、回滚或降级 |
| Learn updates | 是否要更新 memory-bank、lessons、patterns |

### 反模式

- 只写“实现后端 / 实现前端 / 加测试”，没有文件和行为闭环。
- 写 “TODO / TBD / 处理边界情况 / 加适当错误处理”。
- 切片只按技术层拆，不能单独验证。
- 计划里没有后端/API/单元/集成测试命令、样本或人工验收路径。
- 多 agent 并行时没有 disjoint write set，导致互相覆盖。

### 建议格式

```md
# Implementation Plan: <Feature>

## Goal

## Source Docs

## Gates

| Gate | Status | Notes |
|---|---|---|

## File Map

| Path | Action | Responsibility |
|---|---|---|

## Slices

| Slice | Observable behavior | Files | Validation | Review roles |
|---|---|---|---|---|

## Rollback

## Learn Updates
```

---

## 3. Review Pipeline

Review 不等于只看代码。按变更类型选择必要角色；高风险变更跑完整 pipeline。

| Role | 何时必须跑 | 重点问题 | 证据 |
|---|---|---|---|
| Product / CEO Review | 新产品能力、范围变化、关键路径变化 | 目标是否真实、P0 是否过宽、非目标是否清楚 | requirements/design doc 更新 |
| Eng / Architecture Review | 数据、状态、权限、边界、依赖、部署变化 | owning layer 是否唯一、ADR 是否需要、退出成本 | ARCHITECTURE/ADR diff |
| Design Review | UI、交互、布局、视觉系统变化 | 是否符合 DESIGN/layout spec、响应式、可访问性 | 截图或 layout spec |
| DX Review | API、CLI、SDK、文档、集成体验 | 首次使用路径、错误信息、示例、向后兼容 | README/docs/API examples |
| Code Review | 所有非平凡代码变更 | 坏味道、行为风险、测试重点、secret/hardcode | `code-review-standards.md` 检查结果 |
| QA Review | 用户可见流程、API / service / RPA / OCR / AI pipeline | 端到端是否走通，失败态是否可见 | 后端自测、测试命令、样本、replay；浏览器证据仅显式触发 |
| Security / Compliance Review | 鉴权、secret、PII、支付、平台 ToS、爬取/自动化 | 最小权限、日志脱敏、授权边界 | threat notes / audit log |
| Release Review | PR、发布、迁移、回滚 | CI、changelog、回滚、迁移顺序 | PR body / release notes |

### 选择规则

- Web / 管理台：默认先跑后端/API/组件或已有自动化测试；只有用户明确要求、已有项目约定要求、或问题只能通过真实浏览器复现时，才跑浏览器模拟。
- RPA / 浏览器自动化：如果浏览器/设备动作是产品能力本身，可以用浏览器或设备自动化验证；否则不把浏览器模拟当默认测试手段。
- API / SDK / CLI：至少跑 Code + DX；契约变化加 Eng。
- 数据库 / 权限 / 状态机：至少跑 Eng + Code + QA；高风险加 Security。
- AI / OCR / CV / 数据分析：至少跑 Eng + QA；模型或 prompt 输出要有 eval/replay。
- 发布到真实用户：Release 必跑。

如果宿主支持多 agent 且用户允许，可以把不同 review 角色分给不同 agent；否则在同一会话按顺序跑。不要在未授权或写集冲突时强行并行。

---

## 4. 测试默认顺序与 Browser QA 触发边界

需要测试时，默认按这个顺序：

1. **后端 / 服务端自测优先**：unit、service、API、contract、migration、CLI、pipeline、fixture、replay/eval。
2. **项目已有自动化优先**：如果 repo 已有 e2e、component test、Playwright/Cypress，就按项目约定跑最小相关集。
3. **人工验收说明**：当自动化不足时，给出真人验收步骤和样本。
4. **浏览器模拟默认不跑**：除非用户明确要求，或任务本身就是浏览器/RPA/前端交互能力，或 bug 只能在真实浏览器复现。

换句话说：Web / 管理台 / 用户可见 UI 不能只靠编译通过，但默认先用后端/API/组件/已有测试证明行为。不要为了“看起来完整”主动开浏览器模拟。

### 触发 Browser QA 的条件

- 用户明确要求“打开页面 / 浏览器测试 / 截图 / 点一遍 / QA 这个 URL”。
- 当前任务的产品能力就是浏览器自动化、RPA、截图、爬取、页面可发现性、WebAgent 数据。
- bug 只在真实浏览器、登录态、viewport、console/network 或页面事件里可复现。
- release gate / 团队约定明确要求 browser smoke。

### 最小证据

- 访问目标 URL 或本地页面。
- 记录 viewport / 登录态 / 测试账号或脱敏身份。
- 走核心用户流：打开、填写、提交、刷新、错误态、权限态。
- 检查 console error、network failure、loading/empty/error state。
- 保存截图或明确说明为什么无法截图。
- 对移动或响应式页面，至少检查一个窄屏 viewport。

### RPA / 电商后台额外

- 记录平台、数据源类型、账号授权边界。
- 保留截图/HTML/raw payload 的证据层。
- 登录失效、验证码、限流、页面改版必须有失败态。
- 动作频率、random sleep、rate limit 和人工接管要写进 design doc。

---

## 5. Learn 边界

| 层级 | 文件 | 写什么 | 不写什么 |
|---|---|---|---|
| 项目级 Memory | `docs/memory-bank/patterns.md` / `active-context.md` | 该项目的约定、坑、当前焦点 | 跨项目方法论 |
| Skill L1 Lesson | `references/lessons.md` | 用户纠偏出的跨项目 knowhow | 单项目偏好、secret、客户名 |
| Skill L2 Pattern | `references/patterns-skill.md` | 多次验证后的通用模式 | 未验证猜测 |
| Reference Canon | `references/` 下的稳定文档 | 稳定方法论和默认选型 | 临时经验、单次纠偏 |

收尾时按这个顺序处理：

1. 项目状态变化 → 更新 `active-context.md`。
2. 项目专属经验 → 提议写 `docs/memory-bank/patterns.md`。
3. 跨项目纠偏 → 提议写 `references/lessons.md`。
4. 多次复用的 lesson → 提议 promote 到 `patterns-skill.md`。
5. 稳定 pattern 改变默认方法论 → 开 PR 升级 reference。

---

## 6. Stage 5-8 使用方式

| Stage | 使用本文件的方式 |
|---|---|
| Stage 5 Feature 规划 | 用 §2 输出 Implementation Plan，用 §3 标 review roles |
| Stage 6 实现 | 按 slice 执行；每个 slice 完成后跑对应 validation gate |
| Stage 7 验证 | 用 §3 和 §4 选择 QA / backend-first validation / security / DX 检查；Browser QA 仅显式触发 |
| Stage 8 PR/发布 | 用 §3 Release Review + §5 Learn 边界做最终收口 |

输出不要照搬本文件全文。只抽取当前任务命中的 gate、roles、QA 证据和 learn 更新。
