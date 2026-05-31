# Output Manifest Protocol

For pipeline artifacts or files that overwrite an existing generated artifact, append an entry to `MANIFEST.md` in the project root. For small one-off edits, inline rewrites, or user-provided source files, manifest logging is optional unless the calling skill explicitly requires it.

## Format

If `MANIFEST.md` does not exist, create it with this header:

```markdown
# Research Output Manifest

> Tracks generated artifacts produced by paper skills.

| Timestamp | Skill | File | Stage | Description |
|-----------|-------|------|-------|-------------|
```

Then append one row per output file written:

```
| 2026-05-13 14:30 | /paper-plan | paper/PAPER_PLAN_20260513_143022.md | planning | timestamped outline |
| 2026-05-13 14:30 | /paper-plan | paper/PAPER_PLAN.md | planning | latest outline copy |
```

## Stage Values

| Stage | Skills |
|-------|--------|
| `planning` | /paper-plan |
| `writing` | /paper-writing, /paper-write, /paper-refine, /paper-translate |
| `experiment` | /experiment-plan, /experiment-result-to-claim, /experiment-ablation-planner, /experiment-claim-audit |
| `proof` | /proof-formula-derivation, /proof-writer, /proof-checker |
| `citation` | /citation-audit |
| `review` | /paper-review, /rebuttal-review, /rebuttal-writer, /rebuttal-pipeline |
| `figure` | /paper-figures-advise, /paper-figure-archi |

## Pre-flight Check

Before writing output, if the skill depends on a prerequisite file from a previous stage:
1. Check the expected paper-skill path first, usually under `paper/`, `figures/`, or `paper/review-traces/`.
2. If the expected file is missing, check the project root for a user-provided draft, table, result file, or PDF.
3. If still missing, warn that the prerequisite was not found and ask only when the missing file materially changes the next step.
4. Do not block low-risk planning work when the user clearly wants to proceed with partial context.
