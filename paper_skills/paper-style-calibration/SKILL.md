---
name: paper-style-calibration
description: Calibrate academic writing style based on external samples. Extract complete style profile from reference, diagnose target paper, and generate restructured copy. Always performs full analysis (deep) and complete pipeline (profile → diagnosis → restructure). Outputs to ./paper-style/runs/<timestamp>-<target>/.
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

Use this skill to calibrate the writing style of a target manuscript based on external style samples.

**What it does**:
1. **Profile**: Extract complete style profile from reference paper/source
2. **Diagnosis**: Analyze target paper against style profile, identify gaps
3. **Restructure**: Create a copy of target paper with style adjustments applied

**Core guarantee**: Original files are never modified. All outputs go to `./paper-style/runs/<run-id>/`.

## When to Use

- Want to make current paper match style of reference paper
- Want to apply style patterns from previous work to new paper
- Want structured diagnosis of writing style issues

## When NOT to Use

- Need formal academic polishing → use `paper-refine-special-en` first
- Need light humanization after calibration → use `paper-polish-human` after
- Only want to check style without restructuring → this skill always restructures

## Input

```
/paper-style-calibration source=<path> target=<path>
```

**Parameters**:
- `source`: Reference paper/style source (PDF/TeX/Markdown/Word)
- `target`: Target paper to calibrate (TeX/Markdown recommended)

**Format auto-detection** (no need to specify):
- `.tex` → TeX
- `.md` → Markdown
- `.docx/.doc` → Word
- `.pdf` → PDF (last resort, may lose formatting)

System will ask if format cannot be determined.

## Execution Pipeline

Fixed three-stage execution:

```
Stage 1: Profile ──► Stage 2: Diagnosis ──► Stage 3: Restructure
```

### Stage 1: Profile

Extract complete style profile from source:
- Introduction rhetorical architecture (Move 1-3)
- Research status synthesis patterns
- Gap/problem framing patterns
- Contribution-response alignment
- Voice, sentence rhythm, transitions (secondary)

**Output**: `profile/style_profile.md`, `profile/style_metrics.json`

### Stage 2: Diagnosis

Analyze target paper against style profile:
- Map target's current structure
- Identify gaps and deviations
- Generate prioritized modification plan

**Output**: `diagnosis/target_diagnosis.md`, `diagnosis/gap_analysis.json`

### Stage 3: Restructure

Create styled copy of target paper:
- Copy target to `restructured/<stem>.style-restructured<ext>`
- Apply style adjustments to copy only
- Original file untouched

**Output**: `restructured/<target>.style-restructured.*`

## Output Directory Structure

All outputs go to current working directory:

```
./paper-style/
└── runs/
    └── 20260608-153012-my_paper/          # <timestamp>-<target_slug>
        ├── manifest.json                   # Run metadata & status
        ├── profile/                        # Stage 1: Style profile
        │   ├── style_profile.md
        │   └── style_metrics.json
        ├── diagnosis/                      # Stage 2: Diagnosis
        │   ├── target_diagnosis.md
        │   └── gap_analysis.json
        └── restructured/                   # Stage 3: Styled copy
            └── my_paper.style-restructured.tex
```

**Naming**:
- `target_slug`: From target file stem, safe characters only
- Conflicts: Append `-02`, `-03` if same second
- Runs are immutable, old runs preserved

## Hard Constraints

1. **Never modify original files**
   - Target paper copied before any modification
   - Original remains unchanged

2. **Style-only changes**
   - Only modify voice, structure, rhythm, transitions
   - Preserve all factual content: claims, numbers, citations, equations

3. **No content migration**
   - Do not copy methods, arguments, or specific content from source
   - Extract only abstract, de-identified patterns

4. **Target-grounded**
   - Every change must use facts already in target paper
   - Do not introduce new claims, examples, or evidence

## Relationship with Other Skills

| Skill | Relationship |
|-------|-------------|
| `paper-refine-special-en` | Run **BEFORE** this skill for formal polishing |
| `paper-polish-human` | Run **AFTER** this skill for optional light humanization |

**Recommended pipeline**:
```
paper-refine-special-en → paper-style-calibration → paper-polish-human (optional)
```

## What This Skill Does NOT Do

- Does NOT provide `depth` parameter (always full analysis)
- Does NOT provide `mode` parameter (always full pipeline)
- Does NOT modify original files (creates copies)
- Does NOT generate output to `psmfiles/` (uses `paper-style/`)
- Does NOT require `source_kind` (auto-detects format)

## Error Handling

If any stage fails:
- Previous stages' outputs preserved
- `manifest.json` updated with `failed` or `partial` status
- Error details recorded in `manifest.json`
- Original files never touched

## Example Usage

```
# Basic usage
/paper-style-calibration source=reference.tex target=my_paper.tex

# Output will be at:
# ./paper-style/runs/20260608-153012-my_paper/
```