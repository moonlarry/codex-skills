---
name: paper-figures
description: Help with paper figures, charts, and captions. Use when the user wants an architecture figure prompt, experimental chart-type recommendations, figure captions, or table captions for academic papers.
---

# Paper Figures

## Overview

Use this skill for figure ideation and figure-related wording. For editable deck-style diagrams or slide-based assets, also use the local `slides` skill at `C:\Users\pc\.codex\skills\slides\SKILL.md`.

## Workflow

1. Identify whether the user needs:
   - a main architecture figure prompt,
   - chart recommendation from data,
   - a figure caption,
   - or a table caption.
2. Load the matching reference and keep the output tightly scoped to that artifact.
3. If the task requires actual deck or slide generation, route the rendering/build step to `slides` after using this skill for structure and wording.

## Reference Routing

- Architecture figure prompt: `references/architecture-figure.md`
- Experimental chart recommendation: `references/chart-recommendation.md`
- Figure and table captions: `references/captions.md`

## Figure Rules

- Prefer academic clarity over visual decoration.
- Keep text in figures short and label-oriented.
- When recommending charts, account for uncertainty, scale gaps, and label length.
