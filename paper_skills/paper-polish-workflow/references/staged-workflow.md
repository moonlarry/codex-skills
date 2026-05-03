# Staged Polishing Workflow

Use this reference when the user wants interactive, top-down polishing instead of an immediate rewrite.

## Stage 0: Intake

Collect:

- Target section
- Language and medium
- Whether the user wants checking only or revised text
- Whether a target journal or reference paper matters

## Stage 1: Macro Structure

Identify the logical slots in the current text before rewriting.

Typical abstract slots:

- Background
- Gap
- Method
- Results
- Contribution

Typical subsection slots:

- Setup
- Claim
- Evidence
- Interpretation
- Transition

Ask for confirmation of the structure first.

## Stage 2: Sentence or Paragraph Logic

Map each sentence or paragraph to one role.

Check for:

- Missing transitions
- Unsupported conclusions
- Repeated claims
- Abrupt jumps in scope
- Terminology drift

If the logic is wrong, fix the logic plan before touching style.

## Stage 3: Expression Options

Once logic is stable, offer 2-4 expression variants when the user needs to choose tone, density, or emphasis.

Good uses:

- Opening sentence alternatives
- Transition sentence alternatives
- Short vs dense phrasing choices

Avoid generating option tables for every sentence if a direct rewrite is already obvious.

## Stage 4: Coherence Pass

Check:

- Repetition across adjacent sentences
- Transition strength
- Consistency of technical terms
- Alignment between first and last sentence of the unit

## Stage 5: Handoff

After the user confirms the staged decisions:

- Hand off local edits to `paper-refine`
- Hand off heavy English rewriting to `paper-refine-special-en`
- Hand off heavy Chinese rewriting to `paper-refine-special-zh`
- Hand off journal formatting or highlights to `paper-journal-style`
