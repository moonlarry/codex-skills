---
name: paper-translate
description: Translate and rewrite academic paper text across Chinese and English. Use when the user wants Chinese-to-English LaTeX translation, English-to-Chinese literal translation, or Chinese academic paragraph rewriting for Word-style manuscripts.
---

# Paper Translate

## Overview

Use this skill for cross-language transformation and Chinese academic rewriting. Keep `SKILL.md` focused on routing and load the exact template from `references/` based on the input medium and output target.

## Workflow

1. Identify the source language and target language.
2. Identify the medium:
   - LaTeX English output or LaTeX English input: load the matching LaTeX reference.
   - Chinese Word-style paragraph output: load the Word reference.
3. Preserve domain terms, formulas, numbers, and citations unless the reference explicitly says to strip or convert them.
4. Follow the output structure required by the selected reference exactly.

## Reference Routing

- For Chinese draft to English academic LaTeX: read `references/latex-en.md`.
- For English LaTeX to Chinese literal translation: read `references/latex-zh.md`.
- For Chinese draft to polished Chinese paragraph for Word: read `references/word-zh.md`.

## Output Rules

- Do not merge templates. Pick one primary reference per task.
- Keep special character escaping consistent with the destination medium.
- When the user mixes multiple goals, prioritize the explicit output format first, then style.
