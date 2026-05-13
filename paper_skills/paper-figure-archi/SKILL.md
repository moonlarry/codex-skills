---
name: paper-figure-archi
description: Generate an image-generation prompt, show that prompt to the user, and then directly use the same prompt to generate a raster reference image for an academic algorithm architecture figure from paper notes or existing code. Use when the user wants a paper method/framework/architecture diagram reference, especially for deep learning papers where the default visual language should follow a clean Transformer-style architecture.
---

# Paper Figure Archi

## Overview

Use this skill to create a **reference image** for a paper architecture figure. The output is not the final submission-ready vector figure; it is a visual guide for later drawing in TikZ, Figma, PowerPoint, draw.io, Illustrator, or another editable tool.

The workflow has three required stages:

1. Produce a clear image-generation prompt from the paper framework.
2. Show the full prompt to the user so it remains visible for later revision.
3. Immediately use the same prompt with the `imagegen` skill to generate the reference image.

## Inputs

Accept any of the following:

- Existing code implementing the method, such as model classes, modules, configs, training scripts, inference scripts, or pipeline code.
- Paper notes, method section draft, `PAPER_PLAN.md`, `NARRATIVE_REPORT.md`, or a short method description.
- A user-provided list of modules, data flow, losses, inputs, outputs, and intended figure emphasis.

If code is available, inspect it before inventing structure. Prefer evidence from filenames, class names, forward passes, config keys, loss definitions, and training/inference entry points.

## Default Visual Style

For deep learning papers, default to a clean **Transformer-style architecture diagram**:

- left-to-right or top-to-bottom data flow,
- stacked encoder/decoder blocks when appropriate,
- attention, MLP, normalization, residual, fusion, and prediction heads shown as modular blocks,
- minimal labels, no dense paragraphs inside boxes,
- academic palette with white background, thin lines, soft muted colors, and high contrast,
- no decorative 3D, no photorealism, no mascot-like or marketing visuals.

For non-deep-learning algorithms, keep the same academic clarity but adapt the visual metaphor to the actual pipeline, graph, optimization loop, system flow, or theorem/algorithm structure.

## Workflow

### Step 1: Extract the Method Skeleton

Identify the smallest faithful architecture:

1. Inputs and outputs.
2. Main modules and their order.
3. Repeated blocks or stages.
4. Cross-connections, skip connections, memory banks, retrieval, fusion, or feedback loops.
5. Training-only elements such as losses, pseudo-labeling, distillation, contrastive objectives, or auxiliary heads.
6. Inference path versus training path if they differ.
7. What the figure should emphasize: novelty, data flow, efficiency, multimodal fusion, robustness, or theoretical mechanism.

Do not include implementation details that are not part of the paper claim.

### Step 2: Draft the Prompt First

Before image generation, write a prompt under this structure:

```text
Image-generation prompt for paper architecture reference:

Goal:
[one sentence describing the method and what the figure must communicate]

Canvas:
[single wide architecture figure / two-row training-inference figure / pipeline diagram / block diagram]

Architecture:
[ordered modules and data flow]

Deep-learning style:
[Transformer-style defaults, or explicitly say why another style fits better]

Labels:
[short labels only; list exact labels if important]

Visual constraints:
[white background, vector-like, clean academic diagram, no tiny text, no photorealism, no decoration]

Avoid:
[misleading modules, extra claims, unreadable text, decorative elements]
```

Show this prompt to the user before image generation. The displayed prompt is the editable source of truth for later revision requests.

Prompt visibility rule:

- Always display the exact prompt that will be sent to image generation.
- Do not wait for confirmation after showing the prompt; proceed directly to image generation.
- If the user later asks for changes, revise the visible prompt first, show the revised prompt, and then generate a new image from that revised prompt.

### Step 3: Generate the Reference Image

Use the `imagegen` skill immediately after displaying the prompt.

Generation guidance:

- Use the built-in image generation path by default.
- Ask for a raster reference image, not a final publication vector.
- Prefer a wide aspect ratio suitable for a paper figure.
- Ask for diagram-like rendering with crisp blocks and arrows.
- Keep text sparse because generated text may be imperfect.
- If labels are critical, request placeholder-like short labels and plan to redraw them manually later.
- Save project-bound output under `paper/figures/` or `figures/` with a descriptive filename such as `architecture_reference.png`.

### Step 4: Handoff for Later Edits

Before generation, make sure the user has already seen:

- The exact prompt used for the image.
- A short mapping from visual blocks to actual code/paper modules when useful.
- Any uncertainty or missing code evidence that should be corrected before drawing the final vector figure.

This lets the user revise the prompt directly if the generated image is unsatisfactory.

## Output Rules

- Never fabricate modules just to make the diagram look fuller.
- Keep the generated image visually useful for human redrawing, not overloaded with text.
- Mark training-only and inference-only paths when relevant.
- For Transformer-like methods, preserve the recognizability of attention-block structure without copying any specific published figure.
- If code and paper notes disagree, surface the mismatch before generating the prompt.
