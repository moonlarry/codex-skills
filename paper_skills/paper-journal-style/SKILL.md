---
name: paper-journal-style
description: Check academic drafts against target-journal style and submission expectations. Use when the user mentions a target journal, asks for abstract or highlights formatting, wants title or keyword guidance, or needs cross-section consistency checks before submission.
---

# Paper Journal Style

## Overview

Use this skill for journal-specific constraints and submission-facing polish, not for full prose rewriting. Load the smallest reference that matches the user's journal and requested deliverable.

## Workflow

1. Identify the journal, manuscript section, and requested deliverable:
   - Abstract
   - Highlights
   - Title
   - Keywords
   - Whole-paper consistency check
2. Read `references/checklist.md`.
3. If the user names a supported journal, read the matching file under `references/journals/`.
4. Produce journal-facing output:
   - A requirement checklist when the user asks for compliance checking
   - A concise revised deliverable when the user asks for a title, abstract, or highlights draft
   - A consistency report when the user asks for terminology, number, or claim alignment
5. If the user also wants heavy rewriting, route the prose rewrite to `paper-refine`, `paper-refine-special-en`, or `paper-refine-special-zh` and use this skill only for the journal layer.

## Deliverable Rules

- Keep journal requirements separate from language rewrite advice.
- State clearly which items are confirmed from the provided draft and which still require author verification.
- Preserve numbers, datasets, model names, citations, and claim boundaries unless the user explicitly asks to revise them.
- When the current journal guide is not available locally, provide a conservative checklist and tell the user to verify final submission constraints against the journal's latest author instructions.

## Reference Routing

- General intake, consistency, and submission checks: `references/checklist.md`
- CEUS-specific guidance: `references/journals/ceus.md`

## Output Patterns

- Compliance check: short checklist with `pass / needs revision / missing information`
- Highlights request: 3-5 concise bullets in action-result style unless the journal file says otherwise
- Abstract check: report scope coverage, evidence-to-claim alignment, terminology stability, and likely overlength risk
- Cross-section consistency: compare title, abstract, introduction, results, conclusion, and highlights for number drift or claim drift
