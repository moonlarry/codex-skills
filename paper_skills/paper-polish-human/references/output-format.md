# Output Format

## Standard Output

Return:

```text
Part 1 [Human-Polished Text]
<revised text>

Part 2 [Humanization Log]
- Sections edited:
- Main style changes:
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
- Verified:
- Assumptions:
- Unrun checks:
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
