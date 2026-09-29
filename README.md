# zh-book-skills

**25 Chinese books → 25 callable methodology skill packs for AI agents (539 capability cards).**

*把 25 本中文书蒸馏成 AI Agent 可直接调用的方法论技能包——[中文完整说明见文末](#中文说明)。*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Skill Packs](https://img.shields.io/badge/skill_packs-25-8b5cf6.svg)](#the-25-packs)
[![Capability Cards](https://img.shields.io/badge/capability_cards-539-2ea44f.svg)](#the-25-packs)
[![Quote Verification](https://img.shields.io/badge/quote--verified-99.8%25-f59e0b.svg)](#verified-quality)
[![Pipeline](https://img.shields.io/badge/pipeline-cangjie--skill%20v2.5-1677ff.svg)](https://github.com/kangarooking/cangjie-skill)
[![Docs](https://img.shields.io/badge/browse_539_cards-online-2ea44f.svg)](https://aholick.github.io/zh-book-skills/)

---

> 🔧 **Want to distill your own batch of books?** Read the three docs in [`docs/`](./docs/) (Chinese):
> [`怎么做skill-从一本书到可调用的能力.md`](./docs/怎么做skill-从一本书到可调用的能力.md) (methodology long-form + all pitfalls),
> [`书籍蒸馏-Skill生产流程手册.md`](./docs/书籍蒸馏-Skill生产流程手册.md) (production playbook),
> [`自己做第一个skill-实操清单.md`](./docs/自己做第一个skill-实操清单.md) (step-by-step checklist).

## What is this

Each book was distilled into an **agent skill** — not book notes, but a **methodology an AI agent can invoke and execute**.

The difference from reading notes: notes say "what the book says"; each card here says "**when** to use it, **which steps** to follow, and **how you know it was done right**". Every capability card ships with applicability conditions and non-applicability boundaries.

- **Scale**: 25 skill packs / **539 capability cards** / 689 files (~8.1 MB)
- **Format**: plain markdown following the [Agent Skills open specification](https://agentskills.io/specification) (`SKILL.md` + YAML frontmatter + `references/`) — works with Claude Code, hermes, and other compatible agents
- **Pipeline**: [cangjie-skill](https://github.com/kangarooking/cangjie-skill) (MIT), RIA-TV++ v2.5 — six stages from candidate extraction to delivery
- **Browse online**: 🌐 [https://aholick.github.io/zh-book-skills/](https://aholick.github.io/zh-book-skills/) — all 539 cards without installing anything

## Quick start

### Use as plain documents

Just read `SKILL.md` and `references/capabilities/*.md`. It's all markdown.

### Install into an AI agent (recommended)

This is what they're designed for. Drop a pack directory into your agent's skills directory:

| Platform | Skills directory |
|---|---|
| Claude Code (personal) | `~/.claude/skills/` (Windows: `%USERPROFILE%\.claude\skills\`) |
| Claude Code (project) | `.claude/skills/<skill-name>/` inside the repo |
| WorkBuddy | `~/.workbuddy-ai/skills/` |
| hermes (Windows) | `%LOCALAPPDATA%\hermes\skills\` |
| hermes (Linux/macOS) | `$HERMES_HOME/skills/` (default `~/.hermes/skills/`) |

The agent will auto-invoke them when a matching scenario appears (trigger conditions live in each `SKILL.md` `description`); you can also say "use the XX skill for this".

> ⚠️ **Install one pack at a time — don't nest directories.** Recursive scanners treat every `SKILL.md` in a subdirectory as an independent skill.

> 🌐 **English readers**: card content is Chinese (the source books are Chinese editions). Capability slugs, section labels (R/I/A1/A2/E/B) and the routing tables are structured, so agents route correctly and can translate content on the fly.

### Not sure which pack to use?

Several books cover the same domain (3× Python, 3× psychology, 3× thinking tools). Check the **[routing cheatsheet](./路由冲突速查.md)** (`路由冲突速查.md`, Chinese); **naming the book explicitly is the most reliable route** (7/7 hit rate in testing).

## How to read a pack (example: `seven-habits`)

```
seven-habits/
├── SKILL.md                     ← ① start here (triggers + capability routing table)
├── BUILD_MANIFEST.json          ← build manifest (per-file SHA-256; ignorable)
└── references/
    ├── overview.md              ← ② whole-book overview
    ├── capability-index.md      ← ③ capability index
    ├── cheatsheet.md            ← one-page cheat sheet
    ├── glossary.md              ← glossary
    └── capabilities/*.md        ← ④ individual capability cards (15)
```

Each card follows the **RIA-TV++ six-section structure**:

| Section | Content |
|---|---|
| **R** (Reading) | Source quote with line-level location — the verifiable anchor |
| **I** (Interpretation) | The methodology skeleton: what principle this encodes |
| **A1** | In-book application cases (book cases vs. synthetic drills labeled separately) |
| **A2** ★ | Future triggers: in which situations a user needs this |
| **E** (Execution) | Executable steps: I/O contracts, completion criteria, stop conditions |
| **B** (Boundary) ★ | When **not** to use it; author blind spots and failure modes |

`E`/`B` are the most skipped — and most valuable — sections: they decide whether the skill gets invoked in the wrong scenario.

## The 25 packs

| # | Directory | Book (author) | Domain | Cards |
|---|---|---|---|---|
| 1 | `intellectual-modern` | 智识分子 The Intellectual (万维钢) | Thinking | 39 |
| 2 | `underlying-logic` | 底层逻辑 The Underlying Logic (刘润) | Thinking | 38 |
| 3 | `thinking-fast-and-slow` | Thinking, Fast and Slow (Kahneman, zh. ed.) | Thinking | 33 |
| 4 | `computation` | 计算 Computation (吴翰清) | Tech | 27 |
| 5 | `social-psychology` | Social Psychology (Myers, zh. ed.) | Psychology | 27 |
| 6 | `cognitive-drive` | 认知驱动 The Cognitive Drive (周岭) | Growth | 26 |
| 7 | `python-crash-course` | Python Crash Course (Eric Matthes, zh. ed.) | Python | 24 |
| 8 | `how-to-read-a-book` | How to Read a Book (Adler & Van Doren, zh. ed.) | Method | 22 |
| 9 | `poor-charlies-almanack` | Poor Charlie's Almanack (Munger, zh. ed.) | Thinking | 21 |
| 10 | `counseling-for-toads` | Counselling for Toads (de Board, zh. ed.) | Psychology | 20 |
| 11 | `automate-boring-stuff` | Automate the Boring Stuff (Sweigart, zh. ed.) | Python | 20 |
| 12 | `python-quick-reference` | Python Quick Reference (unattributed source) | Python | 20 |
| 13 | `one-thing-in-life` | 人生只有一件事 One Thing in Life (金惟纯) | Growth | 19 |
| 14 | `courage-to-be-disliked` | The Courage to Be Disliked (Kishimi & Koga, zh. ed.) | Psychology | 19 |
| 15 | `deliberate-practice` | Peak (Ericsson & Pool, zh. ed.) | Method | 19 |
| 16 | `elite-time-control` | The Elite's Time Control (桦泽紫苑, zh. ed.) | Productivity | 19 |
| 17 | `naval-almanack` ⚠️ | The Almanack of Naval Ravikant (zh. ed.) | Wealth | 19 |
| 18 | `siddhartha` | Siddhartha (Hesse, zh. ed.) | Meaning | 18 |
| 19 | `goodbye-sapiens` | 再见智人 Goodbye Sapiens (吴冠军) | Thinking | 17 |
| 20 | `settle-yourself` | 往里走，安顿自己 Settle Yourself (许倬云) | Psychology | 17 |
| 21 | `a-year-rebirth` | 用一年时间重生 A Year of Rebirth (娜里跑) | Growth | 16 |
| 22 | `myth-of-sisyphus` | The Myth of Sisyphus (Camus, zh. ed.) | Meaning | 16 |
| 23 | `seven-habits` | The 7 Habits of Highly Effective People (Covey, zh. ed.) | Productivity | 15 |
| 24 | `let-time-make-you-rich` | 让时间陪你慢慢变富 Let Time Make You Rich (李笑来) | Investing | 15 |
| 25 | `nonviolent-communication` | Nonviolent Communication (Rosenberg, zh. ed.) | Communication | 13 |

> ⚠️ **`naval-almanack` has a different provenance**: it is the built-in example shipped with cangjie-skill, not produced by this batch. It passed all structural checks (`validate_skill_pack.py` 0 errors) and works fine, but never went through this batch's two-layer blind evaluation, so it carries no quality numbers. Its `description` was narrowed on 2026-09-20 (card content untouched; traceable via the `local_patch` field in `BUILD_MANIFEST.json`).

## Verified quality

All numbers are author-measured; methods and pitfalls are documented in [docs/](./docs/) (Chinese):

- **Quote traceability: 1725/1729 = 99.8%** (20 books at 100%). Every R-section quote was matched back to the OCR source; the 4 misses were each re-read and explained (`[sic]` marks / list reformatting / OCR truncation) — **none fabricated**.
- **Card-level selection: 98.8%** (500/506) — given the right book, does the agent pick the right card? `trigger`/`global` perfect; weakest class is decoy cards ("looks like A, actually B") at 97.1%.
- **Cross-book routing**: 146 real prompts × 25 candidates × 6 sampling rounds. Hit-rate by source book is 50.9%–61.6%, but per-item re-reads show the landing book almost always **catches the question** — an inherent property of same-domain multi-book corpora, not a defect rate. All 25 packs reachable.
- **Answer-quality double-blind**: 12 stable collision pairs × left/right swap re-judging (24 judgments). After position debiasing, source book 13 : 11 (p=0.84, n.s.) — landing book ≈ source book.
- **Safety fix (3 same-type defects)**: self-harm / domestic-violence prompts used to be answered `none` (user gets nothing) because broad exclusion sentences in two `description` fields locked out the packs' own safety cards. Fixed and re-verified: 4 domestic-violence cases 8/8 routed correctly; the psychological-safety cards now carry China's national mental-health hotline **12356** (nationwide since 2025-05-01).
- **Structural integrity**: all **664 SHA-256 entries** across the 25 `BUILD_MANIFEST.json` files re-verified at release time (`.gitattributes` sets `* -text`, so hashes stay valid on any platform); `validate_skill_pack.py` 0 errors / 0 warnings; frontmatter passes the official [skills-ref](https://github.com/agentskills/agentskills) validator (25/25).
- **Security scanning**: SkillSpector full scan of all 25 packs → **all SAFE**; every heuristic alert re-read against source and confirmed a false positive (HTML comments / git commands quoted from the books).

### Known limitations (stated, not hidden)

- No end-to-end "book-level × card-level" composite number (independent measurements; multiplying them would be a fake number).
- Answer-quality double-blind: n=12 pairs, single judge — directional evidence only.
- **`python-quick-reference`**: source is an unattributed OCR scan; its glossary is an API-syntax reference with significantly higher verbatim density than the other books (~32% at 16-gram level vs. 6.4% corpus average) and cannot be attributed further.
- OCR artifacts are preserved as-is (some marked `[sic]`); no manual proofreading pass.

## Copyright & disclaimer (important)

- **No book full-text is included in this repository.** Capability cards are structured methodology extractions; R-section quotes (median 49 chars, max 163 chars; ~6.4% verbatim coverage of the sources at 16-gram level) are brief quotations for illustration and commentary, each with line-level attribution.
- **All books remain © their authors and publishers.** Nothing here is a substitute for reading the originals — every card's citations point back to the source book. Please buy the books.
- **`python-quick-reference` special note**: the source is an unattributed OCR scan (no copyright page, no author info). If you are the rights holder, please open an issue.
- The pipeline [cangjie-skill](https://github.com/kangarooking/cangjie-skill) is MIT-licensed; the `naval-almanack` directory derives from its bundled example.
- This repo's own docs and structure are released under **MIT** (see [LICENSE](./LICENSE)); that license does **not** cover the underlying copyright of quoted book passages.
- For medical / legal / psychological-crisis scenarios, each pack specifies referral paths — **not a substitute for professional help**.
- Rights holders who believe any content exceeds fair quotation: open an issue and we'll address it.

## Repository structure

```
zh-book-skills/
├── README.md                          ← this file
├── 路由冲突速查.md                     ← cross-book routing cheatsheet (for humans)
├── docs/                              ← how this was built (Chinese)
│   ├── 怎么做skill-从一本书到可调用的能力.md   (methodology long-form)
│   ├── 书籍蒸馏-Skill生产流程手册.md          (production playbook)
│   └── 自己做第一个skill-实操清单.md          (step-by-step checklist)
├── site/                              ← GitHub Pages (English, static, no build step)
├── LICENSE  ·  .gitattributes         (MIT · `* -text` keeps hashes platform-stable)
└── 25 skill packs/                    ← see table above; each independently installable
```

## Acknowledgements

- [cangjie-skill](https://github.com/kangarooking/cangjie-skill) (MIT) — the RIA-TV++ distillation pipeline;
- The authors and translators of the 25 books — the real methodology is theirs; this repo only makes it easier to use.

---

## 中文说明

每本书被蒸馏成一个 **agent skill（技能包）**——不是读书笔记，而是**可被 AI 直接调用执行的方法论**：遇到什么情况该怎么做、按什么步骤做、做完怎么算做对了，每张能力卡都带适用条件与不适用边界。

- **总量**：25 个技能包 / 539 张能力卡 / 689 个文件（约 8.1 MB），遵循 [Agent Skills 开放规范](https://agentskills.io/specification)
- **流水线**：[cangjie-skill](https://github.com/kangarooking/cangjie-skill) RIA-TV++ v2.5 六阶段
- **质量**：引文回源核验 99.8%、卡级选卡 98.8%、25 包安全扫描全 SAFE、664 条 SHA-256 可复算（各项完整数字与核验方法见 [Verified quality](#verified-quality) 一节与 [docs/](./docs/) 三篇文档）
- **在线浏览**：全部 539 张能力卡 → [https://aholick.github.io/zh-book-skills/](https://aholick.github.io/zh-book-skills/)
- **装进 agent**：把某个技能包目录放进 agent 的 skills 目录（Claude Code: `~/.claude/skills/`；hermes Windows: `%LOCALAPPDATA%\hermes\skills\`；WorkBuddy: `~/.workbuddy-ai/skills/`）；同域多书拿不准时查 [`路由冲突速查.md`](./路由冲突速查.md)，指名书名最稳（实测 7/7）
- **版权**：本仓库不含任何原书全文；引文为说明与评论目的的有限引用并逐条附出处；著作权归原作者与出版社所有，请支持正版；权利人可提 issue。

上方英文部分与中文说明内容一致（结构、用法、核验数字、局限、版权）。
