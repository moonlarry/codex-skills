# Style Guardrails

## Overview

These guardrails define what can and cannot be migrated during style calibration. They are CRITICAL safety constraints.

## Core Principle

```
Style changes must be target-grounded:
Every rewritten sentence must preserve only facts, terms, citations, equations,
numbers, and claims already present in the target manuscript.
```

## Prohibited Migrations

### Content-Level Prohibitions

| Category | Specific Items | Reason |
|----------|----------------|--------|
| **Claims** | All claims, conclusions, assertions | Content belongs to target manuscript |
| **Results** | Numerical values, percentages, comparisons | Scientific integrity |
| **Methods** | Algorithm names, procedure details, parameter values | Technical accuracy |
| **Datasets** | Dataset names, benchmark names, corpus identifiers | Factual identity |
| **Metrics** | Metric names with specific values | Scientific integrity |
| **Citations** | Citation keys, author names, paper references | Academic integrity |
| **Equations** | Mathematical formulas, derivations, symbols | Technical precision |
| **Definitions** | Term definitions, notation definitions | Scholarly accuracy |

### Expression-Level Prohibitions

| Category | Specific Items | Reason |
|----------|----------------|--------|
| **Unique phrasing** | Long n-grams (>6 words) from source | Plagiarism risk |
| **Argument structures** | Proof sequences, reasoning chains | Content migration |
| **Framing** | Specific problem framing, motivation angles | Content identity |
| **Terminology** | Method names, system names, tool names from source | Not transferable |
| **Acronyms** | Acronym expansions from source | May not apply to target |
| **Limitations** | Limitation statements, failure cases | Honesty requirement |
| **Future work** | Future direction statements from source | Not applicable |

### Structural Prohibitions

| Category | Specific Items | Reason |
|----------|----------------|--------|
| **Paragraph order** | Argument/paragraph sequence from reference | Content structure |
| **Contribution list** | Specific contribution framing | Content identity |
| **Section structure** | Section organization from reference | Venue-specific |
| **Figure/Table content** | Values, trends, observations in visuals | Factual content |

### Context-Level Prohibitions

| Category | Specific Items | Reason |
|----------|----------------|--------|
| **Ethics statements** | Ethics concerns from source | Manuscript-specific |
| **Funding/Acknowledgement** | Any funding or acknowledgement content | Author-specific |
| **Author contributions** | Contribution descriptions | Author-specific |
| **Institution names** | Lab/university names from source | Identity |
| **Product names** | Commercial tools, platforms from source | Not transferable |

## Allowed Migrations

### Abstract Style Elements

| Element | Migration Rules |
|---------|-----------------|
| **Voice preference** | Transfer ratio (first/third person, passive/active) |
| **Sentence rhythm** | Transfer length distribution statistics |
| **Connector density** | Transfer frequency preferences |
| **Hedging style** | Transfer hedging/certainty ratio |
| **Paragraph length** | Transfer average paragraph statistics |
| **Topic sentence position** | Transfer structural preference |

### Formatting Conventions

| Element | Migration Rules |
|---------|-----------------|
| **Figure references** | "Fig. 1" vs "Figure 1" preference |
| **Table references** | "Table 1" vs "Tbl. 1" preference |
| **Citation format** | Numeric vs author-year display preference |
| **Math notation** | Inline vs display preference (style only) |

### Rhetorical Patterns (De-identified)

| Pattern Type | Migration Rules |
|--------------|-----------------|
| **Motivation template** | Template only: "[DOMAIN] has seen progress in [TASK]." |
| **Gap template** | Template only: "Existing methods struggle with [PROBLEM]." |
| **Proposal template** | Template only: "This work proposes [METHOD]." |
| **Result template** | Template only: "Results show [OUTCOME]." |

Templates must have all content placeholders replaced with target manuscript content.

## Reference Source Extra Restrictions

When `source_kind=reference` (external paper), additional restrictions apply:

### Restricted Extractions

| What | Restriction |
|------|-------------|
| Sentence templates | NOT allowed—even de-identified |
| Specific connector examples | NOT allowed |
| Content-bearing rhetorical patterns | NOT allowed |
| Paragraph opening examples | NOT allowed (content-adjacent) |
| Hedging examples with context | NOT allowed |

### Allowed Extractions (Reference Source)

| What | Allowed Form |
|------|--------------|
| Voice statistics | Ratios only (e.g., "passive: 60%") |
| Sentence length statistics | Distribution numbers only |
| Connector frequency | Density number only (e.g., "0.08 per sentence") |
| Hedging statistics | Ratios only |
| Paragraph statistics | Averages only |

The difference: `own` source can provide patterns with examples; `reference` source provides only abstract statistics.

## Guardrail Enforcement

### Pre-Application Check

Before applying any style change:

1. Does the change modify any factual content? → BLOCK
2. Does the change introduce content from source? → BLOCK
3. Does the change modify a citation? → BLOCK
4. Does the change modify a number or value? → BLOCK
5. Does the change modify an equation? → BLOCK
6. Is the source_kind=reference and change uses content examples? → BLOCK

### Post-Application Check

After applying changes:

1. Verify all claims unchanged
2. Verify all numbers unchanged
3. Verify all citations unchanged
4. Verify all equations unchanged
5. Verify no source-specific terminology introduced
6. Verify hedging matches evidence strength

### Audit Checklist

```markdown
## Guardrail Audit

- [ ] All original claims preserved
- [ ] All numerical values preserved
- [ ] All citation keys preserved
- [ ] All equation content preserved
- [ ] No source terminology introduced
- [ ] No source framing adopted
- [ ] No source argument structure copied
- [ ] Hedging strength matches evidence
- [ ] All changes are style-only
```

## Edge Cases and Exceptions

### When Guardrails Conflict

| Situation | Resolution |
|-----------|------------|
| Style prefers active voice, but method description uses passive correctly | Keep passive (technical accuracy) |
| Style prefers short sentences, but complex claim requires length | Keep length (content accuracy) |
| Style prefers no hedging, but evidence is tentative | Keep hedging (intellectual honesty) |
| Style prefers specific format, but venue requires different | Use venue format (submission requirement) |

Priority order:
```
Factual accuracy > Venue requirements > Style preferences
```

### Section-Specific Exceptions

| Section | Special Treatment |
|---------|-------------------|
| **Abstract** | Minimal style changes; venue-dependent format |
| **Methods** | Preserve technical voice; avoid rhetorical changes |
| **Theory** | No changes to math notation or proof structure |
| **Limitations** | Keep exact honesty statements; no softening |
| **Conclusion** | Minimal changes; venue-dependent structure |

## Plagiarism Prevention

### N-gram Check

After calibration, scan for suspicious n-grams:

1. Extract all 7+ word sequences from calibrated output
2. Check against source document
3. Flag any exact matches of 7+ words
4. Flag any near-matches (1-2 word differences) of 10+ words

If flagged:
- Review the flagged section
- Rewrite to break the similarity
- Ensure content is genuinely target-grounded

### Structural Similarity Check

After calibration, check for structural copying:

1. Compare paragraph sequence between source and target
2. Compare argument flow (if similar domain)
3. Flag if identical structural progression detected

If flagged:
- Review whether structure is genre-appropriate (common in academic writing)
- If structure copied from reference, restructure target paragraphs

## Summary

```
ALLOWED: voice ratios, sentence statistics, connector density, 
         formatting preferences, de-identified templates

BLOCKED: claims, numbers, citations, equations, methods, datasets,
         source terminology, argument structure, unique phrasing,
         limitations, future work, content-bearing patterns

EXTRA BLOCKED for reference sources: all templates, all examples,
         all content-adjacent patterns

PRIORITY: Facts > Venue > Style
```