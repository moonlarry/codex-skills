---
name: paper-figures-advise
description: Advise on paper figures, charts, and captions. Use when the user wants experimental chart-type recommendations, figure captions, or table captions for academic papers.
---

# Paper Figures Advise

## Overview

Use this skill for experimental figure ideation and figure-related wording. For architecture reference images generated from code or method descriptions, use `paper-figure-archi`.

## Workflow

1. Identify whether the user needs:
   - chart recommendation from data,
   - a figure caption,
   - or a table caption.
2. Load the matching reference and keep the output tightly scoped to that artifact.
3. If the task requires actual deck or slide generation, route the rendering/build step to `slides` after using this skill for structure and wording.

## Reference Routing

- Experimental chart recommendation: `references/chart-recommendation.md`
- Figure and table captions: `references/captions.md`
- Architecture reference figures: use `paper-figure-archi`.

## Figure Rules

- Prefer academic clarity over visual decoration.
- Keep text in figures short and label-oriented.
- When recommending charts, account for uncertainty, scale gaps, and label length.
