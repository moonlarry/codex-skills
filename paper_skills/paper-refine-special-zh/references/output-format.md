# Output Format

Choose the output based on the scope of the input.

## Single Paragraph Input

Use:

- Part 1 [Polish Plan]
- Part 2 [Refined Text]
- Part 3 [Review Comments]

`Polish Plan` should briefly state:

- the core logic issue if any
- the compression strategy
- the sentence-level polishing focus

## Multi-Paragraph Subsection, Section, Or Full Paper

Use:

- Part 1 [Section Outline] or [Document Outline]
- Part 2 [Refined Text]
- Part 3 [Review Comments]

## Medium-Specific Rules

- In Word mode, `Refined Text` must be pure text with full-width punctuation and no Markdown.
- In Chinese LaTeX mode, `Refined Text` must preserve LaTeX commands, formulas, citations, and escaping.

## Review Comments Requirements

Always summarize:

- 整体逻辑重排
- 压缩处理
- 逐句润色
- 审稿人视角终检修正

Do not expose raw intermediate drafts unless the user explicitly asks for them.
