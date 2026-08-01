---
name: paper-grill-with-docs
description: "Run a one-question-at-a-time claim-evidence reconciliation only after experiments are complete and a full or near-complete paper exists. Use to trace central Conclusion claims back through the Abstract, Introduction, Results, scientific-support verdicts, and raw-evidence audits, then write paper/CLAIM_EVIDENCE_RECONCILIATION.md. Route missing prerequisites to experiment-result-to-claim or experiment-claim-audit. Do not use before experiments, for experiment planning, broad paper review, manuscript rewriting, or replacing a zero-context evidence audit."
---

# Paper Grill with Docs

Reconcile the finished paper's central claims with its evidence without changing the manuscript or results.

## Check Preconditions

Require a full or near-complete paper and completed experimental materials.

- Route an early research idea to `paper-grill-me`.
- Route a stable method without completed experiments to `experiment-plan`.
- Route completed experiments without a paper to `experiment-result-to-claim`.
- Route missing scientific-support verdicts to `experiment-result-to-claim`.
- Route missing paper-to-raw-result verification to `experiment-claim-audit`.

Do not reproduce either prerequisite audit inside this skill. Consume their artifacts when available and keep unavailable judgments explicit.

## Build the Claim Ledger

1. Read the paper, raw experiment materials, result-to-claim verdicts, and claim-audit artifacts.
2. Extract each central claim from the Conclusion.
3. Trace each claim backward to matching statements in the Abstract and Introduction, supporting analysis in Results, and exact tables, figures, metrics, and source files.
4. Record precise file, section, table, figure, or line locations whenever available.
5. Assign one evidence state:
   - `verified`: independently supported and matched to audited evidence;
   - `partial`: evidence supports a narrower claim;
   - `unsupported`: available evidence does not support the claim;
   - `conflicted`: sources or audits disagree;
   - `author-asserted`: supplied only by the author;
   - `unavailable`: required evidence or audit is missing.

Never promote an author's oral or written assertion to `verified` without independent evidence.

## Resolve Author Decisions

Prioritize the highest-risk mismatch. Ask exactly one author decision at a time, provide a recommended action and reason, and wait for the answer.

Use only these decisions:

- `keep`
- `narrow`
- `rewrite`
- `remove`
- `add experiment`
- `move to limitation`
- `unresolved`

Do not silently choose wording or evidence on the author's behalf.

## Write the Reconciliation

Create or update `paper/CLAIM_EVIDENCE_RECONCILIATION.md` with:

```markdown
# Claim-Evidence Reconciliation

## Status
aligned | partially-aligned | misaligned | blocked

## Preconditions
## Inputs and Audit Artifacts
## Conclusion-Centered Claim Ledger
| Claim ID | Conclusion Location and Claim | Abstract / Introduction Match | Results and Evidence Location | Scientific Support | Raw Match | Evidence State | Scope Ceiling | Author Decision | Required Action |
|---|---|---|---|---|---|---|---|---|---|

## Conflicts and Missing Evidence
## Overclaiming and Scope Drift
## Fairness and Baseline Concerns
## Unresolved Author Decisions
## Final Readiness
## Next Step
```

Mark the reconciliation complete only when every central Conclusion claim has a location-grounded evidence state and an author decision. Otherwise mark it incomplete and retain unresolved items.

## Preserve Audit Boundaries

- Do not modify manuscript text, figures, tables, raw results, or experiment records.
- Do not create or overwrite `PAPER_CLAIM_AUDIT.md`, `PAPER_CLAIM_AUDIT.json`, or result-to-claim verdicts.
- Do not replace `experiment-claim-audit` zero-context verification.
- Do not replace `paper-review`; route broader logic, presentation, or reviewer-risk assessment there.
