# Style Application

## Overview

Apply extracted style patterns to the target manuscript. This document defines how to modify prose while preserving all factual content.

## Application Principle

**Core rule**: Style changes must be target-grounded. Every rewritten sentence must preserve only facts, terms, citations, equations, numbers, and claims already present in the target manuscript.

## Application Pipeline

```
Style Profile + Target Manuscript → Application Plan → Section-by-Section Rewrite → Guardrail Check
```

## Application Planning

### Step 1: Section Scope Determination

Default scope (prose-heavy sections):
- Introduction
- Related Work / Related Works
- Results / Experimental Results
- Discussion (interpretive prose only)

Excluded from style calibration:
- Abstract (formulaic, venue-dependent)
- Methods (technical precision required)
- Theory / Preliminaries (mathematical precision)
- Experiments Setup (technical precision)
- Conclusion (venue-dependent structure)
- Limitations (required honesty statements)

User can override scope with `scope=introduction,results`.

### Step 2: Change Inventory

For each section, identify potential changes:

| Change Type | Example | Permitted |
|-------------|---------|-----------|
| Voice adjustment | "We propose" → "A method is proposed" | Yes |
| Sentence length | Split long sentences, combine short | Yes |
| Connector adjustment | Remove redundant "Furthermore" | Yes |
| Hedging adjustment | "demonstrates" → "suggests" (if evidence allows) | Yes (with caution) |
| Paragraph restructuring | Reorganize topic sentences | Yes |
| Formatting change | "Figure 1" → "Fig. 1" | Yes |
| Content change | Add new claim or remove existing | NO |
| Terminology change | Rename method or dataset | NO |
| Citation change | Add/remove/modify citations | NO |
| Number change | Modify experimental values | NO |
| Math change | Simplify or modify equations | NO |

### Step 3: Prioritize Changes

Apply changes in this order:

1. **Voice adjustments** (most structural impact)
2. **Sentence rhythm** (combine/split sentences)
3. **Transition cleanup** (remove redundant connectors)
4. **Paragraph structure** (reorganize topic sentences)
5. **Formatting alignment** (figure/table references)
6. **Hedging refinement** (only if evidence strength unchanged)

## Voice Application

### Passive Voice Preference

When style profile shows passive preference:

**Before**:
> "We observe that the model performs well on small datasets."

**After**:
> "Strong performance on small datasets was observed."

**Rules**:
- Keep passive for methods/procedures ("The model was trained...")
- Convert active claims to passive when appropriate
- Do NOT change agency for responsibility-bearing statements

### Third-Person Preference

When style profile shows third-person preference:

**Before**:
> "We believe this approach will be useful for future research."

**After**:
> "This approach may prove useful for future research applications."

**Rules**:
- Remove "we" from non-essential claims
- Keep "we" for genuine author actions ("We conducted experiments...")
- Replace "We propose" with "The proposed method" or "This work proposes"

## Sentence Length Application

### Long Sentence Handling

When target sentence exceeds style profile median by 2x:

**Before** (35 words):
> "The experimental results demonstrate that our proposed framework achieves significant improvements across multiple benchmark datasets, particularly showing strong performance on tasks requiring complex reasoning and multi-step inference capabilities."

**After** (split):
> "Experimental results show significant improvements across multiple benchmarks. Particularly strong performance was observed on tasks requiring complex reasoning and multi-step inference."

**Rules**:
- Split at natural clause boundaries
- Maintain logical flow between split sentences
- Do NOT lose any factual content

### Short Sentence Handling

When too many consecutive short sentences (<10 words):

**Before**:
> "We tested the model. It worked well. Results were good. We compared baselines."

**After** (combine):
> "Testing confirmed the model's effectiveness, with favorable results compared to baselines."

**Rules**:
- Combine sentences sharing a logical theme
- Use appropriate connectors (but avoid over-connectorizing)
- Preserve all factual elements

## Transition Application

### Connector Cleanup

When style profile shows explicit connector overuse:

**Before**:
> "Furthermore, we developed a new algorithm. Additionally, we tested it on three datasets. Moreover, the results were positive. In addition, we compared with baselines."

**After**:
> "We developed a new algorithm and tested it on three datasets. Results were positive compared to baselines."

**Rules**:
- Remove redundant connectors in consecutive sentences
- Keep connectors when they serve genuine logical transitions
- Prefer implicit transitions via content flow
- Retain connectors for contrast ("However") when meaning requires it

### Paragraph Opening Variation

When style profile shows connector-heavy openings:

Replace with:
- Subject-first: "The proposed method addresses..."
- Observation-first: "Performance gains were observed when..."
- Context-first: "In the domain of X, prior work has..."
- Question-based: "A key question is whether..."

## Hedging Application

### Hedging Adjustment Rules

**Strengthen hedging** (when evidence is tentative):
- "demonstrates" → "suggests"
- "proves" → "indicates"
- "clearly shows" → "shows"

**Reduce hedging** (when evidence is strong):
- "may suggest" → "suggests"
- "possibly indicates" → "indicates"
- "could potentially" → "can"

**Critical rule**: Only adjust hedging when the evidence strength in the target manuscript genuinely supports the change. Never:
- Strengthen a claim beyond evidence
- Reduce hedging for unsupported assertions
- Change hedging for method claims (keep neutral)

### Evidence-Based Hedging Matrix

| Evidence Type | Appropriate Hedging |
|---------------|---------------------|
| Statistical significance (p<0.05) | "demonstrates", "confirms" |
| Moderate improvement | "shows", "indicates" |
| Qualitative observation | "suggests", "appears" |
| Hypothesis/plausibility | "may", "could" |
| No direct evidence | Keep original hedging |

## Paragraph Structure Application

### Topic Sentence Adjustment

When style profile shows topic-first preference:

**Before** (embedded topic):
> "After running extensive experiments, we found that the model achieved 95% accuracy, demonstrating strong capability in the target task."

**After** (topic-first):
> "The model demonstrates strong capability in the target task. Extensive experiments achieved 95% accuracy."

**Rules**:
- Position main claim/assertion at paragraph start
- Follow with supporting evidence
- Do NOT reorder chronological procedures

### Concluding Sentence Handling

When style profile discourages concluding sentences:

**Before**:
> "The results improved by 15%. This demonstrates the effectiveness of our approach."

**After**:
> "The results improved by 15%." (Remove redundant conclusion)

**Rules**:
- Remove conclusions that merely restate the evidence
- Keep conclusions that add interpretation or implication
- Keep venue-required summary structures

## Formatting Application

### Reference Style Alignment

When style profile shows specific reference formats:

**Figure references**:
- Target: "Fig. 1" → Apply if style profile prefers abbreviated
- Target: "Figure 1" → Apply if style profile prefers full

**Table references**:
- Target: "Table 1" → Apply if style profile prefers full
- Target: "Tbl. 1" → Apply if style profile prefers abbreviated

**Citation style** (LaTeX):
- Numeric "[1]" vs Author-year "(Smith, 2020)" → Align with style profile
- For LaTeX, modify `\cite{}` to appropriate citation command if needed

**Important**: Do NOT change citation content, keys, or bibliography entries—only the reference format in prose.

## Scope Control

### Per-Section Application Limits

Apply style changes proportionally to section size:

| Section | Max Changes | Notes |
|---------|-------------|-------|
| Introduction | 30-50% of sentences | Heavy prose, style matters most |
| Related Work | 20-30% of sentences | Balance style with citation accuracy |
| Results | 15-25% of sentences | Preserve precise claims |
| Discussion | 30-40% of sentences | Interpretive prose, style applies well |

### Change Density Rule

Do NOT modify more than:
- 40% of sentences in any section
- 2 consecutive sentences without preserving at least one
- Any sentence containing critical numbers or equations

## Post-Application Verification

After applying changes, verify:

1. **Content preservation**: All facts, numbers, citations, claims intact
2. **LaTeX integrity**: All commands, environments, math preserved
3. **Logical flow**: Section still reads coherently
4. **Evidence alignment**: Hedging matches evidence strength
5. **Style consistency**: Changes align with style profile

If any check fails, revert changes and reassess.