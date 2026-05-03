# Core Workflow

Follow this pipeline exactly. Do not skip steps.

## Phase 1: Global Logic Check

Evaluate the input at the right scope:

- Single paragraph: check sentence order, causal links, pronoun references, and whether the conclusion is supported.
- Multi-paragraph subsection: check paragraph roles, transitions, repetition, and whether the subsection advances a coherent point.
- Full section or full paper: check section order, duplicated claims, terminology drift, missing transitions, and whether experimental evidence actually supports the stated contributions.

Internal checklist:

- Is the current structure logically ordered?
- Are any paragraphs or sections functionally redundant?
- Are conclusions stated before evidence is available?
- Are any claims stronger than what the evidence supports?
- Does the terminology remain stable across the full span of text?

## Phase 2: Outline Reconstruction

If the input is longer than one paragraph, extract and rebuild the logical skeleton first.

Assign each paragraph or subsection a role such as:

- Background
- Claim
- Evidence
- Explanation
- Transition
- Limitation

Then produce a compact section or document outline and use that outline as the rewrite plan. Do not merely edit the original paragraph order in place.

## Phase 3: Compression

Compress after the outline is stable.

Compression targets:

- Repeated claims
- Empty transitions
- Low-information framing
- Redundant qualifications
- Sentences that can be merged without losing meaning

Compression level:

- Medium-high by default
- Preserve technical facts, parameters, formulas, citations, baselines, comparison targets, and claim boundaries
- Do not remove any evidence needed to support later conclusions

## Phase 4: Sentence-Level Polishing

Polish on top of the compressed draft, not the original text.

Requirements:

- Fix grammar, punctuation, and article usage
- Improve precision and readability
- Tighten transitions between adjacent sentences
- Keep terminology consistent
- Maintain top-tier conference style without adding flashy wording

## Phase 5: Reviewer-Style Final Pass

Read the polished result like a skeptical reviewer.

Check for:

- Overclaimed conclusions
- Weak evidence-to-claim linkage
- Abrupt paragraph transitions
- Hidden ambiguities
- Places where a reviewer would likely question causal interpretation or fairness of comparison

If any issue remains, revise minimally and only then present the final answer.
