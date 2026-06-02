---
name: paper-polish-human
description: Apply a controlled human-style post-polish pass only after a formal full-paper polish has already been completed. Use when the user wants a finished academic paper to sound less mechanical by adding restrained natural phrasing in Introduction, Related Work or Related Works, and Results sections, with optional minor grammar imperfections only when the user explicitly enables grammar-error mode. Before editing, remind the user that paper-polish-workflow, paper-refine-special-en, or paper-refine-special-zh should be used first for formal polishing; preserve facts, citations, LaTeX syntax, formulas, and technical correctness.
---

# Paper Polish Human

## Overview

Use this skill as the final post-polish layer for a paper that has already received formal full-paper polishing. It adds controlled human-style variation after the main formal polishing work is complete.

When executing this skill, adopt the role of a human author doing a final pass on an already polished manuscript while the experiment context is still fresh. Decisions about sentence structure, paragraph rhythm, emphasis asymmetry, and natural variation should follow from that authorial posture, not from surface-level text substitution. The posture is defined in `references/human-style-policy.md` under "Role-Play Posture".

## Pre-Edit Reminder

Before modifying the user's paper, remind them:

```text
This skill is intended for use after formal full-paper polishing. It is recommended to polish the paper first with paper-polish-workflow, paper-refine-special-en, or paper-refine-special-zh, then use paper-polish-human as the final human-style pass.
```

Do not automatically invoke, route to, or execute those skills. Do not wait for confirmation after the reminder; proceed directly with `paper-polish-human` according to this skill's rules.

If the user asks for publication-ready, camera-ready, or submission-ready text, do not introduce intentional grammar imperfections unless grammar-error mode is explicitly enabled in the same request.

## Grammar-Error Mode

Intentional grammar imperfections are disabled by default. Enable them only when the user explicitly asks with phrases such as:

- "enable grammar errors"
- "turn on grammar-error mode"
- "add minor grammar errors"
- "add small grammar mistakes"
- "explicitly enable grammar errors"
- equivalent Chinese requests that explicitly ask to enable grammar errors or add small grammar mistakes

When grammar-error mode is enabled:

1. Insert only minor, recoverable imperfections that do not change technical meaning.
2. Keep the density low: normally 1 imperfection per 250-400 editable words in targeted sections, capped at 6 per full paper unless the user requests another rate.
3. Put imperfections only in Introduction, Related Work or Related Works, and Results sections.
4. Never add grammar errors to Abstract, Method, Theory, Conclusion, tables, figure captions, equations, theorem statements, algorithms, citations, bibliography, or claims involving exact numbers.
5. Log every intentional imperfection in the output.

## Workflow

1. Identify language and medium:
   - English LaTeX
   - Chinese LaTeX
   - Word-style prose
2. Detect document context (regular paper vs rebuttal/revision letter). If the document is a rebuttal, revision letter, or response to reviewers, apply REBUTTAL_DIFF_ANCHORING pattern as appropriate.
3. Give the pre-edit reminder, then proceed directly with this final human-style pass.
4. Read `references/human-style-policy.md` (including Authorial Stance Guardrails in Hard Constraints and Manuscript-Local Voice Calibration).
5. Read `references/ai-pattern-detection.md` to identify applicable AI writing patterns (apply REBUTTAL_DIFF_ANCHORING only in rebuttal/revision context).
6. Read `references/section-targeting.md` to restrict edits to eligible sections.
7. Read `references/ratio-calibration.md` to keep the approximate formal-to-natural ratio near 7:3.
8. Read `references/output-format.md` before returning the final result.
9. Apply the smallest set of changes that makes the polished paper sound less mechanical while preserving academic quality.
10. Run the Audit Pass (see below).
11. Verify that facts, formulas, citations, labels, references, numeric results, and LaTeX commands are unchanged.

## Audit Pass

Before finalizing, audit the rewrite for academic safety:

1. **Factual integrity**: No numbers, datasets, metrics, comparisons, citations, definitions, theorem statements, or claims were changed unless explicitly requested.

2. **Source integrity**: LaTeX commands, equations, citations, references, labels, BibTeX keys, code-like tokens, and range notation (`--`, `---`) are preserved. Math expressions, macros, tables, figure labels, and dash syntax are unchanged.

3. **Academic convention safety**: Passive voice, hedging, technical term reuse, section roadmapping, contribution lists, and result descriptions were not treated as AI-like by default.

4. **Voice calibration**: The rewrite matches the manuscript-local academic voice rather than a generic conversational style.

5. **Humanization safety**: No anecdotes, emotions, unsupported background, stronger conclusions, decorative dashes, or persuasive tropes were introduced.

6. **Scope control**: Only genuine AI-pattern risks were revised; stable technical content was left intact.

7. **Context/mode audit**: Document context (regular paper vs rebuttal/revision letter) was correctly detected. REBUTTAL_DIFF_ANCHORING was applied ONLY in rebuttal/revision context and disabled elsewhere.

## Hard Constraints

- Preserve all technical claims, definitions, assumptions, numeric values, comparisons, formulas, citations, labels, references, and LaTeX commands.
- Do not add new claims, new limitations, new citations, new methods, new experiments, or new interpretation.
- Do not make the prose casual, slangy, promotional, or blog-like.
- Do not apply the 3:7 ratio to the whole document blindly; apply it only to editable prose in the allowed sections.
- Do not hide intentional imperfections. When grammar-error mode is enabled, list them explicitly in the modification log.

## Output Rules

Use the structure in `references/output-format.md`. Always distinguish verified preservation from assumptions or unrun checks.

When uncertain about the intended edit strength or safety boundaries, consult `references/example-transformation.md` for illustrated examples.