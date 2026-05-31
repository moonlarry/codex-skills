---
name: rebuttal-review
description: Review and improve a formal academic rebuttal letter draft for peer review responses.
---

# Rebuttal Review

Use this skill to check an existing rebuttal draft against the original reviews, merged questions, paper evidence, and author strategy.

## Workflow

1. Read the draft rebuttal, raw reviews, merged questions, and strategy notes.
2. Check coverage: every reviewer concern must be answered, intentionally deferred, or marked as needing user input.
3. Check grounding: every number, citation, experiment, theorem claim, or promised change must come from supplied evidence.
4. Check tone: responses should be respectful, precise, and not argumentative.
5. Revise only the rebuttal text needed to fix coverage, grounding, clarity, or tone problems.
6. Preserve unresolved evidence gaps as `[TBD]` rather than inventing results.

## Output Rules

- Return the improved rebuttal text plus a short change log when the user asks for review feedback.
- Do not add experiments, numbers, citations, or commitments that are not supported by the supplied materials.
- Keep reviewer-specific answers traceable to the corresponding reviewer concern.
