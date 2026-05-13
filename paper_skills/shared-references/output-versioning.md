# Output Versioning Protocol

When writing any output file that would overwrite an existing file, use timestamped filename + fixed-name latest copy:

1. Write output to timestamped file: `{FILENAME}_{YYYYMMDD_HHmmss}.md` (or `.json`, `.tex` as appropriate)
   - Timestamp precision to seconds to reduce collisions. In the rare case of sub-second conflicts, append `_2`, `_3` etc.
   - Place the timestamped file in the same directory as the fixed-name file
2. Copy the same content to the fixed-name file: `{FILENAME}.md` (overwrites the previous latest copy)
3. Downstream skills always read the fixed-name file — they do not need to know about timestamps

## Directory Structure

Paper-skill outputs use a compact project layout:

```
project/
├── AGENTS.md                              # Project instructions
├── MANIFEST.md                            # Optional output tracking manifest
│
├── paper/                                 # Manuscript, plans, audits, and reviews
│   ├── PAPER_PLAN.md
│   ├── PAPER_PLAN_20260513_143022.md
│   ├── EXPERIMENT_PLAN.md
│   ├── EXPERIMENT_RESULTS.md
│   ├── PAPER_CLAIM_AUDIT.json
│   ├── CITATION_AUDIT.json
│   ├── PROOF_AUDIT.json
│   ├── main.tex
│   └── review-traces/
│       └── <skill>/<date>_run<NN>/
│
└── figures/                               # Figure prompts, drafts, and generated references
    ├── architecture_prompt.md
    └── architecture_reference.png
```

## What to Timestamp

Files that get overwritten on re-runs:
- `EXPERIMENT_PLAN.md`, `EXPERIMENT_TRACKER.md`, `EXPERIMENT_RESULTS.md`
- `PAPER_PLAN.md`, `REBUTTAL_PLAN.md`, `REVIEW_SUMMARY.md`
- `PAPER_CLAIM_AUDIT.json`, `CITATION_AUDIT.json`, `PROOF_AUDIT.json`
- `main.tex`, rewritten section files, and generated figure prompts

## What NOT to Timestamp

- **Append-only files**: `MANIFEST.md` and review trace folders accumulate entries.
- **User source files**: input PDFs, user-provided drafts, raw result files, and bibliography files should not be duplicated unless a skill explicitly produces a revised copy.
- **Project instructions**: `AGENTS.md` is a single source of truth and should not be timestamped.

Never delete timestamped files. They are the permanent history.

## Path Fallback Rule

Skills that read paper-skill outputs should check the canonical location first and then fall back to common user-provided locations:

```
# For plans and audits:
Read from paper/PAPER_PLAN.md
If not found -> fall back to ./PAPER_PLAN.md

Read from paper/EXPERIMENT_PLAN.md
If not found -> fall back to ./EXPERIMENT_PLAN.md

Read from paper/PAPER_CLAIM_AUDIT.json
If not found -> fall back to ./PAPER_CLAIM_AUDIT.json
```

Skills that write new paper-skill outputs should prefer the canonical project layout unless the user names a specific path.

## Existing Projects

If an existing project already uses a different paper layout, follow that layout instead of moving files. Offer migration only when the user asks for cleanup or standardization.

## Stale State Detection

Before relying on a generated plan, audit, or result summary:
1. Check the file's last modified time with the available shell or file metadata command.
2. If it is clearly older than the current paper direction, warn the user.
3. If the user chooses to start fresh, write a timestamped archive copy and proceed without overwriting the old history.
