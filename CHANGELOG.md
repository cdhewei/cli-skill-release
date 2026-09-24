# CHANGELOG

## [1.9.0] — 2026-09-23 · 体系升级（发布侧治理系统：状态层 + 生命周期 + 自证底座 + 供给侧策展）

### 新增（对标 find-skills++ 治理模型，落实体系级支柱 A/B/E/F——纯代码可落地）
- `registry` 子命令 + 本地账本（默认 `~/.workbuddy/cli-skill-release/ledger.json`，可用 `RELEASER_LEDGER` 覆盖）：
  持有"已发布技能账本"（slug/name/repo/version/score/market/path/published_at）。`release` 成功自动登记，
  从"一次性校验器"升级为"发布侧治理者"（状态层 A）。
- `recheck` 子命令：读账本再校验已发布技能，对比登记分数，报告 漂移/弃用/缺失/健康——
  把"发完即终"升级为持续治理（生命周期 B）。
- `selfdemo` 子命令：当场对自己仓库运行每个能力并打印演示摘要（主张即演示），
  对标 find-skills++ 的 smoke.py，碾压级信任不靠叙述靠当场跑（自证底座 E）。
- `curate` 子命令：把 `gap` 的市场情报升级为可操作策展清单（组合机会 / 空白类目 / 单薄类目优先级），
  主动告诉生态该造什么（供给侧策展 F）。

### 设计依据
- 上一轮对标 find-skills++ 体系模型，指出 cli-skill-release 缺 6 根支柱；其中 A/B/E/F 为纯代码可落地，
  本期逐一落实。C/D（双向活通道：find-skills++ 读取 readiness 信号 + 本工具消费其市场数据）需接口打通 + 授权，
  已预留 `gap --scan-json` 与账本数据结构作为对接基座。
- 子命令增至 **17 个**（本期新增 registry / recheck / selfdemo / curate 四大体系支柱 A/B/E/F）；dogfooding 自身仍 12/12 PASS、100/100。

## [1.8.0] — 2026-09-23 · 长尾强化（diagnose 长尾诊断 + preflight 上架清单 + 双语发现元数据 + clawhub CLI 真一键）

### 新增（长尾功能：让"搜索即获客"成立）
- `diagnose` 子命令：内置 **长尾知识库（症状→根因→修复）**，直接回答用户实际会搜的长尾问题
  （"LICENSE 显示 Other" / "pytest exit code 2" / "CI 一直红" / "技能搜不到" / "我的技能能不能发" 等）。
  竞品（jeremyknows/publish-skills 等）只给静态清单，我们给**可执行 + 可诊断**——这是"搜索即获客"的发动机。
- `preflight` 子命令：`--market clawhub|skillhub|agentskills|generic` 输出**市场定制上架前检查清单**
  （markdown 勾选框 + 必填/建议标注），逐条对应真实规则（MIT-0、skill-card.md 保留名、Topics≤48、3 分类、
  仅文本文件、GitHub topics、`gh skill publish --dry-run` 等），回答长尾"skill publish checklist /
  pre-publish checklist for agent skills"。
- **双语 README.md**：用中英文长尾问题做 H2/H3 标题 + 双语正文，被搜索引擎 / ClawHub / GitHub 索引
  （人类与英文搜索面的核心载体，ClawHub 亦要求 README 必填）。

### 发现元数据强化（基于 2026 实测检索机理）
- `description` 改写为**含英文触发短语的双语**（ClawHub 官方确认 `description` 是 vector search 的匹配面），
  并保留 `description_en`；新增 `tags` 字段（publish / skill-publishing / pre-publish-checklist / clawhub /
  agent-skills / quality-gate …，决定市场相邻位）。
- SKILL.md §5.1 写明**中文 / 英文"被找到"三杠杆**（AI keyword 文本匹配 / description vector search / tags 相邻位 /
  GitHub topics 喂外部索引器）+ 长尾词清单（中/英）。

### release 增强（修正"无公开 API"的旧判断）
- 2026 实测：ClawHub **无公开 REST/网页 publish 端点，但有官方 npm CLI `clawhub publish`**。
  `release` 现可**探测并直调 clawhub CLI**（已装且登录即真一键发布，英文生态），缺 CLI 则回退人工导入情报。
  新增 `--no-cli` 强制回退。诚实标注：网页/REST 端点仍不存在，CLI 是合法真一键路径。

### 设计依据
- 先分析"找 skill 的人/AI"的发起与搜索模式（中文 keyword 文本匹配 + 英文 description/tags/topics 三杠杆 +
  自然语长尾触发），据此把"被找到/一直被需求/真解决问题/链式传播"落成**长尾功能**，而非空想营销。

## [1.7.0] — 2026-09-23 · 被发现 + 链式传播（badge 引擎 + promote 工具箱 + 发现元数据）

### 新增（传播系统，回应"如何被找到/一直被需求/链式传播"）
- `badge` 子命令：基于 `validate` 就绪分生成 shields 风格 SVG 徽章（绿≥90/黄70-89/红<70），
  并输出**链回本技能页**的 markdown 片段；徽章图片自托管于用户仓库 raw 地址，渲染必现。
  这是链式传播的发动机——每个使用者挂徽章，访问者点入即转化。
- `promote` 子命令：一次性产出"宣传工具箱"（README 徽章区 + 电梯演讲
  "别人 lint 文档，我们 execute 引擎" + 社媒可转发文案 + 安装一行 + 链式玩法说明），
  把一次下载变成一次分发。
- SKILL.md frontmatter 强化：keywords/description 命中高频查询
  （发布技能 / 上架 skill / publish skill / validate skill / 技能质量门禁 / skill CI gate / 技能徽章）。
- 新增 §5「如何让本技能被找到 + 链式传播」：设计即分发，文档直接教用户帮我们扩散。

### 设计依据
- 先分析"找 skill 的人/AI"的发起逻辑（本地→原生市场 keyword→社区 score 文本匹配→合并排序）
  与操作逻辑，据此把"被找到/一直被需求/真解决问题/链式传播"落成工具能力，而非空想营销。

## [1.6.0] — 2026-09-23 · 碾压级强化（功能验证 + CI 门禁 + 透明评分 + 组合缺口）

- **★功能级/可执行验证（护城河）**：`validate` 新增 compile + import + `--help` 烟测三维，把技能 CLI **真正执行起来证明它能 boot**。竞品（skill-lint / agent-skill-linter / @effectorhq/skill-lint / Skill Validator 等，已联网核查 6 款）全部只查 paperwork，**无人做功能验证**。权重表重排为 12 维（功能验证独占 32/100：compile 8 / import 10 / --help 14），使"就绪分"从"格式分"升级为"格式+功能分"。
- **`gate` 子命令（CI 门禁）**：`gate --path . --min 90` 就绪分低于阈值即非零退出，把"发布就绪分"变成可编排的流水线质量闸门（readiness-as-a-service），零依赖、1 行接入任何 CI。
- **透明评分 `--rubric`**：输出每维度权重+理由（含 `--json` 的 `rubric` 字段），使分数可被审计、可被信任，回应竞品"20 条规则但不可解释"的短板。
- **`gap` 升级为组合缺口**：本机覆盖始终计算；加 `--scan-json` 叠加 find-skills++ 真实市场数据后，交叉出"市场有供给 / 你为 0"的组合机会（独有飞轮情报）。
- 测试 48 → **54**（新增功能验证 compile/import/--help 三态、gate 通过/拦截、--rubric 输出与 JSON 字段、gap 组合机会）。
- Dogfooding 闭环保持：工具对自身 `validate --path .` 仍 **9/9 PASS、就绪分 100/100（含功能维度）**。

## [1.5.0] — 2026-09-23 · 强化路线落地（TAM 泛化 + 对标头部 + 发布接口预留）

- `validate` 新增 `--mode package`：从「CLI 技能」泛化到**任意零依赖 Python 包**发布（再加打包清单 `pyproject.toml/setup.py/setup.cfg` 检测，信息项不计分）。与 `--mode cli` 共同把受众从"技能作者"拓到"所有 Python 包作者"（TAM ×N）。
- `validate` 新增 `--bench`：把绝对就绪分升级为**对标头部技能画像的相对分**——以满分 100 为头部基线（基准=本工具自身 9/9 PASS），报告"距头部还差 X 分 + 哪些加权维度未达标"，更具驱动力；`--json` 时附带 `bench` 字段。
- `release` 预留 `--api-token` 接口：令牌接收后当前仍走人工导入（情报已预填），待 ClawHub 公开 publish 端点即升级为「真一键」。诚实标注阻塞点，不伪造调用。
- 测试 42 → **48**（新增 `--mode package` 含/不含清单、 `--bench` 输出与 `--json --bench` 结构、 `--api-token` 优雅降级）。

## [1.4.0] — 2026-09-23 · 系统化闭环（可组合输出 + 真实市场情报）

- `validate` 新增 `--json`：输出机器可读报告（`{path,mode,score,summary,items}`），可被 CI / 其他工具 / **市场闭环直接消费**，让工具从「给人看」升级为「可编排」。
- `gap` 新增 `--scan-json <file>`：消费 find-skills++ 导出的真实市场扫描数据，把"本机启发式"升级为「真实在售缺口」情报，激活 **find-skills++ 扫缺口 → scaffold → validate → release → 被检索推荐** 的飞轮护城河。
- **Dogfooding 闭环达成**：修复引用完整性误报（正文描述 `scripts/references/assets` 不再被当成真实引用路径）+ 补齐 `.github/workflows/ci.yml`，使 `python releaser.py validate --path .` 对自身达 **9/9 PASS、就绪分 100/100**，工具首次能「验证自己」满分。
- 测试 36 → **42**（新增 `--json` 结构校验、`--scan-json` 市场模式、`score=100` 自校验、递归守卫）。

## [1.3.0] — 2026-09-22 · 碾压级能力（gap 情报 + 就绪分 + 近一键发布）

- `validate` 新增 **0-100 发布就绪分**（加权 PASS/WARN/FAIL），并支持 `--mode skill|cli` 泛化到任意零依赖 CLI 项目。
- 新增 `gap` 子命令（★市场情报）：扫描本机技能按 8 大类目归类，报告空白类目（缺口）与单薄类目（可补强），并给出 `scaffold` 建议；预留接入 find-skills++ 市场数据形成闭环。
- `release` 升级为**近一键发布**：git 推送后自动构造 ClawHub 导入情报（解析仓库地址、预填 Display/Slug、按关键词归类推荐分类、Topics 自动截断 ≤48）；`--dry-run` 不推送只出情报。（诚实说明：最终 Publish 点击需用户登录会话。）
- 测试深度 5 → **36**（覆盖 validate 全分支 / score / gap / bump 三型 / selfcheck / inventory / release dry-run / cli 模式 / 纯函数）。
- 战略评测见 `STRATEGY-EVAL` 思路：竞品给文档，本工具替你执行 + 懂市场。

## [1.2.0] — 2026-09-22 · 工具中心化（参考 find-skills++ 模式）

- 新增零依赖主工具 `releaser.py`（7 子命令）：`validate`（主动扫描全部发布陷阱）、
  `inventory`（治理本机技能就绪度）、`selfcheck`（零依赖校验）、`scaffold`（委托现有 scaffold.py）、
  `bump`（升版本+CHANGELOG）、`release`（git 推送+ClawHub 指引）、`doctor`（自检/校验）。
- SKILL.md 重写为工具中心：散文降级为背景，releaser.py 子命令成为主体。
- 修复：bump 正则分组引用错误（\10）；selfcheck 跳过生成器模板（scaffold.py）避免误报。
- 新增 `LICENSE`（MIT-0）+ ASCII 版权名，补齐自身发布合规。
- 新增 `tests/test_releaser.py`（5 项，managed venv 全过）。

## [1.1.0] — 2026-09-21 · 脚手架 + 竞争力对齐

- 新增 `scaffold.py` 一键生成带 CI 的完整可发布骨架。
- 明确与 skill-creator 边界（写 vs 发）；补 2026 ClawHub 上架规则（MIT-0 / skill-card.md 冲突 / Topics≤48 / 账号龄）。
- 补 CI 失败诊断决策树。

## [1.0.0] — 2026-09-20 · 初版

- 固化 find-skills++（v5.3.0）真实发布工程经验：目录结构、sys.path、doctor 门、CI、LICENSE、排障 SOP。
