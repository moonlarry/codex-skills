---
name: paper-polish-workflow
description: Run an interactive academic-paper polishing workflow with explicit checkpoints. Use when the user asks to polish step by step, confirm structure before rewriting, revise sentence logic incrementally, compare multiple expression options, or do a gradual abstract or section refinement instead of an immediate full rewrite.
---

# Paper Polish Workflow

## Overview

Use this skill as the orchestration layer for interactive paper polishing. It controls the order of analysis and confirmation, then routes actual heavy rewriting to `paper-refine`, `paper-refine-special-en`, or `paper-refine-special-zh` when needed.

## Workflow

1. Confirm scope and medium:
   - Abstract
   - Paragraph
   - Subsection
   - Full section
   - English LaTeX
   - Chinese LaTeX
   - Word-style prose
2. Read `references/staged-workflow.md`.
3. Run the workflow in order:
   - Macro structure
   - Sentence or paragraph logic
   - Expression options
   - Coherence and repetition pass
4. Keep each checkpoint short and wait for the user's response before moving to the next stage when the user asked for an interactive process.
5. If the user asks for target-journal constraints, highlights, or title checks, also use `paper-journal-style`.
6. If the user asks for a direct full rewrite instead of staged interaction, route to the matching `paper-refine*` skill instead of staying in this workflow.

## Routing Rules

- Use `paper-refine` for light local polishing after logic is fixed.
- Use `paper-refine-special-en` for heavy English rewriting after staged confirmation.
- Use `paper-refine-special-zh` for heavy Chinese rewriting after staged confirmation.
- Use `paper-review` if the user wants critique rather than rewriting.

## Output Rules

- Do not dump a full rewritten section before structure and logic are confirmed when the user asked for staged polishing.
- Offer 2-4 expression options only when wording choice is the actual decision point.
- Batch a few sentences together when that reduces back-and-forth without losing clarity.
- Separate logic disagreements from wording disagreements.
