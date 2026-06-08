# Style Extraction

## Overview

Extract complete style profile from source document(s). This is always full analysis (no depth levels).

## Extraction Pipeline

### Single Source
```
Source Document → Text Extraction → Full Analysis → De-identification → Style Profile
```

### Multi-Source
```
Source 1 ──► Profile 1 ──┐
Source 2 ──► Profile 2 ──┼──► Aggregation ──► Style Profile
Source 3 ──► Profile 3 ──┘
```

## Core Extraction Dimensions

### 1. Introduction Rhetorical Architecture (Primary)

Extract Move 1-3 structure:

**Move 1: Establish Territory**
- Field importance statements
- Prior progress summary
- Thematic review patterns

**Move 2: Establish Niche (Gap)**
- Limitation identification
- Gap type (data/method/theory/evaluation)
- Problem framing structure

**Move 3: Occupy Niche**
- Proposal introduction
- Contribution statements
- Evidence preview patterns

**Output**:
```yaml
introduction_architecture:
  move_sequence: [M1, M1, M2, M3, M3]
  move_variants: [...]
  confidence: 0.85
```

### 2. Research Status Synthesis

Extract how source paper summarizes existing work:

```yaml
research_status_synthesis:
  grouping_strategy: thematic | chronological | method_family
  citation_function:
    background: ratio
    evidence: ratio
    contrast: ratio
    gap_support: ratio
  summary_pattern: progress_to_boundary | taxonomy_to_gap | consensus_to_open
```

### 3. Gap Framing Patterns

Extract how problems are framed:

```yaml
gap_framing:
  gap_type:
    data_gap: frequency
    method_gap: frequency
    theory_gap: frequency
    evaluation_gap: frequency
  structure_pattern: prior_success -> remaining_limitation -> research_need
```

### 4. Contribution-Response Alignment

Extract how contributions map to gaps:

```yaml
contribution_alignment:
  gap_to_contribution_mapping: [...]
  evidence_preview_type:
    experiments: ratio
    theory: ratio
    framework: ratio
    dataset: ratio
```

### 5. Secondary Dimensions (Surface Style)

Voice and sentence patterns (secondary priority):

```yaml
voice_profile:
  first_person_ratio: 0.15
  third_person_ratio: 0.85
  passive_ratio: 0.60
  active_ratio: 0.40

sentence_metrics:
  median_length: 18
  mean_length: 19.5
  short_ratio: 0.20
  medium_ratio: 0.55
  long_ratio: 0.25

transition_profile:
  explicit_connector_density: 0.08
  implicit_transition_ratio: 0.45

hedging_profile:
  hedging_density: 0.12
  certainty_density: 0.08
  hedging_style: moderate
```

## De-identification Rules

All extracted patterns must be de-identified:

### Template De-identification

**Before**:
> "Our Transformer-XL achieves 94.5% on WikiText-103"

**After**:
> "The proposed [METHOD] achieves [METRIC] on [DATASET]"

**Replacement mapping**:
| Type | Placeholder |
|------|-------------|
| Method name | `[METHOD]` |
| Dataset name | `[DATASET]` |
| Metric/Value | `[METRIC]` / `[VALUE]` |
| Benchmark | `[BENCHMARK_REF]` |
| Specific numbers | Remove |
| Citation keys | Remove |

### Rhetorical Pattern Extraction

Extract by function, not content:

| Function | Template |
|------------|----------|
| Motivation | "[DOMAIN] has witnessed progress in [TASK]" |
| Gap | "Existing approaches struggle with [PROBLEM]" |
| Proposal | "This work proposes [METHOD] for [PROBLEM]" |
| Result | "[METHOD] achieves [OUTCOME] on [DATASET]" |

## What NOT to Extract

**Prohibited**:
- Specific method names (keep as `[METHOD]`)
- Dataset names (keep as `[DATASET]`)
- Numerical results
- Citation keys or author names
- Equation content
- Unique phrasings (>6 words)
- Argument sequences
- Limitation statements
- Future work content

## Output Files

Generated in `profile/`:

### style_profile.md

```markdown
# Style Profile

## Introduction Architecture
- Move sequence: ...
- Confidence: ...

## Research Status Synthesis
- Grouping strategy: ...
- Citation functions: ...

## Gap Framing
- Gap types: ...
- Structure pattern: ...

## Contribution Alignment
- Gap-to-contribution mapping: ...
- Evidence preview: ...

## Voice & Rhythm (Secondary)
- Voice preference: ...
- Sentence metrics: ...

## De-identified Templates
- Motivation: ...
- Gap: ...
- Proposal: ...
```

### style_metrics.json

Structured data for programmatic use:
```json
{
  "introduction_architecture": {...},
  "research_status_synthesis": {...},
  "gap_framing": {...},
  "contribution_alignment": {...},
  "voice": {...},
  "sentence_metrics": {...}
}
```

## Multi-Source Aggregation

When multiple sources provided:

### Aggregation Rules

**Pattern Status Classification**:

| Support | Eligible | Status | Behavior |
|---------|----------|--------|----------|
| `support >= 2` and `support * 3 >= eligible * 2` | `eligible >= 2` | `consensus` | Recommended pattern |
| `support >= 2` (not consensus) | `eligible >= 2` | `variant` | Optional variant |
| `support == 1` | any | `source_specific` | Report only, not recommended |

**Example** (3 sources):
- Pattern in 3/3 sources → `consensus`
- Pattern in 2/3 sources → `consensus` (2*3=6 >= 2*3=6)
- Pattern in 1/3 sources → `source_specific`

**Conflict Handling**:
- Conflicting patterns with no consensus → mark `no_consensus`
- Do not average conflicting styles
- Report conflicts in aggregation report

**Field-Level Aggregation**:

```yaml
pattern:
  template: "[METHOD] achieves [RESULT] on [EVALUATION_CONTEXT]"
  support_sources: 3
  eligible_sources: 3
  prevalence: 1.0
  status: consensus
```

### Aggregation Output

**style_profile.md**:
- `consensus` patterns only
- `variant` patterns (optional)
- Short `no_consensus` warnings

**aggregation_report.md**:
- Source discovery and deduplication
- Per-pattern support/eligible/prevalence/status
- Conflicts and `no_consensus` details
- Extraction failures

## Extraction Guarantee

- Always full analysis (no depth levels)
- Always de-identified (no source-specific content)
- Always abstract patterns (no sentence-level extraction)
- Multi-source: only consensus patterns recommended