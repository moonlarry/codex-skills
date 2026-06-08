# Output Format

## Output Directory

All outputs to current working directory:

```
./paper-style/
└── runs/
    └── <timestamp>-<target_slug>/
```

**Naming**:
- `timestamp`: YYYYMMDD-HHMMSS
- `target_slug`: From target file stem
- Conflicts: Append `-02`, `-03`

## Directory Structure

```
./paper-style/
└── runs/
    └── 20260608-153012-my_paper/
        ├── manifest.json              # Run metadata
        ├── profile/                   # Stage 1: Profile
        │   ├── style_profile.md
        │   └── style_metrics.json
        ├── diagnosis/                 # Stage 2: Diagnosis
        │   ├── target_diagnosis.md
        │   └── gap_analysis.json
        └── restructured/              # Stage 3: Restructure
            └── my_paper.style-restructured.tex
```

## manifest.json

Run metadata and status:

```json
{
  "schema_version": "1.0",
  "run_id": "20260608-153012-my_paper",
  "status": "completed",
  "started_at": "2024-06-08T15:30:12Z",
  "completed_at": "2024-06-08T15:32:45Z",
  "inputs": {
    "source_path": "reference.tex",
    "target_path": "my_paper.tex",
    "source_format": "tex",
    "target_format": "tex"
  },
  "integrity": {
    "source_hash": "sha256:...",
    "target_hash_before": "sha256:...",
    "target_hash_after": "sha256:...",
    "original_target_unchanged": true
  },
  "phases": {
    "profile": {
      "status": "completed",
      "artifacts": [
        "profile/style_profile.md",
        "profile/style_metrics.json"
      ]
    },
    "diagnosis": {
      "status": "completed",
      "artifacts": [
        "diagnosis/target_diagnosis.md",
        "diagnosis/gap_analysis.json"
      ]
    },
    "restructure": {
      "status": "completed",
      "entrypoint": "restructured/my_paper.style-restructured.tex",
      "modified_files": ["my_paper.style-restructured.tex"]
    }
  },
  "warnings": [],
  "errors": []
}
```

**Status values**: `completed`, `failed`, `partial`

## Stage 1: Profile Outputs

### profile/style_profile.md

Human-readable style guide:

```markdown
# Style Profile

## Introduction Architecture
- Move sequence: M1 → M1 → M2 → M3 → M3
- Confidence: 0.85

## Research Status Synthesis
- Grouping strategy: thematic
- Citation functions: background (40%), evidence (35%), gap_support (25%)

## Gap Framing
- Gap types: method_gap (60%), evaluation_gap (40%)
- Structure: prior_success → remaining_limitation → research_need

## Contribution Alignment
- Gap-to-contribution mapping: clear
- Evidence preview: experiments

## Voice & Rhythm
- Voice preference: third-person passive
- Median sentence length: 18 words

## De-identified Templates
- Motivation: "[DOMAIN] has witnessed progress in [TASK]"
- Gap: "Existing approaches struggle with [PROBLEM]"
- Proposal: "This work proposes [METHOD] for [PROBLEM]"
```

### profile/style_metrics.json

Structured data:

```json
{
  "introduction_architecture": {
    "move_sequence": ["M1", "M1", "M2", "M3", "M3"],
    "confidence": 0.85
  },
  "research_status_synthesis": {
    "grouping_strategy": "thematic",
    "citation_functions": {
      "background": 0.40,
      "evidence": 0.35,
      "gap_support": 0.25
    }
  },
  "gap_framing": {
    "gap_types": {
      "method_gap": 0.60,
      "evaluation_gap": 0.40
    }
  },
  "contribution_alignment": {
    "mapping_clarity": "clear",
    "evidence_preview": "experiments"
  },
  "voice": {
    "third_person_ratio": 0.85,
    "passive_ratio": 0.60
  },
  "sentence_metrics": {
    "median_length": 18,
    "mean_length": 19.5
  }
}
```

## Stage 2: Diagnosis Outputs

### diagnosis/target_diagnosis.md

Human-readable diagnosis report:

```markdown
# Target Diagnosis Report

## Executive Summary
3 high-priority gaps identified.

## Current Structure
- Move sequence: M1 → M3 (M2 missing)
- Research synthesis: chronological
- Gap framing: implicit

## Gaps Identified

### High Priority
1. **Missing Move 2**: No explicit gap establishment
   - Location: Paragraph 2
   - Expected: "However, existing methods..."

2. **Weak Contribution Alignment**: Unclear gap-to-contribution mapping

### Medium Priority
3. **Suboptimal Research Synthesis**: Chronological vs thematic

## Modification Plan
- Paragraph 2: Add explicit gap statement
- Paragraph 3: Strengthen contribution alignment
```

### diagnosis/gap_analysis.json

Structured gap data:

```json
{
  "target_structure": {
    "move_sequence": ["M1", "M3"],
    "paragraph_count": 5
  },
  "gaps": [
    {
      "severity": "high",
      "type": "missing_move",
      "move": "M2",
      "location": "paragraph_2"
    },
    {
      "severity": "high",
      "type": "alignment_weak",
      "element": "contribution_gap"
    }
  ],
  "modification_plan": [
    {
      "action": "restructure",
      "paragraph": 2,
      "changes": ["add_gap_statement", "connect_prior_work"]
    }
  ]
}
```

## Stage 3: Restructure Output

### restructured/<stem>.style-restructured<ext>

Styled copy of target paper.

**Naming**:
- Single file: `my_paper.tex` → `my_paper.style-restructured.tex`
- Project: copy entire project to `restructured/`

**Content**: Modified copy with style adjustments applied.

**Guarantee**: Original file untouched.

## Standard Output

Console output after execution:

```
Paper Style Calibration Complete

Run ID: 20260608-153012-my_paper
Status: completed

Stages:
  ✓ Profile:    profile/style_profile.md
  ✓ Diagnosis:  diagnosis/target_diagnosis.md
  ✓ Restructure: restructured/my_paper.style-restructured.tex

Output location:
  ./paper-style/runs/20260608-153012-my_paper/

Original file:
  Unchanged: my_paper.tex

Next steps:
  - Review diagnosis/target_diagnosis.md
  - Check restructured/my_paper.style-restructured.tex
  - Optional: Run paper-polish-human for light humanization
```

## Error Output

If stage fails:

```
Paper Style Calibration Partial

Run ID: 20260608-153012-my_paper
Status: partial

Stages:
  ✓ Profile:    profile/style_profile.md
  ✓ Diagnosis:  diagnosis/target_diagnosis.md
  ✗ Restructure: FAILED

Error: [description]

Completed outputs preserved at:
  ./paper-style/runs/20260608-153012-my_paper/

Original file:
  Unchanged: my_paper.tex
```

## Medium-Specific Rules

### For LaTeX

- Preserve all commands, environments, math
- Valid LaTeX output

### For Markdown

- Clean markdown output
- Headers preserved
- Links preserved

## Multi-Source Output

When `sources=` (multiple sources) used:

```
profile/
├── style_profile.md              # Aggregated style (consensus + variants)
├── aggregation_report.md         # Aggregation process report
└── source_individual/            # Per-source summaries (optional)
    ├── source_1_summary.md
    └── source_2_summary.md
```

### profile/aggregation_report.md

```markdown
# Aggregation Report

## Source Discovery
- Discovered: 3 papers
- Deduplicated: 3 papers (0 removed)
- Eligible: 3 papers

## Per-Pattern Analysis

### Pattern: result_summary
- Template: "[METHOD] achieves [RESULT] on [EVALUATION_CONTEXT]"
- Support: 3/3
- Prevalence: 1.0
- Status: consensus

### Pattern: gap_framing
- Template: "However, existing approaches struggle with [PROBLEM]"
- Support: 2/3
- Prevalence: 0.67
- Status: consensus

### Pattern: unique_to_source_3
- Template: "In this groundbreaking work..."
- Support: 1/3
- Prevalence: 0.33
- Status: source_specific
- Action: filtered out

## Conflicts

### Sentence Length
- Status: no_consensus
- Variant A (concise): 2/3 sources
- Variant B (elaborate): 1/3 sources
- Recommendation: choose based on target content

## Deduplication
- Path deduplication: 0 files
- Content hash deduplication: 0 files
```

## No psmfiles/

**Old location**: `psmfiles/` - **Deprecated**

**New location**: `./paper-style/runs/<run-id>/`

All outputs go to new location. No backward compatibility mode.