# Goal Preflight Blocker Scan

Use this reference only from `paper-writing` before starting a `/goal` paper-writing run.

## Purpose

Run a read-only scan before the long pipeline starts. The goal is to ask the user once about real blockers, then let the pipeline proceed autonomously. Do not ask about preferences or minor uncertainty that can be handled with a conservative assumption.

## Blocking Questions Before Start

Ask before starting only when at least one item below is true:

| Blocker | Why it blocks |
|---|---|
| Input material path is missing, unreadable, empty, or points to the wrong project | The pipeline cannot ground the paper |
| Multiple incompatible paper positions or main claims appear in the materials | Choosing one would change the scientific story |
| `assurance=submission` is requested but the target venue/journal cannot be inferred | Submission checks, page limits, anonymity, and citation style may differ |
| A proposed main contribution has no experiment, theory, dataset, citation, or other evidence | Writing it would fabricate or overclaim |
| Existing results contradict the intended claim | The claim must be weakened, reframed, or abandoned |
| Required baseline, ablation, dataset, or statistical evidence is absent and needed for a main claim | The pipeline cannot honestly claim the result |
| Citation audit is requested or obvious citation replacement/removal is needed before writing | Replacing/removing citations affects claims and attribution |
| Existing `paper/`, `figures/`, or result files would need deletion or overwrite and provenance is unclear | Avoid destructive edits to user work |
| Required LaTeX/Python/system dependency is missing and installation is necessary | New dependencies require explicit user approval |
| Target positioning must be author-decided, not inferred | Examples: survey vs method paper, workshop vs journal, anonymous vs camera-ready, strong claim vs conservative claim |

If any blocker exists, write it under `Blocking Questions Before Start`, then pause before Phase 0.

## Non-Blocking Assumptions

Do not ask for these. Continue and log them in `paper/AUTONOMY_LOG.md`:

| Non-blocking issue | Default action |
|---|---|
| Venue unknown in `assurance=draft` | Use a generic academic draft layout and record the assumption |
| Minor missing metadata such as title wording, author block, or keywords | Use placeholders or `[TBD]` markers |
| Figure details are incomplete but the claim can be written without the figure | Create `figures/MANUAL_FIGURES_NEEDED.md` and continue with placeholders |
| Section names or order are uncertain but the problem/claim structure is clear | Use the conservative paper-plan structure |
| Writing style preference is not specified | Use formal academic style |
| Some references need metadata verification but no replacement/removal is currently required | Mark with `[VERIFY]` and route to citation audit later |
| Experiment file format is messy but readable | Parse conservatively and log assumptions |

## Report Template

Write `paper/GOAL_PREFLIGHT_REPORT.md`:

```markdown
# Goal Preflight Report

## Blocking Questions Before Start
- [none | question list]

## Non-Blocking Assumptions
- [assumptions the pipeline will use]

## Logged Risks
- [risks to copy into paper/AUTONOMY_LOG.md]

## Decision
- BLOCKED_BEFORE_START: [why] 
- or START_PIPELINE: no blocking questions found
```

## User Prompt Rule

If blocked, ask only the blocking questions. Do not include general preferences unless they determine whether the pipeline can start safely.
