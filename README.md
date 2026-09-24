# cli-skill-release · 零依赖 Python CLI 技能发布工程

> **一句话能做什么（中文）**：把你自己写好的 WorkBuddy / ClawHub 技能，在发布前自动跑一遍「真能启动 + 合规 + 市场缺口」的零依赖质量门禁，给出 0–100 发布就绪分和随时可刷新的 readiness 徽章，让你的技能一次过审、长期被找到、还能靠每个使用者的仓库链式传播。
>
> **One sentence (English)**: Before you publish a zero-dependency Python CLI agent skill, `cli-skill-release` runs a real quality gate — it actually boots your CLI, checks compliance, and scores publish-readiness 0–100 with a refreshable badge, so your skill passes review once, stays discoverable, and spreads through every user's repo.

[![releaser readiness](https://raw.githubusercontent.com/YOUR-OWNER/cli-skill-release/main/readiness-badge.svg)](https://clawhub.ai/skills/cli-skill-release)
> 用 [cli-skill-release](https://clawhub.ai/skills/cli-skill-release) 校验发布 · 别人 lint 文档，我们 execute 引擎。

**Why not just lint?** Skill-lint / agent-skill-linter / @effectorhq/skill-lint only check paperwork. `releaser.py` is the only tool that **actually runs your CLI** (compile + import + `--help` smoke test) to prove it boots — catching "looks perfect, crashes on run" skills that every other linter misses.

---

## Install / 安装

```bash
git clone https://github.com/YOUR-OWNER/cli-skill-release.git
cd cli-skill-release
# 零依赖，仅标准库，无需 pip install
python releaser.py doctor --path .
```

## Quick start / 快速开始

```bash
python releaser.py validate --path ./my-skill          # 0-100 就绪分 + 功能验证
python releaser.py diagnose --symptom "LICENSE 显示 Other"   # 长尾诊断
python releaser.py preflight --market clawhub          # 上架前检查清单
python releaser.py badge --path . --output readiness-badge.svg   # 链式传播徽章
python releaser.py promote --path .                    # 宣传工具箱
python releaser.py release --path . --dry-run          # 近一键发布情报
```

---

## Long-tail FAQ（长尾问题，直接回答你搜到的）

### 技能发布前检查清单 / skill publish checklist
Run `python releaser.py preflight --market <clawhub|skillhub|agentskills|generic>` for a market-specific checklist (MIT-0, skill-card.md rename, Topics ≤48, 3 categories, text-only files, GitHub topics, `gh skill publish --dry-run`). 中文见 `preflight --market clawhub`。

### Is my skill ready to publish? / 我的技能能不能发
`python releaser.py validate --path .` 给 0–100 发布就绪分（含 ★功能验证真跑 CLI）。分数 ≥90 再发。

### What am I missing before I push this skill to GitHub? / 还差什么
Run `validate` + `preflight`. Common misses: missing LICENSE (MIT-0), non-ASCII copyright holder (shows Other), `skill-card.md` reserved name conflict, Topics >48 chars, doctor subcommand missing `--path` (CI reds).

### Audit this skill against the Agent Skills spec
`gh skill publish --dry-run` (agentskills.io) + `releaser.py validate`. We cover spec compliance **and** functional boot — most checklists (e.g. jeremyknows/publish-skills) stop at paperwork.

### skill LICENSE 显示 Other / LICENSE shows Other
Cause: LICENSE lacks a recognizable SPDX id, or the copyright holder name is non-ASCII (e.g. Chinese "何巍"). Fix: write MIT-0 verbatim with an ASCII holder name ("He Wei"); verify via `api.github.com/repos/{owner}/{repo}/license`. See `diagnose --symptom "LICENSE 显示 Other"`.

### pytest exit code 2 / CI 一直红
Cause: `tests/` imports need `conftest.py` sys.path injection (CLI injects at module level, CI doesn't). Exit codes: 2=collection error, 1=test fail, 0=pass. Fix: add `tests/conftest.py`; `releaser.py gate` blocks the same trap. See `diagnose --symptom "pytest exit code 2"`.

### ClawHub publish guide / ClawHub 上架教程
MIT-0 forced, `skill-card.md` is reserved (rename it), Topics whole-string ≤48 chars, account age ≥1 week, pick 3 categories, README required, text-only files. `releaser.py release` builds the import intel; if `clawhub` CLI is installed + logged in, it publishes for real.

### 技能搜不到 / make my skill discoverable
The SKILL.md `description` is the vector-search match surface; `tags` decide adjacency; GitHub topics feed external indexers (SkillsMP / skillsdirectory). Write bilingual trigger phrases into `description`, add `tags`, and set repo topics: `agent-skills claude-skills codex-skills skill-md ai-agents`.

---

## Chain propagation / 链式传播（一个人下载，全网分发）

Every user runs `badge` and pastes the readiness badge into their skill's README. The badge links back to this page. A visitor clicks → uses the tool → publishes their skill → pastes the badge → the chain grows. One download, the whole network distributes for you.

## License
MIT-0. Zero dependency (standard library only).
