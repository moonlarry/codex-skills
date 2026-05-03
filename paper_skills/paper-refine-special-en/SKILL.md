---
name: paper-refine-special-en
description: Perform high-intensity global polishing for English academic papers in LaTeX. Use when the user wants section-level or document-level rewriting with global logic checking, outline reconstruction, compression, sentence-level polishing, and a final reviewer-style pass, or when the user explicitly asks for a staged structure-to-logic-to-expression English polish workflow with confirmation checkpoints.
---

# Paper Refine Special EN

## Overview

Use this skill for heavy English academic rewriting when local paragraph polish is not enough. It is designed for section-level and document-level improvement, not only for isolated sentences.

## Workflow

1. Determine the scope of the input:
   - Single paragraph
   - Multi-paragraph subsection
   - Full section
   - Full paper
2. Detect whether the user wants:
   - Direct heavy rewrite
   - Staged confirmation workflow with structure -> sentence logic -> expression
3. Read `references/core-workflow.md` and follow the five-phase pipeline exactly.
4. Read `references/global-logic-check.md` to evaluate paragraph roles, section progression, and document-level evidence chains.
5. If the content contains experiments, ablations, efficiency studies, or case analysis, also read `references/experiment-structure.md` and enforce that structure in the final text.
6. Read `references/output-format.md` to choose the correct output shape for paragraph, section, or document inputs.

## Staged Workflow Mode

If the user explicitly asks for incremental confirmation, do not jump straight to a full rewrite.

1. Confirm macro structure first.
2. Map sentence or paragraph roles before editing wording.
3. Offer 2-4 English expression options for a small batch of sentences when useful.
4. After the logic is confirmed, produce the rewritten passage.
5. If the user also names a target journal or asks for highlights, use `paper-journal-style` for the journal-specific layer.

## Scope Rules

- This skill is stronger than `paper-refine` and should be used only when the user wants global restructuring and high-intensity polishing.
- Keep LaTeX commands, formulas, citations, labels, and escaped characters intact unless a change is necessary for correctness.
- For multi-paragraph and longer inputs, do not patch sentence by sentence from the original order. Rebuild the outline first, then rewrite from that outline.
- In staged workflow mode, keep each checkpoint compact and wait for confirmation before moving from structure to wording when the user asked for that interaction pattern.

## Boundary With Other Skills

- Use `paper-refine` for light paragraph-level editing.
- Use this skill for section-level or paper-level global improvement.
- Use `paper-review` for standalone reviewer reports or PDF-focused evaluation rather than final rewritten prose.
