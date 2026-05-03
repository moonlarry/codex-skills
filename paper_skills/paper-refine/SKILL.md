---
name: paper-refine
description: Refine academic writing at the paragraph level. Use when the user wants English LaTeX polishing, Chinese paper polishing, shortening, expansion, or de-AI rewriting for LaTeX or Word-ready text.
---

# Paper Refine

## Overview

Use this skill for local rewriting rather than full-paper evaluation. Pick the smallest reference that matches the user's editing intent so the response stays stable and the trigger remains precise.

## Workflow

1. Determine whether the request is about shortening, expanding, polishing, or de-AI rewriting.
2. Determine whether the source is English LaTeX or Chinese Word-style text.
3. Load exactly one reference unless the user explicitly asks for combined operations.
4. Preserve technical facts, formulas, numeric results, and citations unless the chosen reference explicitly allows restructuring.

## Reference Routing

- English LaTeX shortening, expansion, or full language polish: `references/latex-refine.md`
- Chinese paper polishing for Word-style manuscripts: `references/word-refine.md`
- De-AI rewriting for English LaTeX or Chinese Word text: `references/de-ai.md`

## Editing Rules

- Prefer necessary edits over broad paraphrase.
- Keep the user's original argument and evidence intact.
- If the user asks for both polish and de-AI rewriting, treat de-AI as a threshold check: keep good text unchanged.
