# Output Format

## Standard Output

Return:

```text
Part 1 [Human-Polished Text]
<revised text>

Part 2 [Humanization Log]
- Document context: [regular paper / rebuttal / revision letter / response to reviewers]
- Sections edited:
- AI patterns addressed: [pattern IDs with brief cues, e.g., VOCAB_DENSITY("pivotal"), SUPERFICIAL_ING("underscoring"), SIG_INFLATION("broader impact")]
- Main style changes:
- Claim safety: no new claims, numbers, citations, limitations, or comparisons added.
- Technical items preserved:
- Preserved existing natural traces:

Part 3 [Formal/Natural Ratio Estimate]
- Estimated formal expression:
- Estimated natural expression:
- Grammar-imperfection overlay (if enabled):
- Scope of estimate:

Part 4 [Grammar-Imperfection Log]
- Mode:
- Inserted imperfections:

Part 5 [Verification Notes]
- Audit pass completed: [list any concerns or "None identified"]
- Context detected: [regular paper / rebuttal / revision letter]
- REBUTTAL_DIFF_ANCHORING: [applied / not applied (context mismatch)]
- Preserved: technical terms, citations, LaTeX commands, numerical claims
- Remaining risk: [state if any, otherwise "None in reviewed scope"]
```

## Grammar-Imperfection Log

If grammar-error mode is disabled, write:

```text
Mode: disabled. No intentional grammar imperfections were added.
```

If grammar-error mode is enabled, list each inserted imperfection with:

- Section
- Original polished phrase, if available
- Revised phrase
- Imperfection type
- Why meaning is unchanged

## Medium Rules

For LaTeX, return valid LaTeX and preserve commands, equations, citations, labels, references, and environments.

For Word-style prose, return clean prose without Markdown inside Part 1.

For partial inputs, report that the ratio estimate applies only to the provided excerpt.