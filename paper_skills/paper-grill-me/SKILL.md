---
name: paper-grill-me
description: "Run a one-question-at-a-time innovation-gate interview before algorithm design, experiment planning, or paper drafting. Use to test whether a research problem extends the knowledge boundary, whether existing methods or direct transfer already solve it, and whether proposed components map to defensible subproblems and evidence. Always write paper/PAPER_IDEA_GRILL.md, including a conditional or not-ready diagnosis. Do not use for completed-experiment claim judgment, post-paper evidence auditing, manuscript review, or prose rewriting."
---

# Paper Grill Me

Test the paper idea before committing to an algorithm, experiment plan, or manuscript.

## Scope

- Inspect available idea notes, literature notes, code, preliminary results, venue constraints, and project instructions before asking questions.
- Resolve discoverable facts from the workspace or authoritative sources instead of asking the user.
- Ask exactly one highest-value unresolved question at a time and wait for the answer.
- Include in every question: the current decision, a recommended answer, the reason, and the next judgment it unlocks.
- Do not design a complete experiment roadmap, draft paper text, or invent novelty.

## Run the Three Gates in Order

Revisit an earlier gate whenever a later answer invalidates it.

### Gate 1: Problem and Knowledge Boundary

Establish:

- the engineering or scientific need;
- the known boundary and affected setting;
- a core problem that remains meaningful without naming the proposed method;
- the new knowledge the research could add.

Do not pass this gate when the idea is only a new component, combination, or implementation of an already-solved problem.

### Gate 2: Existing Methods and Direct-Transfer Novelty Defense

Establish:

- the strongest existing method or direct-transfer alternative;
- the strongest objection that this alternative already solves the problem;
- the concrete failure mechanism, boundary mismatch, or unverified hypothesis that prevents direct transfer;
- the evidence required to defend that distinction.

Label an untested limitation as a hypothesis. Do not present absence of evidence as proof of failure.

### Gate 3: Problem, Solution, and Evidence

Decompose the core problem into the fewest necessary subproblems. Map each retained idea or method component to a subproblem, a claim, a counterclaim, and minimum convincing evidence. Reject or defer ideas that have no problem-level role.

Keep evidence planning at the requirement level. Route detailed baselines, metrics, budgets, and run order to `experiment-plan` after the method is stable.

## Set the Verdict

Use exactly one status:

- `framing-ready`: all three gates pass and the central contribution is defensible.
- `conditional`: the direction is plausible but depends on explicit unresolved hypotheses or evidence.
- `not-ready`: a gate fails, the problem is already covered, or the method is driving the problem definition.

Treat `conditional` and `not-ready` as valid outcomes. Never force a positive verdict.

## Always Write the Record

Create or update `paper/PAPER_IDEA_GRILL.md` before ending, whether the interview converges, fails a gate, or the user asks to stop. Record:

```markdown
# Paper Idea Grill

## Status
framing-ready | conditional | not-ready

## Termination
converged | gate-failed | user-stopped | incomplete

## Need, Boundary, and Core Problem
## Candidate and Rejected Ideas
## Strongest Objection and Direct-Transfer Hypothesis
## One-Sentence Contribution
## Problem-Solution Mapping
## Claims and Counterclaims
## Evidence Needed
## Blocking Questions
## Recommended Thinking Directions
## Next Step
```

Distinguish verified facts, user assertions, hypotheses, and missing evidence inside the record.

## Route the Next Step

- Route `framing-ready` work to `paper-plan`.
- Route a stable method that needs a detailed validation roadmap to `experiment-plan`.
- Route `conditional` or `not-ready` work back to the blocking questions in `paper/PAPER_IDEA_GRILL.md`.
