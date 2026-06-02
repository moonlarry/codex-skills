# Style Extraction

## Overview

Extract writing style patterns from source documents. This document defines what to extract and how to process it safely.

## Extraction Pipeline

```
Source Document → Text Extraction → Style Analysis → De-identification → Style Profile
```

## What to Extract

### 1. Voice and Agency Patterns

**Extract**:
- First-person vs. third-person ratio
- Active vs. passive voice ratio
- Agency attribution patterns ("We observe" vs. "It was observed")

**Output format**:
```yaml
voice_profile:
  first_person_ratio: 0.15
  third_person_ratio: 0.85
  passive_ratio: 0.60
  active_ratio: 0.40
  preferred_pattern: "third-person passive"
```

### 2. Sentence Length Distribution

**Extract**:
- Word count per sentence for each section type
- Median, mean, standard deviation
- Short/medium/long sentence ratios

**Output format**:
```yaml
sentence_metrics:
  introduction:
    median: 18
    mean: 19.5
    std: 4.2
    short_ratio: 0.20  # <12 words
    medium_ratio: 0.55 # 12-25 words
    long_ratio: 0.25   # >25 words
  methods:
    median: 22
    mean: 24.1
    std: 6.8
  results:
    median: 16
    mean: 17.2
    std: 3.5
```

### 3. Transition and Connector Patterns

**Extract**:
- Frequency of explicit connectors (However, Furthermore, Moreover, Additionally, Therefore, Thus, Specifically)
- Implicit transition patterns (direct continuation, juxtaposition)
- Paragraph opening patterns

**Output format**:
```yaml
transition_profile:
  explicit_connector_density: 0.08  # per sentence
  top_connectors:
    - "However": 0.25
    - "Furthermore": 0.15
    - "Therefore": 0.10
  implicit_transition_ratio: 0.45
  paragraph_opening_patterns:
    - "connector_first": 0.30
    - "subject_first": 0.40
    - "observation_first": 0.30
```

### 4. Hedging and Certainty Patterns

**Extract**:
- Hedging verb frequency (suggest, indicate, may, might, appear, seem)
- Certainty verb frequency (demonstrate, confirm, prove, establish)
- Qualifier frequency (potentially, possibly, likely, clearly, obviously)

**Output format**:
```yaml
hedging_profile:
  hedging_density: 0.12
  certainty_density: 0.08
  common_hedges:
    - "suggests": 0.30
    - "indicates": 0.25
    - "may": 0.20
  common_certainties:
    - "demonstrates": 0.40
    - "shows": 0.35
  hedging_style: "moderate"  # light | moderate | heavy
```

### 5. Paragraph Structure Patterns

**Extract**:
- Average paragraph length (sentences per paragraph)
- Topic sentence position (first, second, embedded)
- Concluding sentence presence
- Evidence-to-claim ratio

**Output format**:
```yaml
paragraph_profile:
  avg_sentences_per_paragraph:
    introduction: 5.2
    methods: 4.8
    results: 4.1
  topic_sentence_position:
    first: 0.70
    second: 0.20
    embedded: 0.10
  concluding_sentence_presence: 0.40
```

### 6. Formatting Conventions

**Extract**:
- Figure reference style ("Fig. 1" vs "Figure 1")
- Table reference style ("Table 1" vs "Tbl. 1")
- Citation style ("[1]" vs "(Author, Year)")
- Math notation patterns

**Output format**:
```yaml
formatting_profile:
  figure_reference: "Fig. N"
  table_reference: "Table N"
  citation_style: "numeric"  # numeric | author_year
  inline_math_style: "$...$"
  display_math_style: "\\begin{equation}"
```

## What NOT to Extract

**Prohibited extraction items**:
- Specific method names or algorithm names
- Dataset names or benchmark names
- Metric names with specific values
- Numerical results or statistics
- Citation keys or author names
- Equation content or formulas
- Claim structures with specific content
- Unique phrasings or long n-grams (>6 words)
- Argument sequences or proof structures
- Limitation or future work content
- Any content-bearing rhetorical patterns

## De-identification Process

All extracted patterns must be de-identified before storage:

### Sentence Template De-identification

**Before**:
> "Our Transformer-XL model achieves 94.5% accuracy on WikiText-103, surpassing the previous SOTA of 89.2%."

**After de-identification**:
> "The proposed [METHOD] model achieves [METRIC] on [DATASET], surpassing the previous [BENCHMARK_REF]."

**Rules**:
1. Replace method names with `[METHOD]`
2. Replace dataset names with `[DATASET]`
3. Replace specific metrics/values with `[METRIC]` or `[VALUE]`
4. Replace benchmark references with `[BENCHMARK_REF]`
5. Remove all numerical values
6. Remove all citation keys
7. Preserve rhetorical structure only

### Rhetorical Pattern Extraction

Extract patterns based on function, not content:

| Pattern Function | Example Template |
|------------------|------------------|
| **Motivation statement** | "[DOMAIN] has witnessed significant progress in [TASK]." |
| **Gap identification** | "However, existing approaches struggle with [PROBLEM]." |
| **Proposal introduction** | "In this work, we propose [METHOD] to address [PROBLEM]." |
| **Result framing** | "Experimental results demonstrate that [METHOD] achieves [OUTCOME]." |
| **Comparison setup** | "Compared to [BASELINE], our approach shows [ADVANTAGE]." |
| **Limitation acknowledgment** | "A limitation of this work is [LIMITATION]." |

## Source Kind Restrictions

### When `source_kind=own`

- Full extraction allowed
- Sentence templates can be retained (de-identified)
- Paragraph structure patterns can be retained
- Formatting conventions can be retained

### When `source_kind=reference`

**Restricted to abstract patterns only**:

- Voice ratios (first/third person, passive/active)
- Sentence length distributions (statistics only, no templates)
- Transition density (connector frequency, no specific examples)
- Hedging ratios (statistics only)
- Paragraph structure statistics (no patterns)

**Do NOT extract from reference**:
- Any sentence templates (even de-identified)
- Specific connector usage examples
- Any content-adjacent patterns
- Paragraph opening patterns with content hints

## Output Files

Generate these files in `psmfiles/`:

### STYLE_GUIDE.md

```markdown
# Style Guide

## 1. Voice and Tone
- Preferred voice: [third-person passive]
- First-person usage: [rare/moderate/common]
- Active voice exceptions: [when agent is important]

## 2. Sentence Rhythm
- Target median sentence length: [N words]
- Section-specific targets:
  - Introduction: [N]
  - Methods: [N]
  - Results: [N]
- Short sentence allowance: [percentage]

## 3. Transitions
- Connector density: [low/medium/high]
- Preferred connectors: [list]
- Implicit transition ratio: [percentage]

## 4. Hedging
- Hedging style: [light/moderate/heavy]
- Preferred hedge verbs: [list]
- Certainty verbs for strong evidence: [list]

## 5. Paragraph Structure
- Target paragraph length: [N sentences]
- Topic sentence position: [first/second/embedded]
- Concluding sentence usage: [yes/no/optional]

## 6. Formatting
- Figure references: [style]
- Table references: [style]
- Citation format: [style]
```

### STYLE_METRICS.json

```json
{
  "voice": { ... },
  "sentence_length": { ... },
  "transitions": { ... },
  "hedging": { ... },
  "paragraphs": { ... },
  "formatting": { ... }
}
```

### RHETORICAL_PATTERNS.md

```markdown
# Rhetorical Patterns (De-identified)

## Motivation Patterns
- [TEMPLATE_1]
- [TEMPLATE_2]

## Gap Patterns
- [TEMPLATE_1]
- [TEMPLATE_2]

## Proposal Patterns
- [TEMPLATE_1]

## Result Patterns
- [TEMPLATE_1]
- [TEMPLATE_2]

Note: These patterns are de-identified templates. Replace placeholders with target manuscript content.
```