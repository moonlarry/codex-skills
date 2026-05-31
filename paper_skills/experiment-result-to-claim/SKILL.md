---
name: experiment-result-to-claim
description: Use when experiments complete to judge what claims the results support, what they don't, and what evidence is still missing. A paper architect/reviewer evaluates results against intended claims and routes to next action (pivot, supplement, or confirm). Use after experiments finish — before writing the paper or running ablations.
---

# Experiment Result-to-Claim Gate

Experiments produce numbers; this gate decides what those numbers *mean*. Collect results from available sources, get a paper architect/reviewer judgment, then route based on the verdict.

## Context: $ARGUMENTS

## When to Use

- After a set of experiments completes (main results, not just sanity checks)
- Before committing to claims in a paper or review response
- When results are ambiguous and you need an objective second opinion

## Workflow

### Step 1: Collect Results

Gather experiment data from whatever sources are available in the project:

1. **W&B** (preferred): `wandb.Api().run("<entity>/<project>/<run_id>").history()` — metrics, training curves, comparisons
2. **EXPERIMENT_LOG.md**: full results table with baselines and verdicts
3. **paper/EXPERIMENT_TRACKER.md** or **EXPERIMENT_TRACKER.md**: check which experiments are DONE vs still running
4. **Log files**: `ssh server "tail -100 /path/to/training.log"` if no other source
5. **PAPER_PLAN.md**, **paper/EXPERIMENT_PLAN.md**, or human-readable local notes: intended claims and experiment design

Assemble the key information:
- What experiments were run (method, dataset, config)
- Main metrics and baseline comparisons (deltas)
- The intended claim these experiments were designed to test
- Any known confounds or caveats

### Step 2: Paper Architect/Reviewer Judgment

Delegate the collected results to the paper architect/reviewer role for objective evaluation:

```text
Paper architect/reviewer task:
  RESULT-TO-CLAIM EVALUATION

  Judge whether experimental results support the intended claim.

  Intended claim: [the claim these experiments test]

  Experiments run:
  [list experiments with method, dataset, metrics]

  Results:
  [paste key numbers, comparison deltas, significance]

  Baselines:
  [baseline numbers and sources — reproduced or from paper]

  Known caveats:
  [any confounding factors, limited datasets, missing comparisons]

  Please evaluate:
  1. claim_supported: yes | partial | no
  2. what_results_support: what the data actually shows
  3. what_results_dont_support: where the data falls short of the claim
  4. missing_evidence: specific evidence gaps
  5. suggested_claim_revision: if the claim should be strengthened, weakened, or reframed
  6. next_experiments_needed: specific experiments to fill gaps (if any)
  7. confidence: high | medium | low

  Be honest. Do not inflate claims beyond what the data supports.
  A single positive result on one dataset does not support a general claim.
```

### Step 3: Parse and Normalize

Extract structured fields from the paper architect/reviewer response:

```markdown
- claim_supported: yes | partial | no
- what_results_support: "..."
- what_results_dont_support: "..."
- missing_evidence: "..."
- suggested_claim_revision: "..."
- next_experiments_needed: "..."
- confidence: high | medium | low
```

### Step 3.5: Check Paper Claim Audit Context (if available)

**Skip this step if `paper/PAPER_CLAIM_AUDIT.json` does not exist.**

```
if paper/PAPER_CLAIM_AUDIT.json exists:
    read verdict from file
    attach to verdict output:
        paper_claim_audit: PASS | WARN | FAIL | NOT_APPLICABLE | BLOCKED | ERROR

    if verdict in ["FAIL", "BLOCKED", "ERROR"]:
        append to verdict: "[PAPER CLAIM AUDIT CONCERN] — see paper/PAPER_CLAIM_AUDIT.md"
        downgrade confidence to "low" regardless of reviewer judgment

    if verdict == "WARN":
        append to verdict: "[PAPER CLAIM AUDIT: WARN] — paper-to-evidence audit flagged potential issues"
else:
    paper_claim_audit = "unavailable"
    verdict is labeled "provisional — no paper claim audit available"
    (this does not block evidence collection, but any downstream writing verdict remains provisional)
```

See `../shared-references/assurance-contract.md` for the audit artifact contract.

### Step 4: Route Based on Verdict

#### `no` — Claim not supported

1. Record postmortem in findings.md (Research Findings section):
   - What was tested, what failed, hypotheses for why
   - Constraints for future attempts (what NOT to try again)
2. Update the project pipeline status in `findings.md`, `EXPERIMENT_RESULT_TO_CLAIM.md`, `paper/EXPERIMENT_TRACKER.md`, or project notes. Do not edit `AGENTS.md` as a status log.
3. Decide whether to pivot to a user-provided alternative, revise the method, or narrow the paper claim

#### `partial` — Claim partially supported

1. Update the working claim to reflect what IS supported
2. Record the gap in findings.md
3. Design and run supplementary experiments to fill evidence gaps
4. Re-run experiment-result-to-claim after supplementary experiments complete
5. **Multiple rounds of `partial` on the same claim** → record analysis in findings.md, consider whether to narrow the claim scope or switch ideas

#### `yes` — Claim supported

1. Record confirmed claim in project notes
2. If ablation studies are incomplete → trigger `/experiment-ablation-planner`
3. If all evidence is in → ready for paper writing

### Step 5: Record Claim Verdict

Always write a compact verdict record to the project's experiment notes:

- `findings.md` if it exists
- otherwise `EXPERIMENT_RESULT_TO_CLAIM.md` at the project root

Record:

- experiment identifier or result source
- intended claim
- verdict: `yes` | `partial` | `no`
- confidence
- what the data actually supports
- missing evidence
- routing action: confirm, supplement, or pivot

If `PAPER_PLAN.md` exists, also note which planned claim or section should be updated. Do not invent a project database or external workflow; keep the verdict local, human-readable, and easy for `paper-plan`, `experiment-ablation-planner`, and `paper-writing` to consume.

## Rules

- **The paper architect/reviewer is the judge, not the paper executor.** The paper executor collects evidence and routes; the reviewer role evaluates. This prevents post-hoc rationalization.
- Do not inflate claims beyond what the data supports. If the reviewer says "partial", do not round up to "yes".
- A single positive result on one dataset does not support a general claim. Be honest about scope.
- If `confidence` is low, treat the judgment as inconclusive and add experiments rather than committing to a claim.
- If reviewer delegation is unavailable, make the best local judgment you can and mark it `[pending external review]`. Drafting may continue only as provisional work; do not route to confirmed paper writing as if the claim passed.
- Always record the verdict and reasoning in findings.md, regardless of outcome.

## Review Tracing

After the paper architect/reviewer judgment, save a trace following `../shared-references/review-tracing.md`. Write files directly to `paper/review-traces/experiment-result-to-claim/<date>_run<NN>/` and include the prompt, raw reviewer response, parsed verdict, routing action, and whether the result is `[pending external review]`. Respect the `--- trace:` parameter when present (default: `full`).
