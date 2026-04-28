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

### 必备（任何项目）

- [ ] 已写项目一句话定义。
- [ ] 已选择项目形态。
- [ ] `README.md` 存在。
- [ ] `ARCHITECTURE.md` 存在，记录所有关键架构决策（交叉引用 ADR）。
- [ ] `DEVELOPMENT.md` 存在。
- [ ] `BRANCHING.md` 存在并已生效（团队已知）。
- [ ] `CONSTITUTION.md` 存在（红线 v0.1）。
- [ ] `AGENTS.md` 存在（明示先读 active-context + Constitution）。
- [ ] `docs/requirements/requirements-v0.0.1.md` 存在（来自用户）。
- [ ] `docs/design/active/design_doc-v0.0.1-bootstrap.md` 存在；`backlog/` 与 `done/` 目录已建立（可空）。
- [ ] `docs/decisions/ADR-0001-record-architecture-decisions.md` 存在。
- [ ] `docs/governance/folder-declaration-v0.md` 存在。
- [ ] `docs/governance/changelog.md` 存在。
- [ ] `.env.example` 存在，真实 secret 已 ignore。
- [ ] bootstrap script 存在。
- [ ] 最小 app/service/CLI 可以运行。

### UI 类项目额外

- [ ] `DESIGN.md` 存在（design tokens + 组件清单 + 交互模式）。
- [ ] `docs/design/layout-spec-<page>.md` 存在（首页或核心页）。

### Agent 协作项目额外

- [ ] `docs/memory-bank/brief.md` / `tech-context.md` / `patterns.md` / `active-context.md` 四件套存在（参考 `memory-bank-guide.md`）。
- [ ] `docs/prompts/README.md` 存在，至少含基线 prompt（参考 `prompts-guide.md`）。

### AI / LLM 项目额外

- [ ] LLM 调用统一 client 已就位（红线 §6 项可勾）。
- [ ] `prompts/` 目录已建立（与 `docs/prompts/` 区分：`prompts/` 是运行时 prompt 文件，`docs/prompts/` 是 agent 操作手册）。
- [ ] eval baseline 计划已写（即使未实施）。

---

## POC / Spike Checklist

适用于生命周期短、单人验证、未进生产的临时验证。命中任一升级触发条件，停止 POC 模式，切回正式项目流程。

### POC 最小必备

- [ ] `README.md` 写清要验证的问题、运行方式、结论位置。
- [ ] `.gitignore` 覆盖 `output/`、`runtime/`、`logs/`、`uploads/`、`temp_*`、真实数据目录。
- [ ] 有依赖声明（`requirements.txt` / `pyproject.toml` / `package.json`）。
- [ ] 有 `docs/spike-note.md` 或等价文档，记录假设、样本、方法、结果、是否升级。
- [ ] 样本数据脱敏；真实 token/cookie/客户数据不入仓。
- [ ] 有一个可重复运行的 smoke command。

### 升级触发

- [ ] 第二个开发者要接手。
- [ ] 接入真实客户数据、生产账号或长期凭证。
- [ ] 持续运行超过 1 周。
- [ ] 要成为产品/交付项目基础。
- [ ] 新增存储、权限、公共 API、后台任务或持续部署。

## 场景 Checklist

命中具体业务场景时，读取 `scenario-playbooks.md` 后做最小自检。

### RPA / 数据采集

- [ ] 中国电商场景已先确定平台：淘宝系 / 京东 / 拼多多 / 抖音。
- [ ] 已区分数据源类型：公域数据 / 商家管理后台 / 客户自有系统。
- [ ] 公域数据已记录合法性、授权边界、平台 ToS 风险；逆向/抓包默认不作为推荐路径。
- [ ] 商家后台数据已优先评估官方 API、后台导出、异步报表下载能力。
- [ ] 商家后台浏览器自动化已评估 `undetected-chromedriver` + Selenium，不默认使用 plain Chromium。
- [ ] 浏览器自动化 selector 有集中维护策略；中国电商后台优先 XPath + 文本/层级锚点。
- [ ] 动作频率有 bounded random sleep、任务级 rate limit、明确等待条件和失败重试上限。
- [ ] 手机 RPA 方案已说明设备/账号授权、节流、截图/录屏证据、失败状态和人工接管。
- [ ] 手机 RPA 默认使用 Appium + ADB/iOS automation，并说明设备池和账号状态管理。
- [ ] 数据来源、授权边界、平台 ToS 风险已记录。
- [ ] API / 自动化 / OCR / 抓包等方式的选择理由已写清。
- [ ] raw 证据、标准化结果、分析结果分层。
- [ ] 登录失效、限流、页面变更、空数据有失败态。
- [ ] 不提交 cookie、客户数据、导出的敏感原始数据。

### OCR / 文档智能

- [ ] 输出 schema 先于模型实现。
- [ ] 已判断 OCR 是后台基础能力还是核心能力。
- [ ] 已记录服务器规格和并发预算；2C2G/2C4G 低配后台优先轻量 OCR，不默认引入 PaddleOCR。
- [ ] 如选择 PaddleOCR，已有理由：准确度要求高、中文复杂版面、表格/版面恢复或 OCR 是核心能力。
- [ ] page image / OCR blocks / extracted fields / reviewed result 分层。
- [ ] 关键字段有置信度、来源页码/bbox、校验或人审。
- [ ] 至少 3 个真实样本 fixture 或脱敏样本。

### LLM 生产链路

- [ ] AI 任务已先按目标分流到经典场景：NLP 分类/抽取、CV 检测/分割/理解、统计/时序、LLM/Agent 混合。
- [ ] 已按成本、时效、效果、流量、私有化要求选择经典算法 / 经典算法 + LLM/VLM 兜底 / 纯 LLM/VLM。
- [ ] LLM/VLM 成本已按单次调用估算；高流量任务（如 100000/day 量级）已避免默认纯 API LLM。
- [ ] NLP 分类/抽取已评估 Level 1 规则/正则、Level 2 PaddleNLP 零样本、Level 3 BERT/PaddleNLP、Level 4 LLM 兜底。
- [ ] 没有数据集但要快速落地的 NLP 任务，可用 LLM prompt baseline；长期/私有化/低预算任务需验证 BERT/PaddleNLP。
- [ ] CV 固定识别/检测优先 YOLO；精准分割优先 YOLO-seg；通用分割/辅助标注可用 SAM；图像理解用 VLM。
- [ ] 图像语义比对已评估 image embedding + 向量库；VLM 只做开放理解/兜底解释，不替代高精度检测/分割。
- [ ] 统计/时序任务已建立 XGBoost/LightGBM 或领域 SOTA baseline；LLM 只做解释报告或 Agent 调工具做 chat analysis。
- [ ] 分类/抽取任务有标签集、schema、字段校验和字段级评估；CV 有标注 schema 与 accuracy/mAP/IoU/recall；时序有 backtest 与 MAE/MAPE/RMSE。
- [ ] prompt 有 ID、版本、默认指针和回滚路径。
- [ ] 每次调用记录 provider/model/prompt_version/token/latency/error。
- [ ] 结构化输出有 schema 校验和失败重试策略。
- [ ] 失败样例进入 eval 或 replay 用例库。
- [ ] streaming 场景定义 first chunk、chunk idle、total timeout。

---

## Feature Checklist

- [ ] 已有关联 design doc，并位于 `docs/design/active/`（开工前从 `backlog/` 移过来）。
- [ ] design doc 关联了 requirements doc 和（如有）相关 ADR。
- [ ] 已有关联 requirements doc。
- [ ] Scope 和 non-goals 明确。
- [ ] 已识别影响目录。
- [ ] 数据、状态、API、UI 变化已写清。
- [ ] **不触 `CONSTITUTION.md` 红线**；触红线则已走豁免/修改流程。
- [ ] 触动架构维度的决策已开 ADR（或更新现有 ADR）。
- [ ] UI 改动：layout spec 已更新或新增。
- [ ] UI 改动：所选组件全部在 `DESIGN.md` 内；新组件已先入 `DESIGN.md`。
- [ ] 有测试或验证计划。
- [ ] 已判断是否需要 changelog。
- [ ] 实现已拆成 vertical slices。
- [ ] `docs/memory-bank/active-context.md` 已写当前焦点（agent 协作项目）。

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
- [ ] **`CONSTITUTION.md` 红线 0 触发**（或已走豁免流程并在 PR body 注明）。
- [ ] Design doc 已关联，状态正确（feature 完成时已从 `active/` 移到 `done/` 并回填 `Validation Results`）。
- [ ] requirements doc、架构文档已同步。
- [ ] 架构维度变更：对应 ADR 已新建/更新。
- [ ] UI 改动：DESIGN.md / layout spec 已同步。
- [ ] 验证命令结果明确。
- [ ] 风险和回滚方式已说明。
- [ ] 分支与提交信息符合 `BRANCHING.md`。
- [ ] `docs/memory-bank/active-context.md` 已更新（agent 协作项目）。

---

## Constitution 红线触发处理 Checklist

agent / 开发者发现自己的实现**会**触线时：

- [ ] **立刻停下**，不要"绕路实现"（绕路通常等于偷偷违反）。
- [ ] 在 PR / 对话中明示触发哪条（例 `Constitution §2 架构边界`）。
- [ ] 给三种路径供用户选择：
  - (a) **改方案规避**：调整实现方式，红线不变。
  - (b) **提议放宽红线**：开 PR 修改 `CONSTITUTION.md`，至少 2 名维护者批准。
  - (c) **临时豁免**：由 owner 批准，明确截止时间和补救计划，记入 changelog。
- [ ] 由 owner 决策，不要 agent 自行选路径。

> 红线之外的"觉得不太好"不是触线，走 `DEVELOPMENT.md` / patterns.md 路线沟通。

---

## Memory Bank Hygiene Checklist（agent 协作项目）

每次会话结束前，agent 自检：

- [ ] `active-context.md` `Last updated` 已刷。
- [ ] 已完成 / 进行中 / 下一步 / 给下一会话留言 全部更新（"留言"必须具体，不写"继续"）。
- [ ] 阻塞与决策待定已列。
- [ ] 发现新 pattern / 反模式 → **提议**更新 `patterns.md`（不擅自改）。
- [ ] 发现 `tech-context.md` 与代码现实漂移 → 提议更新。
- [ ] 没把密钥 / 客户名单 / 内部链接（含 token 的）写进 memory-bank。

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
- `CONSTITUTION.md` 写成"代码风格 / 一般最佳实践"大杂烩，触线变成"风格不统一"，红线失效。
- design doc 全堆在 `docs/design/` 一层，没有 lifecycle，agent 看不出哪份还有效。
- ADR 写成"我们决定用 X"一句话，没有 Options Considered 与 Consequences，未来无法回溯。
- Memory Bank 把 `ARCHITECTURE.md` 全文复制进 `tech-context.md` → 双份维护必漂移。
- `active-context.md` 多会话不更新 → agent 基于过期信息工作。
- 把密钥 / 客户名单 / 内部链接写进 memory-bank → 它会被 agent 多次读到。
- prompts 写成长篇散文，agent 难解析；或一个 prompt 包含多个意图。

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
