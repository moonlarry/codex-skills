# Codex Skills Collection 🚀

> 让 AI 成为你的学术写作助手，而不是冷冰冰的代码机器 ✨

## 这个仓库有什么用？ 🤔

这个仓库源于我使用 Codex 的日常需求。写论文时总会遇到各种挑战：翻译卡壳、润色纠结、图表设计头大、Rebuttal 等。于是我在过程中参考了多个优秀项目，不断总结修改，最终凝练成一套 **AI Agent 工作协议 + 学术写作 Skills**，让 Codex 更懂学术写作的"规矩"：

- 📝 翻译论文时保持学术风格，不跑偏
- 🎨 润色段落时只改必要的地方，不改掉你的观点
- 📊 设计图表时遵循学术规范，不花里胡哨
- 📋 写 Rebuttal 时既礼貌又有力，有理有节
- 🔍 结合已有报告，针对论文提出修改意见

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

执行任务时默认在三种场景中选择，每种场景都有 **架构师 + 执行者** 双角色分工，避免一个Agent既规划又干活容易翻车，同时Agent在执行任务时可调用对应的 skills 完成任务。

---

### 2. paper_skills 文件夹 — 学术写作 Skills 📚

包含 **11 个专门针对学术论文的 Skills**，每个聚焦一个具体场景（持续更新）：

#### 🔄 paper-translate
中英双向翻译（中文→英文 LaTeX、英文 LaTeX→中文）及中文 Word 风格学术重写。

#### ✨ paper-refine
段落级润色（缩短/扩展/润色）与去 AI 感重写，支持英文 LaTeX 和中文 Word。

#### 🔍 paper-review
论文评审检查：致命逻辑问题扫描与评审人风格拒稿风险评估报告。

#### 📈 paper-figures
图表设计助手：架构图 Prompt 生成、实验图表类型推荐、图表标题撰写。

#### 📊 paper-analysis
实验结果分析：数据→LaTeX 分析段落生成与模型选择建议。

#### 🔥 paper-refine-special-en
高强度英文润色：先凝练论文逻辑，然后段落/章节/全文级重写、全局逻辑检查、结构重建、分阶段确认。

#### 🔥 paper-refine-special-zh
高强度中文润色：先凝练论文逻辑，然后Word/LaTeX 全文重写、全局逻辑检查、结构重建、分阶段确认。

#### 📋 paper-journal-style
期刊投稿检查：Abstract/Highlights/Title 格式、跨章节一致性、期刊特定要求。配合 [Awesome-llm-paper-wiki](https://github.com/moonlarry/awesome-llm-paper-wiki) 中的期刊报告功能和期刊论文 markdown 使用更佳。

#### 🎯 paper-polish-workflow
交互式润色流程：结构→逻辑→表达→一致性，分步确认后逐步推进。

#### 💌 paper-rebuttal-review
Rebuttal 信评审：检查草稿合格性，提出改进建议。

#### 💌 paper-rebuttal-writer
Rebuttal 信撰写：基于策略和待做事项，生成正式、礼貌、有力的 Rebuttal。

---

## 怎么用？ 💡

### 方法一：直接引用

在 .codex 的项目目录下放置 `AGENTS.md`，AI 就会自动遵循这套协议。

### 方法二：按需调用 Skills

在对话中提到相关关键词，AI 就会触发对应的 Skill：
- "帮我翻译这段论文" → 触发 `paper-translate`
- "润色这个段落" → 触发 `paper-refine`
- "评审这篇论文" → 触发 `paper-review`
- "帮我写 Rebuttal" → 触发 `paper-rebuttal-writer`

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
| Rebuttal 相关 Skills | [Paper2Rebuttal](https://github.com/AutoLab-SAI-SJTU/Paper2Rebuttal) by AutoLab-SAI-SJTU |

感谢这些项目作者的辛勤工作！🌟

---

## 最后说两句 💬

这套东西不是要替代你写论文，而是帮你 **少走弯路、少翻车**。AI 懂规矩了，你就能更放心地让它干活。

欢迎 Star ⭐、Fork 🍴、提 Issue 💬！

---

*让 AI 成为你的好搭档，而不是你的新麻烦 🤝*