# Style Application

## Overview

Apply extracted style profile to target paper through three-stage pipeline.

## Three-Stage Pipeline

```
Stage 1: Profile ──► Stage 2: Diagnosis ──► Stage 3: Restructure
```

All three stages always execute. No mode selection.

## Stage 1: Profile

Already completed by style-extraction.md.

**Input**: Source document
**Output**: `profile/style_profile.md`, `profile/style_metrics.json`

## Stage 2: Diagnosis

Analyze target paper against style profile.

### 2.1 Target Structure Mapping

Map target's current Introduction structure:

```yaml
target_current_structure:
  move_sequence: [M1, M2, M3, ...]
  paragraph_functions: [...]
  citation_usage: [...]
```

### 2.2 Gap Analysis

Compare target vs. style profile:

| Aspect | Target | Profile | Gap |
|--------|--------|---------|-----|
| Move sequence | [M1, M3] | [M1, M2, M3] | Missing M2 |
| Research synthesis | Chronological | Thematic | Strategy mismatch |
| Gap framing | Implicit | Explicit | Framing weak |
| Contribution alignment | Unclear | Clear | Missing alignment |

### 2.3 Prioritized Issues

Generate prioritized modification plan:

**High Priority**:
- Missing Move 2 (gap establishment)
- Unclear contribution-to-gap mapping
- Weak problem framing

**Medium Priority**:
- Suboptimal research synthesis grouping
- Inconsistent citation functions
- Sentence rhythm deviations

**Low Priority**:
- Voice preferences (active/passive)
- Connector density
- Paragraph length variations

### 2.4 Diagnosis Output

**diagnosis/target_diagnosis.md**:

```markdown
# Target Diagnosis Report

## Executive Summary
Target paper deviates from style profile in 3 high-priority areas.

## Current Structure
- Move sequence: M1 → M3 (M2 missing)
- Research synthesis: Chronological grouping
- Gap framing: Implicit
- Contribution alignment: Unclear

## Gaps Identified

### High Priority
1. **Missing Move 2**: No explicit gap establishment
   - Current: "Many methods exist..."
   - Expected: "However, existing methods struggle with..."
   - Location: Paragraph 2

2. **Weak Contribution Alignment**: Contribution not mapped to specific gap
   - Current: "We propose X..."
   - Expected: "To address [GAP], we propose X..."

### Medium Priority
3. **Suboptimal Research Synthesis**: Chronological vs. thematic
   - Current: "A (2020) did... B (2021) did..."
   - Expected: "Recent approaches fall into two categories..."

### Low Priority
4. **Voice Preference**: Active vs. passive ratio
5. **Connector Density**: Higher than profile

## Modification Plan

### Paragraph 2 (Restructure)
- Add explicit gap statement
- Connect to prior work limitations
- Preview research need

### Paragraph 3 (Enhance)
- Strengthen contribution-to-gap mapping
- Add evidence preview

## Risks
- Target paper's technical claims must be preserved
- Do not add new claims or evidence
- Only restructure existing content
```

**diagnosis/gap_analysis.json**:

```json
{
  "target_structure": {...},
  "gaps": [
    {"severity": "high", "type": "missing_move", "move": "M2", "location": "para_2"},
    {"severity": "high", "type": "alignment_weak", "element": "contribution_gap"}
  ],
  "modification_plan": [
    {"action": "restructure", "paragraph": 2, "changes": [...]},
    {"action": "enhance", "paragraph": 3, "changes": [...]}
  ]
}
```

## Stage 3: Restructure

Create styled copy of target paper.

### 3.1 File Copy Strategy

**Single file target**:
```
my_paper.tex ──► restructured/my_paper.style-restructured.tex
```

**Project directory target**:
```
project/
  ├── main.tex
  ├── sections/
  └── figures/
  
──► restructured/
      ├── main.style-restructured.tex
      ├── sections/
      └── figures/
```

**Copy rules**:
- Copy to `restructured/` before any modification
- Original file untouched
- Preserve all relative paths
- Exclude `.git/`, `paper-style/`

### 3.2 Modification Scope

**Default scope** (prose sections):
- Introduction
- Related Work / Related Works
- Results (interpretive prose)
- Discussion

**Excluded** (technical precision):
- Abstract
- Method
- Theory / Preliminaries
- Experiments Setup
- Conclusion
- Limitations

User can override in natural language.

### 3.3 Modification Types

**Type 1: Restructure** (major)
- Reorganize paragraph flow
- Add missing Move sections
- Strengthen contribution-gap alignment

**Type 2: Enhance** (medium)
- Strengthen gap framing
- Improve research synthesis grouping
- Adjust contribution statement

**Type 3: Refine** (minor)
- Adjust voice (active/passive)
- Modify sentence rhythm
- Tune connector density

### 3.4 Modification Priority

Apply in order:
1. **Structure first**: Move sequence, paragraph organization
2. **Content second**: Gap framing, contribution alignment
3. **Style last**: Voice, rhythm, connectors

### 3.5 Safety Constraints

**Before any change**:
- Does this modify factual content? → BLOCK
- Does this add new claims? → BLOCK
- Does this modify numbers/citations/equations? → BLOCK

**After changes**:
- Verify all claims preserved
- Verify all numbers unchanged
- Verify all citations intact

### 3.6 Restructure Output

**File**: `restructured/<stem>.style-restructured<ext>`

**Content**: Modified copy with style adjustments applied.

## Execution Order

```
1. Profile (from source)
   └── profile/style_profile.md
   └── profile/style_metrics.json

2. Diagnosis (target vs profile)
   └── diagnosis/target_diagnosis.md
   └── diagnosis/gap_analysis.json

3. Restructure (copy and modify)
   └── restructured/<target>.style-restructured.*

4. Manifest (record all)
   └── manifest.json
```

All stages always execute. No user selection.