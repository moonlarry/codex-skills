# Codex Skills Collection 🚀

> 让 AI 成为你的学术写作助手，而不是冷冰冰的代码机器 ✨

## 这个仓库有什么用？ 🤔

这个仓库源于我使用 Codex 的日常需求。写论文不是单点任务，而是一条从构思、实验、写作、审稿到 rebuttal 的长链路；每一步都需要 AI 有边界、有证据、懂学术表达。因此这个仓库把 **AI Agent 工作协议 + 学术写作 Skills** 组织成当前 7 个模块：

- ✍️ **论文写作主链路**：从材料、claim 和实验结果生成论文计划、LaTeX 初稿、编译检查和完整写作流程。
- 📊 **图表与实验表达**：围绕实验图、表格、caption、架构参考图和结果分析，帮助把 evidence 表达清楚。
- 🧪 **实验验证与 Claim 管理**：规划实验、判断结果是否支撑 claim、设计 ablation，并核对论文数字和证据一致性。
- 🔄 **翻译与润色**：处理中英翻译、段落润色、全文级精修和交互式 polish，让表达更稳、更准。
- 🔍 **审稿与投稿检查**：从 reviewer、期刊/会议格式和引用真实性角度检查投稿前硬伤。
- ∑ **理论与证明**：梳理公式推导、撰写证明、检查 theorem/lemma/proof 的逻辑闭合性。
- 💌 **Rebuttal**：拆解评审意见、制定回应策略、生成和审查正式 rebuttal。

一句话：**让 AI 干活，不让 AI 乱干活。** 🎯

---

## 仓库内容一览 📦

### 1. AGENTS.md — AI 工作协议 🤖

整个项目的"总规矩"，基于 oh-my-codex 定义了 Codex Agent 的工作姿态：

- **核心姿态**：有证据才说话，不确定就问，不瞎猜不硬编
- **Agent 规范**：双角色分工执行，内置通用任务和两个专用场景
- **决策纪律**：多个理解方向时先问用户，不偷偷选一个
- **简洁原则**：最小改动解决问题，不多加功能不多写代码
- **手术式修改**：只改需要的，不改旁边的
- **验证闭环**：改完要验证，不能嘴上说"好了"

执行任务时默认在三种场景中选择，每种场景都有 **架构师 + 执行者** 双角色分工，避免一个Agent既规划又干活容易翻车。论文相关任务遵循 **paper architect/reviewer + paper executor** 范式：前者负责审阅、诊断和计划，后者负责执行、验证和汇报，同时可调用对应的 skills 完成任务。

---

### 2. paper_skills 文件夹 — 论文相关的 Skills 📚

当前包含 **7 个论文写作模块、26 个可触发 Skills**，每个模块对应论文生产中的一段具体流程。迁入的论文工作流共享 `paper_skills/shared-references/` 下的写作、引用、审计和输出协议，并统一使用相对路径引用这些协议。

#### ✍️ 模块一：论文写作主链路（4 个）
把已有研究叙事、实验结果和大纲组织成可编译的论文初稿。

- `paper-plan`：从研究叙事、实验结果或现有材料生成论文大纲，明确 claims、evidence、章节结构、figure plan 和 citation plan。
- `paper-write`：根据 `PAPER_PLAN.md` 逐节生成 LaTeX 论文草稿，初始化模板、章节、数学宏和参考文献。
- `paper-compile`：编译 LaTeX、定位常见错误、循环修复，并检查 PDF、引用、页数和 stale section 文件。
- `paper-writing`：完整写作编排入口，串联 `paper-plan → paper-figure → paper-write → paper-compile → local review-and-polish`。

#### 📊 模块二：图表与实验表达（4 个）
把实验结果、算法结构和图表需求变成论文中能读懂的 visual evidence。

- `paper-analysis`：把实验数据转成 grounded 的 LaTeX 分析段落，或给出论文写作/模型选择建议。
- `paper-figures-advise`：做图表设计建议，负责实验图类型推荐、figure/table caption 和图表表达建议。
- `paper-figure-archi`：从代码或方法描述抽取算法框架，先展示 image-gen prompt，再直接用同一个 prompt 调用 `imagegen` 生成架构图参考图；深度学习默认 Transformer 风格。
- `paper-figure`：从实验结果生成 publication-quality plots、tables 和 LaTeX include 片段。

#### 🧪 模块三：实验验证与 Claim 管理（4 个）
让实验、结论和论文表述之间形成闭环，避免“结果不支撑 claim”。

- `experiment-plan`：把方法或 idea 转成 claim-driven 的实验路线图，规划 baseline、ablation、指标、预算和 run order。
- `experiment-result-to-claim`：实验完成后判断结果支持哪些 claim、哪些 claim 证据不足，并路由到 confirm、supplement 或 pivot。
- `experiment-ablation-planner`：在主结果通过后，从审稿人视角设计必要、可执行、不过度堆叠的 ablation。
- `experiment-claim-audit`：核对论文中的数字、比较和 scope claim 是否严格匹配原始结果文件。

#### 🔄 模块四：翻译与润色（5 个）
处理段落、章节和全文级表达，让论文语言更稳、更准、更像学术写作。

- `paper-translate`：中英双向翻译，支持中文到英文 LaTeX、英文 LaTeX 到中文，以及中文 Word 风格学术重写。
- `paper-refine`：段落级润色、缩短、扩写和去 AI 感重写，适合小范围文本修改。
- `paper-refine-special-en`：英文论文高强度全局润色，按结构、逻辑、表达和 reviewer-style pass 分阶段推进。
- `paper-refine-special-zh`：中文论文高强度全局润色，面向中文 Word 或中文 LaTeX，强调段落角色和章节推进。
- `paper-polish-workflow`：交互式润色流程，结构→逻辑→表达→一致性，分步确认后再改文字。

#### 🔍 模块五：审稿与投稿检查（3 个）
站在审稿人和投稿规范视角，检查论文是否有明显硬伤。

- `paper-review`：做致命逻辑问题扫描或 reviewer-style 拒稿风险报告，适合投稿前 final check。
- `paper-journal-style`：检查目标期刊/会议的 abstract、highlights、title、keywords、跨章节一致性和格式要求。
- `citation-audit`：检查 BibTeX 条目真实性、元数据准确性，以及引用上下文是否真的支持论文表述。

#### ∑ 模块六：理论与证明（3 个）
面向理论线、公式推导和证明完整性，避免“看起来像证明但其实没闭合”。

- `proof-formula-derivation`：把零散公式、假设和推导目标整理成 paper-ready 的理论推导线。
- `proof-writer`：撰写或补全 theorem、lemma、proposition、corollary 的证明，并诚实标注不可证或需弱化的命题。
- `proof-checker`：审阅证明漏洞、缺失假设、逻辑断点和反例风险，并生成 proof audit。

#### 💌 模块七：Rebuttal（3 个）
处理评审意见、回复策略和正式 rebuttal 文本，既要有力，也不能乱承诺。

- `rebuttal-review`：检查和改进已有 rebuttal 草稿，关注覆盖率、语气、证据和可提交性。
- `rebuttal-writer`：基于原文、review、问题合并和 rebuttal idea/to-do list，生成正式回复。
- `rebuttal-pipeline`：完整 rebuttal 流程，解析评审意见、拆分 concerns、制定策略、起草回复、做安全检查和 follow-up 管理。

---

## 怎么用？ 💡

### 方法一：直接引用

在 .codex 的项目目录下放置 `AGENTS.md`，AI 就会自动遵循这套协议。

### 方法二：按需调用 Skills

在对话中提到相关关键词，AI 就会触发对应的 Skill：
- "基于 NARRATIVE_REPORT 写完整论文" → 触发 `paper-writing`
- "帮我翻译这段论文" → 触发 `paper-translate`
- "润色这个段落" → 触发 `paper-refine`
- "评审这篇论文" → 触发 `paper-review`
- "基于代码生成论文框架图参考" → 触发 `paper-figure-archi`
- "核对论文里的实验数字" → 触发 `experiment-claim-audit`
- "帮我写 Rebuttal" → 触发 `rebuttal-writer`

### 方法三：分阶段确认

如果你比较谨慎，可以要求"分阶段润色"，AI 会先确认结构、再确认逻辑、最后改文字，每步都让你点头再继续。

---

## 参考与致谢 🙏

本项目参考了以下优秀仓库：

| 内容 | 参考来源 |
|:---|:---|
| AGENTS.md 工作协议 | [oh-my-codex](https://github.com/Yeachan-Heo/oh-my-codex) by Yeachan-Heo |
| 双 Agent 架构设计 | [SWE-CI](https://github.com/SKYLENAGE-AI/SWE-CI) 论文 |
| paper_skills 部分写作 Skills | [awesome-ai-research-writing](https://github.com/Leey21/awesome-ai-research-writing) by Leey21 |
| 论文模块一、二、三、六中的部分 skills / 工作流设计 | [Auto-claude-code-research-in-sleep](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep) by wanshuiyin |
| Rebuttal 相关 Skills | [Paper2Rebuttal](https://github.com/AutoLab-SAI-SJTU/Paper2Rebuttal) by AutoLab-SAI-SJTU |

感谢这些项目作者的辛勤工作！🌟

---

## 最后说两句 💬

这套东西不是要替代你写论文，而是帮你 **少走弯路、少翻车**。AI 懂规矩了，你就能更放心地让它干活。

欢迎 Star ⭐、Fork 🍴、提 Issue 💬！

---

*让 AI 成为你的好搭档，而不是你的新麻烦 🤝*
