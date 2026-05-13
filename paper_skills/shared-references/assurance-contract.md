# Assurance Contract

This contract is referenced by `paper-writing`, `experiment-claim-audit`, `citation-audit`, and `proof-checker`.

## Levels

- `draft`: audits may run when detectors match; missing audit artifacts are non-blocking.
- `submission`: required audits must emit JSON artifacts and blocking verdicts must stop finalization.

## Required Submission Artifacts

- `paper/PROOF_AUDIT.json`
- `paper/PAPER_CLAIM_AUDIT.json`
- `paper/CITATION_AUDIT.json`

Each artifact should include:

```json
{
  "audit_skill": "skill-name",
  "verdict": "PASS | WARN | FAIL | NOT_APPLICABLE | BLOCKED | ERROR",
  "reason_code": "short_machine_readable_reason",
  "summary": "One-line human-readable summary.",
  "audited_input_hashes": {
    "main.tex": "sha256:..."
  },
  "trace_path": "paper/review-traces/<skill>/<date>_run<NN>/",
  "review_trace": "<fresh paper architect/reviewer trace id>",
  "review_role": "paper architect/reviewer",
  "generated_at": "<UTC ISO-8601>",
  "details": {}
}
```

## Blocking Policy

At `submission`, the parent workflow must stop before the Final Report if any required artifact is missing, malformed, stale against the inspected inputs, or has verdict `FAIL`, `BLOCKED`, or `ERROR`.

`WARN` is not automatically blocking, but the final report must surface it.
