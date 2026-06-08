# Style Guardrails

## Core Principle

```
Style changes must be target-grounded:
Every rewritten sentence preserves only facts, terms, citations, equations,
numbers, and claims already present in the target manuscript.
```

## Three-Stage Safety

### Stage 1: Profile (Safe)

- Only reads source document
- No modification to any file
- Output is de-identified patterns only

### Stage 2: Diagnosis (Safe)

- Only reads target document
- Generates analysis report
- No modification to target

### Stage 3: Restructure (Protected)

- Copies target before modification
- Modifies copy only
- Original file untouched

## Prohibited Changes

### Content Level

| Category | Prohibited | Reason |
|----------|------------|--------|
| Claims | All claims, conclusions | Content ownership |
| Results | Numerical values, comparisons | Scientific integrity |
| Methods | Algorithm details, parameters | Technical accuracy |
| Datasets | Dataset names, benchmarks | Factual identity |
| Citations | Citation keys, authors | Academic integrity |
| Equations | Formulas, derivations | Technical precision |

### Expression Level

| Category | Prohibited | Reason |
|----------|------------|--------|
| Unique phrasing | >6 word sequences from source | Plagiarism risk |
| Author features | Distinctive expressions | Identity protection |
| Argument chains | Source reasoning sequences | Content migration |
| Framing | Source problem angles | Content ownership |

### Structural Level

| Category | Prohibited | Reason |
|----------|------------|--------|
| Paragraph order | Source sequence | Content structure |
| Section organization | Source outline | Venue-specific |
| Contribution framing | Source presentation | Content identity |

## Allowed Changes

### Structure

- Move sequence adjustment (M1-M2-M3)
- Paragraph reorganization
- Gap framing strengthening
- Contribution alignment

### Expression

- Voice adjustment (active/passive)
- Sentence length modification
- Connector density tuning
- Hedging level calibration

### Format

- Figure reference style
- Table reference style
- Citation format alignment

## De-identification Requirement

All extracted patterns must be de-identified:

**Before**: "Our Transformer-XL achieves 94.5%"
**After**: "The proposed [METHOD] achieves [METRIC]"

**Placeholder mapping**:
- Method → `[METHOD]`
- Dataset → `[DATASET]`
- Metric → `[METRIC]`
- Value → `[VALUE]`
- Number → remove
- Citation → remove

## Copy-Before-Modify Rule

**Mandatory** for Stage 3:

1. Create `restructured/` directory
2. Copy target file(s) to `restructured/`
3. Modify copy only
4. Original file never touched

**Single file**:
```
target.tex ──► restructured/target.style-restructured.tex
```

**Project directory**:
```
project/ ──► restructured/project-copy/
```

## Pre-Modification Check

Before any change to copy:

1. Is this a factual claim? → Preserve exactly
2. Is this a numerical value? → Preserve exactly
3. Is this a citation? → Preserve exactly
4. Is this an equation? → Preserve exactly
5. Is this from source (not target)? → BLOCK

## Post-Modification Check

After changes to copy:

1. All original claims preserved?
2. All numbers unchanged?
3. All citations intact?
4. No source terminology introduced?
5. No source phrasing copied?

## N-gram Plagiarism Check

After restructure, scan for similarity:

**Block** (rewrite required):
- 8+ consecutive words match source
- 10+ words with 1-2 differences

**Review** (manual check):
- 5-7 word matches in same paragraph
- Semantic + position + length similarity

**Exclude**:
- Fixed technical terms
- Standard academic phrases
- Common connector sequences

## Error Handling

If any stage fails:

1. Preserve completed stages' outputs
2. Update manifest.json with status
3. Record error details
4. Never touch original files
5. Allow retry from failed stage

## Priority Order

When conflicts arise:

```
Factual accuracy > Venue requirements > Style preferences
```

**Examples**:
- Style prefers active, but method needs passive → Keep passive
- Style prefers short sentences, but claim needs length → Keep length
- Style prefers no hedging, but evidence is tentative → Keep hedging

## Summary

```
ALLOWED:
- Structure: Move sequence, paragraph organization
- Expression: Voice, rhythm, connectors
- Format: References, citations style

BLOCKED:
- Content: Claims, results, methods, datasets
- Expression: Source phrasing, unique sequences
- Structure: Source order, organization

GUARANTEED:
- Original files never modified
- Changes only to copies in restructured/
- All modifications target-grounded
```