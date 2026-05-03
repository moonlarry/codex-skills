---
name: paper-review
description: Review paper text or PDFs from a final-check or reviewer perspective. Use when the user wants fatal logic checks on English LaTeX, or a harsh reviewer-style report on a full paper PDF with concrete rejection risks and revision advice.
---

# Paper Review

## Overview

Use this skill for evaluation rather than rewriting. For full-paper PDF review where layout and page context matter, also use the local `pdf` skill at `C:\Users\pc\.codex\skills\pdf\SKILL.md`.

## Workflow

1. Determine review scope:
   - Paragraph or section level fatal-error scan: load `references/logic-check.md`.
   - Full paper or uploaded PDF review: load `references/reviewer-report.md` and use `pdf` workflow first if rendering or page-level inspection matters.
2. Assume the draft is already mature; only surface substantive issues.
3. Prefer concrete, decision-relevant findings over stylistic suggestions.

## Reference Routing

- English LaTeX final logic and consistency check: `references/logic-check.md`
- Reviewer-style paper report with score and revision advice: `references/reviewer-report.md`

## Review Rules

- Keep criticism specific and evidence-based.
- For PDF tasks, inspect rendered pages before finalizing claims about figures, tables, or formatting.
- If the user asks only for language polishing, do not use this skill; route to `paper-refine`.
