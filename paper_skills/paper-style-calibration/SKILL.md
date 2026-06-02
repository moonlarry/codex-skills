---
name: paper-style-calibration
description: Calibrate academic writing style based on external samples (user's previous papers, reference papers, or style descriptions). Extract style patterns and apply them to the target manuscript while preserving all factual content. Use when the user wants their paper to match a specific style from reference materials. Supports PDF, TeX, Markdown, and Word inputs. Prefer editable formats (TeX, Markdown, Word) over PDF when available.
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
---

# Paper Style Calibration

## Overview

Use this skill to calibrate the writing style of a target manuscript based on external style samples. This is NOT content generation or formal polishing—it is style alignment after the paper's substance is already written.

**When to use**:
- User has a previous paper and wants the current paper to match its style
- User wants to imitate the style of a reference paper (PDF/TeX/Markdown/Word)
- User provides a style description (e.g., "prefer third-person passive, short sentences")
- User has an existing style guide file

**When NOT to use**:
- The paper needs formal academic polishing → use `paper-refine-special-en` or `paper-polish-workflow`
- The paper needs human-style post-polish → use `paper-polish-human`
- The paper needs content generation → use `paper-write` or `paper-gen`

## Pre-Edit Reminder

Before starting, remind the user:

```text
This skill calibrates writing style based on external samples. For best results:
1. Prefer editable formats (TeX, Markdown, Word) over PDF—they preserve structure and formatting.
2. If only PDF is available, the skill will extract text, but some formatting may be lost.
3. Run formal polishing (paper-refine-special-en) BEFORE style calibration for best results.
```

Proceed directly after the reminder.

## Input Format Priority

When the user provides style source files, prioritize in this order:

| Priority | Format | Notes |
|:--------:|--------|-------|
| 1 (Best) | **TeX (.tex)** | Preserves LaTeX commands, structure, math notation |
| 2 | **Markdown (.md)** | Clean text, easy to analyze |
| 3 | **Word (.docx/.doc)** | Requires conversion; structure preserved |
| 4 | **PDF (.pdf)** | Last resort; text extraction may lose formatting |

If the user provides multiple formats, ask which to use or select the highest priority format automatically.

## Workflow

1. **Identify input type**:
   - `source=path/to/file`: Style source file (PDF/TeX/Markdown/Word)
   - `source_kind=own|reference`: Whether it's user's own paper or a reference paper
   - `description="..."`: Text-based style description
   - `guide=path/to/STYLE_GUIDE.md`: Pre-existing style guide
   - `target=path/to/file`: Target manuscript to calibrate

2. **Give the pre-edit reminder**, then proceed.

3. **Read style guardrails**: `references/style-guardrails.md` (CRITICAL—defines what can/cannot be migrated).

4. **Extract or load style profile**:
   - If `source=` provided: Read `references/style-extraction.md` and extract style.
   - If `description=` provided: Read `references/intake-and-sources.md` and parse description.
   - If `guide=` provided: Load the existing style guide directly.

5. **Plan style application**: Read `references/style-application.md` to determine which sections and elements to calibrate.

6. **Apply style changes**: Modify only allowed elements (voice, sentence rhythm, transitions, formatting) while preserving all factual content.

7. **Run guardrail check**: Verify no prohibited content was migrated.

8. **Generate outputs**: Follow `references/output-format.md`.

## Hard Constraints

- **Preserve all factual content**: No changes to claims, numbers, citations, equations, definitions, experimental results, or technical terminology.
- **No content migration**: Do not copy methods, arguments, framing, or any substantive content from the style source.
- **Style-only changes**: Only modify voice, sentence rhythm, transition style, paragraph structure, and formatting conventions.
- **Target-grounded**: Every rewritten sentence must contain only facts already present in the target manuscript.
- **Hedging safety**: Adjust hedging only when evidence strength remains unchanged; never strengthen claims beyond evidence.

## Output Rules

Use the structure in `references/output-format.md`. Generate style profile files in `psmfiles/` directory.

## Relationship with Other Skills

| Skill | Relationship |
|-------|-------------|
| `paper-refine-special-en` | Run BEFORE style calibration for formal academic polishing |
| `paper-polish-human` | Run AFTER style calibration for optional light humanization |
| `paper_style_mimic` | Provides extraction mechanism; this skill is the workflow layer |

**Important**: This skill is NOT a replacement for formal academic polishing. It only calibrates style based on external samples—it does not fix logic, structure, or academic expression. Always run `paper-refine-special-en` or `paper-polish-workflow` BEFORE style calibration for best results.

**Recommended pipeline**:
```
paper-refine-special-en → paper-style-calibration → paper-polish-human (optional)
```

- `paper-refine-special-en`: Fixes formal academic expression, logic, and structure
- `paper-style-calibration`: Aligns writing style to external sample (style-only, no content changes)
- `paper-polish-human`: Optional light humanization (must respect style guide constraints)

## Source Kind Implications

| source_kind | Extraction Depth | Restrictions |
|-------------|-----------------|--------------|
| `own` | Full style profile allowed | Still cannot migrate claims/results |
| `reference` | Only abstract rhetorical patterns | Maximum guardrails; no sentence-level patterns from reference |

**Default safety policy**: If `source_kind` is not explicitly declared, treat as `reference` (most restrictive). This prevents accidental content migration from unspecified sources.

When `source_kind=reference` (or defaulted), apply extra caution:
- Extract only high-level patterns (voice preference, average sentence length, transition density)
- Do NOT extract specific phrasings or sentence templates
- Do NOT retain any content-bearing patterns from the reference paper

Explicit declaration required:
- Use `source_kind=own` only when the source is the user's own previous paper
- Use `source_kind=reference` when the source is any external paper