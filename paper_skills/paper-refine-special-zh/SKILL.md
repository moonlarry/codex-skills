---
name: paper-refine-special-zh
description: Perform high-intensity global polishing for Chinese academic writing in either Word-style plain text or Chinese LaTeX. Use when the user wants section-level or document-level rewriting with global logic checking, outline reconstruction, compression, sentence-level polishing, and a final reviewer-style pass, or when the user explicitly asks for a staged structure-to-logic-to-expression Chinese polish workflow with confirmation checkpoints.
---

# Paper Refine Special ZH

## Overview

Use this skill for heavy Chinese academic rewriting when ordinary paragraph polish is insufficient. It supports both Word-ready Chinese prose and Chinese LaTeX while keeping the same global polishing workflow.

## Workflow

1. Determine the scope of the input:
   - Single paragraph
   - Multi-paragraph subsection
   - Full section
   - Full paper
2. Determine the medium:
   - Word-style plain Chinese text
   - Chinese LaTeX with commands, formulas, or references
3. Detect whether the user wants:
   - Direct heavy rewrite
   - Staged confirmation workflow with structure -> sentence logic -> expression
4. Read `references/core-workflow.md` and follow the five-phase pipeline exactly.
5. Read `references/global-logic-check.md` to evaluate paragraph roles, section progression, and whole-document consistency.
6. If the content contains experiments, ablations, efficiency studies, or case analysis, also read `references/experiment-structure.md` and enforce that structure in the final text.
7. Read `references/output-format.md` to choose the correct output format for the scope and medium.

## Staged Workflow Mode

If the user explicitly asks for step-by-step confirmation, keep the rewrite interactive.

1. Confirm macro structure first.
2. Confirm sentence or paragraph logic before rewriting wording.
3. Offer 2-4 Chinese expression options for a small batch when that helps the user choose tone or density.
4. Assemble the rewritten passage only after logic is confirmed.
5. If the user also names a target journal or asks for highlights, use `paper-journal-style` for journal-specific constraints.

## Medium Rules

- In Word mode, output pure Chinese text with full-width punctuation and no Markdown markers.
- In Chinese LaTeX mode, preserve LaTeX commands, formulas, citations, and required escaping.
- Do not accidentally flatten Chinese LaTeX into plain text.
- In staged workflow mode, keep each checkpoint short and wait for user confirmation when the user requested that interaction pattern.

## Boundary With Other Skills

- Use `paper-refine` for light local polishing.
- Use this skill for section-level or paper-level restructuring and full polishing.
- Use `paper-review` when the user wants a standalone review report rather than a rewritten final draft.
