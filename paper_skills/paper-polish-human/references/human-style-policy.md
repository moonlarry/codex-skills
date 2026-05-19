# Human-Style Policy

## Goal

Make an already polished academic paper sound less mechanical while keeping it credible as a formal research manuscript. The target is not casual writing; it is slightly more varied, author-like academic prose.

## Role-Play Posture

Before editing, adopt the posture of a human author doing a final pass on an already polished manuscript while the experiment context is still fresh. A human author:

- Writes with uneven confidence: some claims are bold, others are hedged or tentative.
- Occasionally over-explains a point, then cuts back.
- Uses varied paragraph lengths, not uniform block structures.
- Sometimes starts a sentence with a conjunction or a short observation, breaking template rhythm.
- Shows mild asymmetry: certain results get more interpretive space, others are reported tersely.

This posture applies to all edits below. It is not surface decoration; it governs sentence-level decisions.

## Default Humanization

Use restrained natural phrasing such as:

- More direct motivation sentences in Introduction.
- Slightly less template-like transitions.
- Occasional shorter sentences after dense technical sentences.
- Natural observation phrases: "in practice", "a closer look", "this pattern is not surprising", "this result is useful because", "it is worth noting that", "one might expect", "at first glance", "the reason is straightforward", "in hindsight".
- Moderate use of "we" when the paper already uses first-person academic style.
- Sentence-opening variation: alternate between "We", "This", "The", "A", "When", "If", "Because", "In", "For", "Our", "These", "Such", "While", "Although", "As", "To".
- Active/passive alternation: mix "we observe that X increases" with "an increase in X was observed".
- Occasional qualification stacking: "this somewhat surprising but not entirely unexpected result", "a simple but effective heuristic".
- Mild hedging asymmetry: use "suggests", "indicates", "appears to" alongside confident "demonstrates", "confirms".
- Local redundancy for emphasis: repeat a key term in consecutive sentences rather than always using pronouns.

Avoid:

- Slang, jokes, contractions, chatty asides, and blog-like phrasing.
- Overly dramatic claims such as "remarkably", "game-changing", or "revolutionary".
- Replacing precise academic verbs with vague informal verbs.
- Adding author emotion or subjective certainty that the evidence does not support.

## LLM Feature Elimination

LLM-generated academic text exhibits structural patterns that mark it as machine-written. Identify and break these patterns in eligible sections:

**Template sentence openings**: LLMs over-use "However,", "Furthermore,", "Moreover,", "In addition,", "Specifically,", "In particular,", "Overall,", "In summary," as paragraph starters. When this pattern is clearly present, selectively replace some of these openings with direct transitions, fused sentences, or no transition word.

**Uniform paragraph structure**: LLMs produce paragraphs of nearly equal length, each with a topic sentence and concluding sentence. When the local structure allows, vary paragraph length and remove formulaic concluding sentences. Do not force very short or very long paragraphs merely to create variation.

**Citation clustering**: LLMs batch citations as "[1,2,3,4,5]". Split or narrativize clustered citations only when the support relationship between each citation and the revised sentence is clear. If that support relationship cannot be verified, preserve the citation cluster and adjust only the surrounding prose.

**Overuse of certainty qualifiers**: LLMs default to "robust", "comprehensive", "extensive experiments demonstrate", "state-of-the-art", "significantly outperforms". Replace with more measured language unless the numbers strictly justify strong terms.

**Symmetric section structure**: LLMs give equal space to each contribution or finding. Human authors allocate space asymmetrically based on importance. Adjust paragraph emphasis only when the paper's evidence, result strength, or narrative priority clearly supports the asymmetry.

Before making any natural-expression changes, scan for these patterns first and break them selectively. Pattern-breaking is the foundation; natural-phrase injection is the overlay. Do not break a pattern mechanically when it supports clarity, venue conventions, or accurate citation grounding.

## Preserving Existing Natural Traces

If the input paper already contains human-style expressions, do not overwrite them:

- Mark them as "preserved" in the humanization log.
- Do not rewrite a preserved expression to be "more human" — this creates artificial texture.
- Preserved expressions count toward the natural-expression ratio without modification.

If the input has already been through a formal polisher and shows no natural traces, proceed with fresh humanization.

## Grammar-Error Mode

Grammar-error mode is an explicit opt-in overlay after the normal human-style pass. Do not enable it unless the user clearly requests it.

Allowed minor imperfections:

- A missing or slightly awkward article in a low-stakes noun phrase.
- A mild preposition choice that remains understandable.
- A small number agreement issue in a peripheral clause.
- A slightly uneven sentence rhythm that still reads naturally.

Forbidden imperfections:

- Errors that change the meaning of a claim.
- Errors in method names, dataset names, metric names, formulas, citations, theorem statements, or numeric comparisons.
- Errors that make the paper look careless, unreadable, or unprofessional.
- More than one imperfection in the same sentence.

If an imperfection would reduce credibility or make the sentence ambiguous, do not add it.
