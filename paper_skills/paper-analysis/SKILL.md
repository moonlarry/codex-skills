---
name: paper-analysis
description: Analyze experimental results for academic papers and give model-selection guidance. Use when the user wants LaTeX experiment analysis paragraphs from data, or practical guidance on which model family to use for academic writing, coding, or paper-polish workflows.
---

# Paper Analysis

## Overview

Use this skill for data-grounded interpretation and model-choice heuristics. Keep time-sensitive rankings out of the main skill body; if the user asks for the latest model status, verify it separately instead of trusting stale lists.

## Workflow

1. Identify whether the task is experiment interpretation or model selection.
2. For experiment interpretation, ground every claim in the provided numbers and load `references/experiment-analysis.md`.
3. For model selection, use `references/model-selection.md` as principles only. If the user asks for the latest or current best model, verify with up-to-date sources before answering.

## Reference Routing

- Experimental result to LaTeX analysis paragraph: `references/experiment-analysis.md`
- Stable model-selection heuristics for writing and coding workflows: `references/model-selection.md`

## Analysis Rules

- Never fabricate trends, significance, or causal stories unsupported by the data.
- Distinguish between absolute gains, relative gains, efficiency tradeoffs, and robustness claims.
- Treat model advice as scenario-based guidance, not a frozen leaderboard.
