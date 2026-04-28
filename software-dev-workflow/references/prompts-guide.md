# Prompts 目录指南

为项目沉淀**可复用的高频动作模板**，让 agent 在不同会话中以相同方式触发同样的工作流。

> 与宿主无关：本指南只规定**目录约定与模板内容**，不依赖 Claude Code / Cursor / Codex 的斜杠命令机制。任何 agent 都可以"读取 + 应用"这些 prompt 模板。

## 设计原则

- **一个 prompt 一个意图**：拆细，不堆砌。
- **明确输入与产出**：每份 prompt 顶部声明"我需要什么 + 我交付什么"。
- **引用 references**：让 prompt 路由到本 Skill 的 reference，而非复制方法论。
- **对 agent 友好**：用结构化 markdown，避免长段散文。
- **与项目共生**：prompts 可被项目自定义/覆盖；本指南给的是基线模板。

## 目录约定

```
docs/prompts/
├── README.md                 # 索引：列出所有可用 prompt
├── scaffold-new-project.md   # 从 0 启动新项目
├── spike-start.md            # 启动 POC / Spike（轻量模式）
├── handover-audit.md         # 接手项目盘点
├── new-feature.md            # 已有项目加 feature
├── scenario-routing.md       # 命中业务场景时如何路由 scenario-playbooks
├── new-design-doc.md         # 创建 design doc
├── new-adr.md                # 记录架构决策
├── pre-pr.md                 # PR 前自检
├── update-active-context.md  # 会话末更新 memory bank
├── refactor-safely.md        # 行为保持型重构
└── debug-incident.md         # 救火 / 故障排查
```

> 路径 `docs/prompts/`，与 `docs/design/`、`docs/governance/`、`docs/memory-bank/` 平级。

## 通用 prompt 结构

每份 prompt 文件用统一结构：

```md
# Prompt: <动作名>

## 何时用
<用户请求满足什么条件时触发>

## 我需要的输入
- <字段 1>
- <字段 2>

## 我会交付
- <产物 1>
- <产物 2>

## 步骤
1.
2.
3.

## 引用的 references
- references/<file>.md
- references/<file>.md

## 输出格式
<7 字段模板 / 自由文本 / 文件 diff / 检查报告 ...>

## 完成判定
- [ ]
- [ ]
```

---

## 基线 prompt 模板

下面 11 份是建议默认 ship 的基线模板（v0.4.1 起新增 `spike-start` 与 `scenario-routing`，回流 v0.4 的 POC/Spike 与场景层）。项目可挑用、改写、新增。

### scaffold-new-project.md

```md
# Prompt: Scaffold New Project

## 何时用
- 用户从空目录或新 repo 开始
- 用户问"这个项目怎么搭"

## 我需要的输入
- 用户提供的需求文档（或同意我先生成 Requirements Doc 模板让用户填写）
- 项目形态候选（Web+Backend / Desktop+Local Agent / Python Agent/CLI / Library / Monorepo / Unknown）
- 业务场景信号（RPA/采集、OCR、视觉质检、数据治理、LLM 生产链路、POC 等，如有）
- 关键约束（语言、合规、时间、预算）

## 我会交付
- 形态判断（含理由与代价）
- 关键架构决策清单（Stage 3 输出）
- starter 目录树（Stage 4）
- 首次提交文件清单
- BRANCHING.md / Constitution.md / Memory Bank 初始版

## 步骤
1. 校验需求文档完备性（references/checklists.md）
2. 形态判断走 architecture-cases.md §0 的 4 步法
3. 如命中业务自动化场景，先读 scenario-playbooks.md，确定最小切片和验证门禁
4. 走 architecture-cases.md 20 大类 + AI 项目 architecture-cases-ai.md 11 大类，逐项确定方案
5. 落地 BRANCHING.md（决策方向：trunk-based / git-flow / GitHub flow）
6. 落地 Constitution.md（项目红线）
7. 落地 starter tree（references/project-blueprints.md）
8. 初始化 Memory Bank（references/memory-bank-guide.md）
9. 输出首次提交清单 + Stage 4 完成判定

## 引用的 references
- references/stage-playbook.md（Stage 0–4）
- references/scenario-playbooks.md（命中场景时）
- references/architecture-cases.md
- references/architecture-cases-ai.md（AI 项目）
- references/project-blueprints.md
- references/spec-templates.md（模板索引）
- references/templates-core.md / templates-governance.md / templates-specs.md（按需）
- references/memory-bank-guide.md
- references/checklists.md（需求完备性 + 新项目 checklist）

## 输出格式
7 字段「下一步」模板 + 文件清单

## 完成判定
- [ ] 需求完备性 PASS 或用户明确接受 NEEDS-INPUT
- [ ] ARCHITECTURE.md / BRANCHING.md / Constitution.md / DESIGN.md（UI 类）/ AGENTS.md 已就位
- [ ] Memory Bank 四件套有最小可用版本
- [ ] 首次提交清单符合 references/checklists.md「新项目 Checklist」
```

### handover-audit.md

```md
# Prompt: Handover Audit（接手项目盘点）

## 何时用
- 用户接手他人/团队的现有 repo
- 用户问"这项目怎么继续 / 该不该重构 / 先做什么"
- 用户给出已有 repo URL

## 我需要的输入
- repo 访问（路径或 URL）
- 业务负责人是否可达
- 用户的目标（接手后想干什么）

## 我会交付
- 自动识别报告（项目类型 + 工具链推断）
- 项目现状报告（references/inheriting-projects.md §2 模板）
- 路径建议（A 遵循 / B 稳定 / C 建文档 / D 重构）
- 第一周「做与不做」清单
- Memory Bank 四件套初始版（基于盘点结果）

## 步骤
1. 跑「自动识别清单」（references/inheriting-projects.md §1.0）
2. 6 块盘点 checklist 逐项过
3. 输出现状报告（标注「已确认 / 推断」）
4. 推荐路径并说明触发条件
5. 用现状结果初始化 Memory Bank

## 引用的 references
- references/inheriting-projects.md
- references/architecture-cases.md（校准 as-is 决策）
- references/memory-bank-guide.md

## 输出格式
现状报告 + 7 字段「下一步」模板

## 完成判定
- [ ] 现状报告完成
- [ ] 路径选定（A/B/C/D 之一）
- [ ] Memory Bank 初始化
- [ ] 第一周不做清单已沟通
```

### new-feature.md

```md
# Prompt: New Feature

## 何时用
- 已有项目要加新 feature / 页面 / API / workflow / agent 能力

## 我需要的输入
- feature 名称与一句话目标
- 关联的 requirements 与 design doc（若已有）
- 影响范围预估（前端 / 后端 / 数据 / 集成）

## 我会交付
- design doc（如缺失）
- vertical slice 拆解（milestones）
- 影响目录与文件清单
- 测试与 docs 计划
- Constitution 与 patterns.md 检查（不违反红线、走项目模式）

## 步骤
1. 校验关联 requirements 是否完备（checklists.md 完备性 checklist）
2. 若无 design doc，按 `spec-templates.md` → `templates-specs.md` 创建 v0.0.1
3. UI 类：layout spec → 组件选择查 DESIGN.md
4. 拆 vertical slices，标 validation gate
5. 检查 Constitution 红线 + patterns.md 模式
6. 更新 active-context.md

## 引用的 references
- references/stage-playbook.md（Stage 5）
- references/spec-templates.md（模板索引）
- references/templates-specs.md（Design Doc / PR Body）
- references/checklists.md（Feature Checklist）
- references/memory-bank-guide.md（patterns.md / active-context.md）

## 输出格式
7 字段「下一步」模板 + milestone 列表

## 完成判定
- [ ] 已有 design doc（status: active）
- [ ] vertical slice 切完，每个有 validation gate
- [ ] active-context.md 更新
```

### new-design-doc.md

```md
# Prompt: New Design Doc

## 何时用
- Stage 2，要建 PRD/design doc

## 步骤
1. 套 `spec-templates.md` → `templates-specs.md` 的 Design Doc 模板
2. 关联 requirements 与（UI 类）layout spec
3. 写验收标准 + 回滚点
4. Status 默认 `draft`，归档到 `docs/design/`

## 引用
- references/spec-templates.md（模板索引）
- references/templates-specs.md（Design Doc）
- references/stage-playbook.md（Stage 2）

## 完成判定
- [ ] 验收标准可被开发者直接用
- [ ] 没有"大而全总集"
```

### new-adr.md

```md
# Prompt: New ADR

## 何时用
- 做出可追溯的架构/技术决策（选库、改边界、引入新模式）
- 决策有后果，未来要回溯"当时为什么这么选"

## 步骤
1. 找 `docs/decisions/` 下当前最大编号 N
2. 新建 `ADR-<N+1>-<kebab-title>.md`
3. 套 `spec-templates.md` → `templates-governance.md` 的 ADR 模板
4. 链接到 ARCHITECTURE.md Technical Baseline 对应行
5. 提交 PR 走 review

## 引用
- references/spec-templates.md（模板索引）
- references/templates-governance.md（ADR 模板）
- references/architecture-cases.md / architecture-cases-ai.md（决策时查 case）

## 完成判定
- [ ] 文件名有连续编号
- [ ] 状态字段明确（proposed/accepted/superseded/deprecated）
- [ ] 后果（trade-offs）写清
```

### pre-pr.md

```md
# Prompt: Pre-PR Self-check

## 何时用
- 准备开 PR 前
- 用户说"准备提交 / 该开 PR 了 / ship"

## 步骤
1. 跑 PR Readiness checklist
2. 跑 Constitution 红线检查
3. 同步 design doc / requirements / DESIGN（UI）/ ARCHITECTURE（架构改动）
4. 更新 active-context.md：标 PR 状态
5. 输出 PR body（`spec-templates.md` → `templates-specs.md` 模板）

## 引用
- references/checklists.md（PR Readiness）
- references/spec-templates.md（模板索引）
- references/templates-specs.md（PR Body）

## 完成判定
- [ ] PR Readiness 全过
- [ ] Constitution 0 违反
- [ ] PR body 完整
```

### update-active-context.md

```md
# Prompt: Update Active Context

## 何时用
- 每次会话结束前
- 重要进展后

## 步骤
1. 在 active-context.md 写"已完成"
2. 写"进行中" / "下一步"
3. 列阻塞与决策待定
4. 给下一会话 agent 的留言（具体）
5. 仅当发现新/反模式时，提议更新 patterns.md（不擅自改）

## 引用
- references/memory-bank-guide.md

## 完成判定
- [ ] 时间戳更新
- [ ] hand-off 留言具体（不是"继续"这种废话）
```

### refactor-safely.md

```md
# Prompt: Refactor Safely

## 何时用
- 行为保持型重构
- 用户说"清理 / 模块化 / 重命名 / 重构"

## 步骤
1. 明确"不改变行为"
2. 识别覆盖测试 / smoke check（缺则先补）
3. 校验 Constitution（重构不违反红线）
4. 走 vertical slice，不大爆炸
5. 边界变化时同步 ARCHITECTURE / folder declaration / patterns.md
6. 收尾跑测试

## 引用
- references/checklists.md（Refactor Checklist）
- references/stage-playbook.md（Stage 9）

## 完成判定
- [ ] 行为不变（测试为证）
- [ ] 边界变化已同步文档
```

### debug-incident.md

```md
# Prompt: Debug Incident（救火）

## 何时用
- 生产/UAT 出问题
- 用户说"挂了 / 错了 / 不工作了"

## 步骤
1. 先恢复（回滚 / feature flag / 临时绕过）
2. 复现路径：最小重现
3. 定位 root cause（log + trace + diff 最近变更）
4. 修复 + 加 regression test
5. 写 post-mortem 入 `docs/post-mortems/`（建议而非强制）
6. 更新 patterns.md「反模式」（如果是模式问题）

## 引用
- references/checklists.md（Bug Fix Checklist）
- references/architecture-cases.md §11（可观测性）

## 完成判定
- [ ] 已恢复
- [ ] regression test 已加
- [ ] root cause 已记录
```

### spike-start.md

```md
# Prompt: Spike Start（启动 POC / Spike）

## 何时用
- 用户说 POC / spike / demo / sandbox / 试一下 / 一次性脚本
- 生命周期 < 1 周、单人开发、不进生产、不接真实敏感数据
- 目标是回答一个明确的可行性问题

## 我需要的输入
- 要验证的具体问题（一句话）
- 验收信号：什么结果算 "可行"，什么结果算 "不可行"
- 数据来源（脱敏样本 / mock / 公开数据）
- 时间盒（建议 ≤ 5 个工作日）

## 我会交付
- `README.md`（POC 问题、运行方式、结论位置）
- `.gitignore`（output / runtime / data / secret 全 ignore）
- 依赖声明（requirements.txt / pyproject.toml / package.json）
- `docs/spike-note.md`（假设、方案、样本、结论、是否升级）
- 可重复运行的 smoke command
- 升级判断：是否命中升级触发条件

## 步骤
1. 确认时间盒与升级触发条件已对齐用户。
2. 落地 4 份最小文件（README / .gitignore / 依赖 / spike-note）。
3. 写 smoke command，先确保能跑通最小路径。
4. 实施验证，把假设、方法、样本、结果填回 spike-note。
5. 收尾：给结论（可行 / 不可行 / 需二次验证）+ 证据 + 下一步（废弃 / 归档 / 升级）。
6. 任一升级触发条件命中 → 立即停止 POC 模式，切回 `scaffold-new-project`。

## 引用的 references
- references/scenario-playbooks.md §G（POC / Spike 轻量模式 + 升级触发）
- references/stage-playbook.md Stage P
- references/checklists.md（POC / Spike Checklist）

## 输出格式
7 字段模板，`项目形态` 注明 `POC/Spike`，`停止条件` 必须包含升级触发条件。

## 完成判定
- [ ] 4 份最小文件已落地
- [ ] smoke command 可重复运行
- [ ] spike-note 有结论和证据
- [ ] 升级判断已显式给出（是 / 否 + 命中哪条）
```

### scenario-routing.md

```md
# Prompt: Scenario Routing（业务场景路由）

## 何时用
- 用户描述命中业务自动化场景：RPA / 数据采集、OCR / 文档智能、视觉 / 多媒体质检、数据治理 / 分析报表、LLM 生产链路、浏览器自动化 / WebAgent。
- 信号词：电商后台、千牛 / 京麦 / 抖店、PDF / 发票 / 合同、图像质检、经营报表、prompt 多版本、Playwright / Selenium 等。

## 我需要的输入
- 当前阶段（用 SKILL.md 7 字段先判断）
- 项目形态（已选 / 待定）
- 命中的场景（从 scenario-playbooks 表里挑 1 条，或 `Unknown`）
- 是否已有 design doc / ADR

## 我会交付
- 命中场景的最小切片清单（直接来自 scenario-playbooks 对应小节）
- 默认技术选型 + 不推荐反例 + 退出成本
- design doc 必写章节
- 验证门禁与反模式
- 是否需要补 ADR（架构维度变化时）

## 步骤
1. 在 `scenario-playbooks.md` 场景速查表里定位场景小节（A–G）。
2. 把该小节的「最小切片」「Design Doc 必写」「验证门禁」「反模式」抽出来。
3. 如果场景涉及架构维度（LLM 调用方式、RAG、Prompt 管理、向量库等）→ 同步指向 `architecture-cases-ai.md` 对应大类，**不复制内容**。
4. 把场景信息写进 7 字段模板的 `项目形态` 字段，例如：`Python Agent/CLI（场景：OCR / 文档智能）`。
5. 校对 checklists.md 对应场景 checklist；缺失项进入 `缺失内容`。

## 引用的 references
- references/scenario-playbooks.md（首选）
- references/architecture-cases-ai.md（命中 LLM / Agent 架构维度）
- references/checklists.md（场景 Checklist）
- references/spec-templates.md → templates-specs.md（design doc 模板）

## 输出格式
7 字段模板，`项目形态` 必须带场景标签；`下一步 3 个动作` 至少包含「补齐场景 design doc 必写章节」与「跑场景 checklist」各 1 条。

## 完成判定
- [ ] 场景已定位到 scenario-playbooks 具体小节
- [ ] 最小切片 / 默认选型 / 验证门禁 / 反模式 已抽到回答里
- [ ] 架构维度变化已明确是否需要 ADR
- [ ] 场景 checklist 已跑或已记入 `缺失内容`
```

---

## prompts/README.md（索引）

每个项目的 `docs/prompts/README.md` 应该列出可用 prompt 与触发信号：

```md
# Prompts Index

| Prompt | 触发信号 |
|---|---|
| scaffold-new-project | 新 repo / 空目录 / "怎么搭" |
| spike-start | POC / spike / demo / 一次性脚本 / 试一下 |
| handover-audit | 接手 / 老代码 / "怎么继续" |
| new-feature | 加功能 / 加页面 / 加 API |
| scenario-routing | 命中 RPA / OCR / 视觉 / 数据治理 / LLM 生产链路 / 浏览器自动化 |
| new-design-doc | 要 PRD / design doc |
| new-adr | 选库 / 改边界 / 改模式 |
| pre-pr | 准备提交 / ship |
| update-active-context | 会话结束 / 重要进展后 |
| refactor-safely | 重构 / 清理 / 重命名 |
| debug-incident | 挂了 / 错了 / 救火 |
```

## 与 SKILL.md 决策树的关系

prompts 是**决策树命中后的"操作手册"**，不替代 SKILL.md 的路由：

```
用户请求
  → SKILL.md 决策树（命中第几条 + 阶段判断）
    → 找到对应 prompt（如 scaffold-new-project）
      → prompt 路由到具体 references
        → 输出 7 字段模板 + 产物
```

> 项目可在自家 `docs/prompts/` 覆盖或新增。本 Skill 提供基线模板。

## 反模式

- prompt 把 references 内容复制进来 → 双份维护必漂移。
- prompt 写成长篇散文 → agent 难解析。
- 一个 prompt 包含多个意图 → 拆成多个。
- 不更新 prompts/README.md 索引 → 写了没人知道。
- 把敏感凭证 / 客户数据写进 prompt → 会被 agent 多次读到。
