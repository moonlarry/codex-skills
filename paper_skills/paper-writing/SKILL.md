---
name: paper-writing
description: "Workflow 3: Full paper writing pipeline. Orchestrates paper-plan → paper-figure → paper-write Markdown drafts → paper-write LaTeX section files → paper-compile → paper-review → formal polish → optional human-style polish → proof/claim/citation audits to go from a narrative report to a polished PDF. Use for /goal-based long-running paper writing, \"写论文全流程\", \"write paper pipeline\", \"从报告到PDF\", \"paper writing\", Markdown-first paper drafting, or complete paper generation and revision. At `— effort: max | beast` (or explicit `— assurance: submission`), Phase 6 gates the Final Report on required audit artifacts and blocking verdicts."
---

# Workflow 3: Paper Writing Pipeline

Orchestrate a complete paper writing workflow for: **$ARGUMENTS**

## Overview

This skill chains the paper-writing, review, polish, compile, and audit skills into a single automated pipeline:

```
/paper-plan → /paper-figure → /paper-write Markdown drafts → /paper-write LaTeX → /paper-compile
  (outline)     (plots)        (draft prose)                  (section files)       (build PDF)

→ /paper-review → formal polish → optional /paper-polish-human
  (review)        (academic polish) (human-style final pass)

→ /proof-checker → /experiment-claim-audit → /citation-audit → Final Report
  (proof gate)     (number/claim gate)        (citation gate)
```

Each phase builds on the previous one's output. The final deliverable is a polished, reviewed `paper/` directory with LaTeX source and compiled PDF.

This pack follows the AGENTS.md paper workflow: the paper architect/reviewer diagnoses and plans; the paper executor applies minimal edits and verifies outputs.

## Constants

- **VENUE = `ICLR`** — Target venue. Options: `ICLR`, `NeurIPS`, `ICML`, `CVPR`, `ACL`, `AAAI`, `ACM`, `IEEE_JOURNAL` (IEEE Transactions / Letters), `IEEE_CONF` (IEEE conferences). Affects style file, page limit, citation format.
- **MAX_IMPROVEMENT_ROUNDS = 2** — Number of review→fix→recompile rounds in the improvement loop.
- **REVIEW_ROLE = paper architect/reviewer** — Use the AGENTS.md paper review role for plan, figure, writing, and improvement review. Do not hard-code a model or backend.
- **REVIEW_SKILL = `paper-review`** — Full-paper reviewer-style diagnosis before edits.
- **FORMAL_POLISH_SKILL = `paper-polish-workflow` or `paper-refine-special-en` / `paper-refine-special-zh`** — Formal academic polish after review-driven fixes.
- **HUMAN_POLISH_SKILL = `paper-polish-human`** — Optional final naturalness pass after formal polishing; intentional grammar imperfections stay disabled unless the user explicitly enables grammar-error mode.
- **AUTO_PROCEED = true** — Auto-continue between phases. Set `false` to pause and wait for user approval after each phase.
- **HUMAN_CHECKPOINT = false** — When `true`, Phase 5 pauses after each review round to let the user see the score and provide custom modification instructions.
- **GOAL_PREFLIGHT = required** — In `/goal` mode, run a read-only blocker scan before Phase 0. Ask only once, and only if blocking questions exist.
- **GOAL_AUTONOMY = `execute-first`** — In `/goal` mode, do not ask for non-blocking clarification. Make conservative assumptions, continue, and record uncertainty in `paper/AUTONOMY_LOG.md` and the Final Report.
- **ILLUSTRATION = `manual`** — Architecture, pipeline, and qualitative figures are manual inputs in this migrated pack. Use existing figures or create simple local TikZ/diagram placeholders only when the user asks.

> Override inline: `/paper-writing "NARRATIVE_REPORT.md" — venue: NeurIPS, human checkpoint: true`
> IEEE example: `/paper-writing "NARRATIVE_REPORT.md" — venue: IEEE_JOURNAL`

## Goal Mode Invocation

Use `/goal` when the paper task should continue across turns until a verifiable stopping condition is met. The goal text should name the input material, target venue or journal, assurance level, pause conditions, and final artifacts when known. If non-critical details are missing, infer conservative defaults, continue, and log the assumption instead of asking.

Before starting the pipeline in `/goal` mode, run **Phase -1: Goal Preflight Blocker Scan**. This is a read-only scan for obvious or likely blockers. If blockers exist, present one consolidated question list and wait before starting. If no blockers exist, start immediately and record non-blocking assumptions in `paper/AUTONOMY_LOG.md`.

Recommended full-pipeline goal:

```
/goal 基于 [材料路径]，使用现有 paper-writing 工作流完成论文写作、修改、润色与验证。启动前先执行 Goal Preflight Blocker Scan，只读排查明显或潜在阻塞问题；若存在阻塞项，先集中询问用户再开始。若无阻塞项，先读取所有指定材料，建立 claim-evidence matrix、Problem Definition Lock 和 Introduction Blueprint，然后依次执行 paper-plan、paper-figure、paper-write Markdown 草稿、paper-write LaTeX 成稿、paper-compile、paper-review、formal polish，并在需要时执行 paper-polish-human。默认非必要不问用户：遇到目标期刊/会议不明确、材料有轻微缺口、章节命名不确定、figure 细节不足但可占位等非阻塞问题时，先采用保守假设继续执行，并把假设与风险写入 paper/AUTONOMY_LOG.md 和最终报告。不得编造实验结果、引用、数据、方法或结论。只有当下一步会改变核心科学 claim、需要缺失证据支撑主贡献、替换/删除引用、增加实验、删除不确定来源文件、安装依赖、或必须由作者决定论文定位时才暂停。停止条件是 paper/drafts/*.md 与 paper/sections/*.tex 均完成，paper/main.pdf 编译成功，PAPER_IMPROVEMENT_LOG.md 和最终报告生成；若 assurance=submission，则 proof-checker、experiment-claim-audit、citation-audit 三类审计 JSON 均存在，且无 FAIL、BLOCKED 或 ERROR。
```

Draft-mode goal:

```
/goal 基于 [材料路径] 生成论文草稿，使用 paper-writing 工作流完成问题定义、计划、图表、Markdown 草稿、LaTeX section 文件、编译和一轮审稿式修改。assurance=draft。默认非必要不问用户，能根据材料保守推断的内容先执行并记录假设。停止条件是 paper/drafts/*.md 与 paper/sections/*.tex 均完成，paper/main.pdf 编译成功，并生成 PAPER_IMPROVEMENT_LOG.md、paper/AUTONOMY_LOG.md 和最终报告。不得编造实验结果、引用或结论；关键证据缺失到无法支撑主贡献时才暂停。
```

Submission-mode goal:

```
/goal 基于 [材料路径] 完成投稿前论文版本。使用 paper-writing 工作流完成写作、编译、审稿式修改、正式润色和 submission 级审计。assurance=submission。默认非必要不问用户，保守假设必须记录到 paper/AUTONOMY_LOG.md。停止条件是 paper/main.pdf 编译成功，proof-checker、experiment-claim-audit、citation-audit 均生成 JSON 审计文件，且无 FAIL、BLOCKED 或 ERROR。引用替换、claim 弱化、实验缺失、原始数据缺失等会影响投稿真实性的问题必须作为阻塞项暂停并报告。
```

In goal mode, auto-proceed through low-risk phases: material inventory, problem-definition locking, plan generation, data-figure generation, Markdown drafting, Markdown-to-LaTeX section conversion, ordinary compile fixes, review-driven wording/structure fixes that preserve scientific meaning, formal polishing, recompilation, and final reporting. Do not ask for non-blocking clarification. Instead, make the smallest conservative assumption, continue, and append it to `paper/AUTONOMY_LOG.md`.

Pause only for blocking decisions:
- the next action would change the paper's scientific position or main claim;
- a main contribution has no supporting evidence and cannot be honestly weakened locally;
- citation replacement/removal is required;
- new experiments, dependency installation, or destructive file operations are required;
- the paper's target or positioning cannot be conservatively inferred without changing format, claims, or submission requirements.

For non-blocking uncertainty, continue and log:
- unknown venue in draft mode -> use a generic article/conference draft template and record the assumption;
- missing non-data figure -> create `figures/MANUAL_FIGURES_NEEDED.md`, reference it as manual, and continue with text placeholders;
- incomplete wording preferences -> use formal academic style and record that no custom style preference was provided;
- minor missing metadata -> use `[VERIFY]` or `[TBD]` markers and include them in the Final Report.

When invoking child skills from goal mode, pass this autonomy rule through: child skills should not ask for clarification unless the issue is blocking under the rules above.

Read `references/goal-preflight.md` for the full preflight blocker taxonomy and report template.

## Inputs

This pipeline accepts one of:

1. **`NARRATIVE_REPORT.md`** (best) — structured research narrative with claims, experiments, results, figures
2. **Research direction + experiment results** — the skill will help draft the narrative first
3. **Existing `PAPER_PLAN.md`** — skip Phase 1, start from Phase 2

The more detailed the input (especially figure descriptions and quantitative results), the better the output.

## Pipeline

### Phase -1: Goal Preflight Blocker Scan

Run this phase before Phase 0 for every `/goal` invocation.

1. Read `references/goal-preflight.md`.
2. Inspect the provided material paths, existing `paper/` workspace, experiment/result files, citation files, target venue hints, and requested assurance level.
3. Write `paper/GOAL_PREFLIGHT_REPORT.md` with:
   - `Blocking Questions Before Start`
   - `Non-Blocking Assumptions`
   - `Logged Risks`
   - `Decision`
4. If blocking questions exist, stop before Phase 0 and ask the user only those questions in one consolidated message.
5. If no blocking questions exist, create or append `paper/AUTONOMY_LOG.md`, then continue to Phase 0 without asking.

Never use preflight as a general interview. It exists only to catch problems that would waste a long goal run or force later scientific/file-safety reversals.

### Phase 0: Assurance Setup

Resolve the active `assurance` level and persist it so Phase 6's local
artifact gate reads the same value. **Run once at pipeline start, before Phase 1.**

**Resolution order** (first match wins):

1. Explicit `— assurance: draft | submission` in `$ARGUMENTS`
2. Derived from `— effort:`
   - `lite` / `balanced` → `draft` (default, **zero change from current behavior**)
   - `max` / `beast` → `submission`
3. Default: `draft`

**Action:**

```bash
mkdir -p paper/audit
echo "<resolved-level>" > paper/audit/assurance.txt   # draft or submission
```

**What each level does downstream:**

- **`draft`** — Audits run only when their content detector matches
  (Phase 4.5 / 4.7 / 5.5 / 5.8). Missing artifacts are non-blocking, but
  skipped audits must be reported as `not run` or `not applicable`; silent
  skip is not allowed.
- **`submission`** — The three mandatory audits (proof-checker,
  experiment-claim-audit, citation-audit) are treated as load-bearing gates. Each
  sub-audit must emit its JSON artifact (PASS / WARN / FAIL / NOT_APPLICABLE /
  BLOCKED / ERROR) — never silent-skip. Phase 6 checks the required artifacts
  and blocks the Final Report on missing or blocking verdicts.

**Escape hatch:** a user wanting the old "beast = depth-only, no audit gate"
can pass `— effort: beast, assurance: draft` explicitly. Legal but
discouraged for actual submissions. See
`../shared-references/assurance-contract.md` for the full contract.

**Announce the resolved level in-line before Phase 1:**

```
📋 Assurance: <level> (derived from effort: <effort>)
   <either "current behavior, no audit gate" OR "mandatory audits gated by local artifact checks">
```

### Phase 1: Paper Plan

Invoke `/paper-plan` to create the structural outline:

```
/paper-plan "$ARGUMENTS"
```

**What this does:**
- Parse NARRATIVE_REPORT.md for claims, evidence, and figure descriptions
- Build a **Claims-Evidence Matrix** — every claim maps to evidence, every experiment supports a claim
- Design section structure (5-8 sections depending on paper type)
- Plan figure/table placement with data sources
- Scaffold citation structure
- The paper architect/reviewer checks the plan for completeness

**Output:** `PAPER_PLAN.md` with section plan, figure plan, citation scaffolding.

**Checkpoint:** Present the plan summary to the user.

```
📐 Paper plan complete:
- Title: [proposed title]
- Sections: [N] ([list])
- Figures: [N] auto-generated + [M] manual
- Target: [VENUE], [PAGE_LIMIT] pages

AUTO_PROCEED=true: continue to figure generation. If AUTO_PROCEED=false, ask whether to proceed.
```

- **User approves** (or AUTO_PROCEED=true) → proceed to Phase 2.
- **User requests changes** → adjust plan and re-present.

### Phase 2: Figure Generation

Invoke `/paper-figure` to generate data-driven plots and tables:

```
/paper-figure "PAPER_PLAN.md"
```

**What this does:**
- Read figure plan from PAPER_PLAN.md
- Generate matplotlib/seaborn plots from JSON/CSV data
- Generate LaTeX comparison tables
- Create `figures/latex_includes.tex` for easy insertion
- The paper architect/reviewer checks figure quality and captions

**Output:** `figures/` directory with PDFs, generation scripts, and LaTeX snippets.

> **Scope:** `paper-figure` covers data plots and comparison tables. Architecture diagrams, pipeline figures, and method illustrations are handled in Phase 2b below.

#### Phase 2b: Architecture & Illustration Generation

This migrated pack does not depend on external illustration skills. If the paper plan includes architecture diagrams, pipeline figures, audit cascades, or method illustrations:

1. Check whether the user already provided figure files in `figures/`.
2. If not, create a short `figures/MANUAL_FIGURES_NEEDED.md` list with figure ID, caption intent, required content, and source data.
3. Only create a simple local TikZ or LaTeX placeholder when the user explicitly asks for one.

All non-data figures must be reviewed by the paper architect/reviewer before Phase 3, but the paper executor should not invent visual content or claim generated assets exist.

**Checkpoint:** List generated vs manual figures.

```
📊 Figures complete:
- Data plots (auto, Phase 2): [list]
- Architecture/illustrations (auto, Phase 2b, mode=<illustration>): [list]
- Manual (need your input): [list]
- LaTeX snippets: figures/latex_includes.tex

[If manual figures needed]: log them in `figures/MANUAL_FIGURES_NEEDED.md`, continue with text placeholders, and report them in the Final Report unless the figure is essential to a core claim.
[If all auto]: AUTO_PROCEED=true: continue to Markdown drafting and LaTeX finalization. If AUTO_PROCEED=false, ask whether to proceed.
```

### Phase 3: Markdown Drafting and LaTeX Finalization

Invoke `/paper-write` to generate complete Markdown drafts first, then compile-ready LaTeX section files:

```
/paper-write "PAPER_PLAN.md"
```

**What this does:**
- Write each section as a complete Markdown draft in `paper/drafts/*.md`
- Preserve a problem-alignment note in each Markdown draft
- Convert each Markdown draft into one matching LaTeX file in `paper/sections/*.tex`
- Keep `main.tex` as the master file that inputs section files
- Insert figure/table references from `figures/latex_includes.tex`
- Build `references.bib` from citation scaffolding
- List stale files from previous section structures; archive/remove only files proven generated by the current workflow, otherwise ask first
- Automated bib cleaning (remove uncited entries)
- De-AI polish (remove "delve", "pivotal", "landscape"...)
- The paper architect/reviewer checks each section for quality

**Output:** `paper/` directory with `drafts/*.md`, `main.tex`, `sections/*.tex`, `references.bib`, `math_commands.tex`.

**Checkpoint:** Report section completion.

```
✍️ Markdown drafting and LaTeX finalization complete:
- Markdown drafts: [N] written in paper/drafts/
- LaTeX sections: [N] written in paper/sections/
- Citations: [N] unique keys in references.bib
- Stale files cleaned: [list, if any]

AUTO_PROCEED=true: continue to compilation. If AUTO_PROCEED=false, ask whether to proceed.
```

### Phase 4: Compilation

Invoke `/paper-compile` to build the PDF:

```
/paper-compile "paper/"
```

**What this does:**
- `latexmk -pdf` with automatic multi-pass compilation
- Auto-fix common errors (missing packages, undefined refs, BibTeX syntax)
- Up to 3 compilation attempts
- Post-compilation checks: undefined refs, page count, font embedding
- Precise page verification via `pdftotext`
- Stale file detection

**Output:** `paper/main.pdf`

**Checkpoint:** Report compilation results.

```
🔨 Compilation complete:
- Status: SUCCESS
- Pages: [X] (main body) + [Y] (references) + [Z] (appendix)
- Within page limit: YES/NO
- Undefined references: 0
- Undefined citations: 0

AUTO_PROCEED=true: continue to the improvement loop. If AUTO_PROCEED=false, ask whether to proceed.
```

### Phase 4.5: Proof Verification (theory papers only)

**Skip this phase if the paper contains no theorems, lemmas, or proofs.**

```
if paper contains \begin{theorem} or \begin{lemma} or \begin{proof}:
    Run /proof-checker "paper/"
    The paper architect/reviewer will:
    - Verify all proof steps (hypothesis discharge, interchange justification, etc.)
    - Check for logic gaps, quantifier errors, missing domination conditions
    - Attempt counterexamples on key lemmas
    - Generate PROOF_AUDIT.md with issue list + severity

    If FATAL or CRITICAL issues found:
        Fix before proceeding to improvement loop
    If only MAJOR/MINOR:
        Proceed, improvement loop may address remaining issues
else:
    skip — no proofs, no action
```

### Phase 4.7: Experiment Claim Audit

**Skip if no result files exist (e.g., survey/position papers with no experiments).**

```
if results/*.json or results/*.csv or outputs/*.json exist:
    Run /experiment-claim-audit "paper/"
    Fresh zero-context reviewer compares every number in the paper
    against raw result files. Catches rounding inflation, best-seed
    cherry-pick, config mismatch, delta errors.

    If FAIL:
        Fix mismatched numbers before improvement loop
    If WARN:
        Proceed, but flag for manual verification
else:
    skip — no experimental results to verify
```

### Phase 5: Review, Fix, and Polish Loop

Run up to `MAX_IMPROVEMENT_ROUNDS` review→fix→recompile rounds using the explicit review and polish skills below. Preserve facts, equations, numeric results, citations, labels, and LaTeX structure unless a reviewer finding proves they must change.

#### Phase 5.0: Paper Review

Invoke `/paper-review "paper/"` for a full-paper reviewer-style pass.

**What this does:**
- Identify CRITICAL / MAJOR / MINOR issues in problem framing, claim strength, method clarity, experiment support, section flow, limitations, and submission risk
- Separate fixable writing issues from scientific or evidence gaps
- Produce review notes for `PAPER_IMPROVEMENT_LOG.md`

If a finding requires a new experiment, claim replacement, citation replacement/removal, or author-level positioning decision, pause and surface the issue instead of auto-editing.

#### Phase 5.1: Minimal Fixes and Recompile

The paper executor applies only the smallest fixes supported by the review, then reruns `/paper-compile "paper/"`.

**Typical fixes:**
- Soften overclaims to match evidence
- Clarify notation, assumptions, and experiment interpretation
- Fix missing transitions or weak limitations
- Repair section-level inconsistencies without changing the scientific content

After each round, save the rebuilt PDF as `paper/main_round<N>.pdf` and append the review/fix summary to `paper/PAPER_IMPROVEMENT_LOG.md`.

#### Phase 5.2: Formal Academic Polish

After review-driven fixes compile cleanly, invoke the appropriate formal polish skill:

- Use `/paper-polish-workflow "paper/"` when the user wants staged structure→logic→expression refinement.
- Use `/paper-refine-special-en "paper/"` for high-intensity English LaTeX polish.
- Use `/paper-refine-special-zh "paper/"` for high-intensity Chinese paper polish.

Formal polish may improve clarity, academic tone, paragraph rhythm, and local flow. It must not add new claims, citations, experiments, numbers, formulas, or results.

#### Phase 5.3: Optional Human-Style Final Pass

Invoke `/paper-polish-human "paper/"` only after formal polishing is complete and only when the user wants a less mechanical final style. Intentional grammar imperfections are disabled unless the user explicitly enables grammar-error mode in the same request.

This pass is limited to eligible prose sections and must preserve facts, formulas, citations, labels, references, numeric values, and technical meaning.

#### Phase 5.4: Final Compile and Format Check

Run `/paper-compile "paper/"` again after all review and polish edits.

**Format check:** auto-detect and fix overfull hboxes, verify page count vs venue limit, ensure compact formatting, and confirm the final PDF opens. Location-aware thresholds: any main-body overfull blocks completion regardless of size; appendix overfulls block only if >10pt; bibliography overfulls block only if >20pt.

**Output:** `paper/main_round0_original.pdf`, per-round PDFs, final `paper/main.pdf`, and `paper/PAPER_IMPROVEMENT_LOG.md`.

### Phase 5.5: Final Experiment Claim Audit (MANDATORY submission gate)

After the review, fix, and polish loop finishes, **rerun** `/experiment-claim-audit` before the final report whenever the paper contains numeric claims and machine-readable raw result files exist.

Use the same detectors as Phase 4.7:
- numeric-claim regex over `paper/main.tex` and `paper/sections/*.tex`
- raw-evidence file search in `results/`, `outputs/`, `experiments/`, and `figures/` for `.json`, `.jsonl`, `.csv`, `.tsv`, `.yaml`, or `.yml`

This phase is **mandatory** if both detectors are positive. It blocks the final report.
If numeric claims exist but no raw result files are found, stop and warn the user before declaring the paper complete.
If no numeric claims exist, skip.

```bash
NUMERIC_CLAIMS=$(rg -n -e '[0-9]+(\.[0-9]+)?\s*(%|\\%|±|\\pm|x|×)' \
  -e '(accuracy|BLEU|F1|AUC|mAP|top-1|top-5|error|loss|perplexity|speedup|improvement)' \
  paper/main.tex paper/sections 2>/dev/null || true)

RAW_RESULT_FILES=$(find results outputs experiments figures -type f \
  \( -name '*.json' -o -name '*.jsonl' -o -name '*.csv' -o -name '*.tsv' -o -name '*.yaml' -o -name '*.yml' \) 2>/dev/null | head -200)

if [ -n "$NUMERIC_CLAIMS" ] && [ -n "$RAW_RESULT_FILES" ]; then
    Run /experiment-claim-audit "paper/"
    If FAIL:
        Fix mismatched numbers before the final report
elif [ -n "$NUMERIC_CLAIMS" ]; then
    Stop and warn: the paper contains numeric claims but no raw evidence files were found
fi
```

**Empirical motivation:** in a real submission run, the final paper claimed a narrower experiment grid than the raw JSON actually contained, and a tolerance value was rounded down past the actual relative error. Both were caught only after manual `experiment-claim-audit` invocation in the final round; the improvement loop did not detect them.

### Phase 5.8: Citation Audit (submission gate)

After the final experiment-claim-audit passes, run `/citation-audit` to verify every `\cite{...}` along three axes: existence, metadata correctness, and context appropriateness. This is the final layer of the evidence-and-claim assurance stack (`experiment-result-to-claim` → `experiment-claim-audit` → `citation-audit`).

```
if paper/references.bib (or paper.bib) exists and contains entries cited from sec/*.tex:
    Run /citation-audit "paper/"
    Fresh paper architect/reviewer with web/DBLP/arXiv lookup
    verifies each entry:
      (i)   EXISTENCE — paper resolves at claimed arXiv ID / DOI / venue
      (ii)  METADATA — author names, year, venue, title match canonical sources
      (iii) CONTEXT — cited paper actually establishes the claim it supports

    Output:
      - CITATION_AUDIT.md (human-readable per-entry verdict report)
      - CITATION_AUDIT.json (machine-readable verdict ledger)
      - Per-entry verdicts: KEEP / FIX / REPLACE / REMOVE

    If any REPLACE or REMOVE verdicts:
        Surface to user for human approval — never auto-modify content claims
    If only FIX verdicts (metadata corrections):
        Apply with user confirmation, then recompile
    If all KEEP:
        Pass — bibliography clean for submission
else:
    skip — no bib file or no citations
```

**Why this is the most diagnostic of the four audit layers:** wildly fake citations are easy to spot. The dangerous failure mode is a real paper used to support a claim it does not actually establish (wrong-context citations) — these slip past metadata-only checks and damage submission credibility. Run cost is wall-clock heavy (web lookup per entry); run once per submission, not per save.

**Empirical motivation:** in a real submission run, several real papers were cited in contexts they did not actually support, and at least one bib entry shipped with `author = "Anonymous"` because the metadata had not been resolved. None were caught by the improvement loop or numeric claim audit; only fresh web-lookup review surfaced them.

### Phase 6: Final Report

**Phase 6.0 — Submission Gate**

Before writing the Final Report, resolve the active assurance level. This
uses the **same derivation rule as Phase 0** so a run where Phase 0 was
skipped or its write failed cannot silently downgrade a `beast` / `max` /
`— assurance: submission` invocation back to draft.

**Resolution at the gate** (re-derive; do not trust `paper/audit/assurance.txt`
alone):

1. Parse `$ARGUMENTS` for an explicit `— assurance: draft | submission` or
   an `— effort: lite | balanced | max | beast` directive.
2. Derive the expected level:
   - explicit `assurance:` wins
   - else `lite` / `balanced` → `draft`, `max` / `beast` → `submission`
   - else `draft`
3. Read `paper/audit/assurance.txt`. If the file is missing, write it now
   with the derived level.
4. If the file's value **disagrees** with the derived level (e.g. file
   says `draft` but `$ARGUMENTS` says `beast`), **overwrite** the file
   with the derived level and surface a one-line warning in-chat:
   `⚠️ paper/audit/assurance.txt was draft but $ARGUMENTS says submission; overriding.`
5. Use the re-derived level as authoritative for the rest of Phase 6.

```bash
# Final authoritative value, written and read from the same source
ASSURANCE=<derived-from-$ARGUMENTS>        # draft | submission
mkdir -p paper/audit
echo "$ASSURANCE" > paper/audit/assurance.txt
```

If `ASSURANCE=draft`, skip directly to the Final Report template below —
**current behavior, no change** for the default `balanced` user.

If `ASSURANCE=submission`, run the pre-flight checklist below, then the
local artifact gate. The audit JSON artifacts are the source of truth — do
NOT self-declare "audits complete" based on conversation memory.

#### Submission pre-flight checklist

Print this checklist verbatim at the start of Phase 6.0 and confirm each row
before proceeding. This resists the common failure mode of the model
skipping audits while claiming to have run them.

```
📋 Submission audits required before Final Report:
   [ ] 1. /proof-checker        → paper/PROOF_AUDIT.json
   [ ] 2. /experiment-claim-audit    → paper/PAPER_CLAIM_AUDIT.json
   [ ] 3. /citation-audit       → paper/CITATION_AUDIT.json
   [ ] 4. Read all three JSON artifacts and check schema-relevant fields
   [ ] 5. Block Final Report iff any artifact is missing, stale by inspected hashes, malformed, or has FAIL / BLOCKED / ERROR
```

#### Invoking the three audits

Each sub-audit runs in a **fresh paper architect/reviewer context**.
Never pass prior audit output as context — this preserves reviewer
independence per `../shared-references/reviewer-independence.md`.

Each sub-audit **always** emits its JSON artifact, even when the content
detector is negative. A detector-negative run emits verdict
`NOT_APPLICABLE`; a silent skip is forbidden. See the "Submission artifact
emission" section of each audit's SKILL.md.

Order:

1. `/proof-checker "paper/"` → writes `paper/PROOF_AUDIT.json` (emits
   `NOT_APPLICABLE` if the paper contains no theorems / lemmas / proofs)
2. `/experiment-claim-audit "paper/"` → writes `paper/PAPER_CLAIM_AUDIT.json`
   (emits `NOT_APPLICABLE` if the paper has no numeric claims; emits
   `BLOCKED` if numeric claims exist but raw result files are missing)
3. `/citation-audit "paper/"` → writes `paper/CITATION_AUDIT.json`
   (emits `NOT_APPLICABLE` if no `.bib` file or no `\cite{...}` usage)

#### Running the local artifact gate

Read `paper/PROOF_AUDIT.json`, `paper/PAPER_CLAIM_AUDIT.json`, and
`paper/CITATION_AUDIT.json` directly. Confirm each has:

- `audit_skill`
- `verdict`
- `reason_code`
- `summary`
- `audited_input_hashes`
- `trace_path`
- `generated_at`

Proceed to the Final Report only when all mandatory artifacts exist, their
declared input hashes match the current files you inspected, and no verdict is
`FAIL`, `BLOCKED`, or `ERROR`. If any check fails, refuse to mark the PDF
submission-ready and list the concrete rerun or fix needed.

---

**Phase 6.1 — Final Report** (runs only after the submission gate is green,
or directly if `assurance=draft`)

```markdown
# Paper Writing Pipeline Report

**Input**: [NARRATIVE_REPORT.md or topic]
**Venue**: [ICLR/NeurIPS/ICML/CVPR/ACL/AAAI/ACM/IEEE_JOURNAL/IEEE_CONF]
**Assurance**: [draft | submission]
**Submission-ready**: [yes | no]   <!-- yes iff assurance=submission AND the local artifact gate passes -->
**Date**: [today]

## Pipeline Summary

| Phase | Status | Output |
|-------|--------|--------|
| 0. Assurance Setup | ✅ | paper/audit/assurance.txt = [draft\|submission] |
| 0.5 Autonomy Log | ✅ | paper/AUTONOMY_LOG.md ([N] assumptions / [M] unresolved items) |
| 1. Paper Plan | ✅ | PAPER_PLAN.md |
| 2. Figures | ✅ | figures/ ([N] auto + [M] manual) |
| 3. Markdown Drafting | ✅ | paper/drafts/*.md ([N] drafts, problem-aligned) |
| 3b. LaTeX Finalization | ✅ | paper/sections/*.tex ([N] sections, [M] citations) |
| 4. Compilation | ✅ | paper/main.pdf ([X] pages) |
| 5.0 Paper Review | ✅ | paper-review findings logged |
| 5.1 Minimal Fixes | ✅ | [score0]/10 → [scoreN]/10, recompiled PDFs |
| 5.2 Formal Polish | ✅ | [paper-polish-workflow \| paper-refine-special-en \| paper-refine-special-zh] |
| 5.3 Human Polish | [DONE\|SKIPPED] | paper-polish-human only if requested |
| 5.4 Final Compile | ✅ | paper/main.pdf ([X] pages), overfull/page checks |
| 4.5 Proof Audit | [PASS\|WARN\|FAIL\|NOT_APPLICABLE\|BLOCKED\|ERROR] | PROOF_AUDIT.{md,json} |
| 5.5 Experiment Claim Audit | [PASS\|WARN\|FAIL\|NOT_APPLICABLE\|BLOCKED\|ERROR] | PAPER_CLAIM_AUDIT.{md,json} |
| 5.8 Citation Audit | [PASS\|WARN\|FAIL\|NOT_APPLICABLE\|BLOCKED\|ERROR] | CITATION_AUDIT.{md,json} |
| 6.0 Assurance Artifact Gate | [OK\|STALE\|BLOCKING_VERDICT\|HAS_ISSUES\|SCHEMA_INVALID\|MISSING] per audit (N/A if draft) | audit JSON artifacts |

## Improvement Scores
| Round | Score | Key Changes |
|-------|-------|-------------|
| Round 0 | X/10 | Baseline |
| Round 1 | Y/10 | [summary] |
| Round 2 | Z/10 | [summary] |

## Deliverables
- paper/main.pdf — Final polished paper
- paper/drafts/*.md — Complete Markdown drafts, one file per section
- paper/main_round0_original.pdf — Before improvement
- paper/main_round1.pdf — After round 1
- paper/main_round2.pdf — After round 2
- paper/PAPER_IMPROVEMENT_LOG.md — Full review log
- paper/AUTONOMY_LOG.md — Conservative assumptions, non-blocking uncertainty, manual follow-ups, and items not confirmed by the user
- Formal polish log — Formal academic polishing changes and preservation checks
- Human polish log — Optional final naturalness pass, including explicit grammar-error log only when grammar-error mode was enabled
- paper/PROOF_AUDIT.{md,json} — Proof-obligation verification (always emitted at `assurance=submission`; `NOT_APPLICABLE` when no theorems)
- paper/PAPER_CLAIM_AUDIT.{md,json} — Numerical claim verification (always emitted at `assurance=submission`; `NOT_APPLICABLE` when no numeric claims; omitted in `draft` mode if Phase 5.5 detector was negative)
- paper/CITATION_AUDIT.{md,json} — Bibliography verification (always emitted at `assurance=submission`; `NOT_APPLICABLE` when no `.bib` or no `\cite{...}`; omitted in `draft` mode if Phase 5.8 detector was negative)
- audit JSON artifacts — local submission gate evidence (submission only)

## Remaining Issues (if any)
- [items from final review that weren't addressed]

## Assumptions and Unconfirmed Items
- [items copied from paper/AUTONOMY_LOG.md, including whether each item is blocking or non-blocking]

## Next Steps
- [ ] Visual inspection of PDF
- [ ] Add any missing manual figures
- [ ] Submit to [venue] via OpenReview / CMT / HotCRP
```

## Output Protocols

> Follow these shared protocols for all output files:
> - **[Output Versioning Protocol](../shared-references/output-versioning.md)** — write timestamped file first, then copy to fixed name
> - **[Output Manifest Protocol](../shared-references/output-manifest.md)** — log pipeline artifacts and generated overwrites to MANIFEST.md
> - **[Output Language Protocol](../shared-references/output-language.md)** — note: paper-writing always outputs English LaTeX for venue submission

## Key Rules

- **Large file handling**: If a large output cannot be written in one pass, use the available project-safe file editing mechanism to write it in smaller chunks and then verify the final file.
- **Don't skip phases.** Each phase builds on the previous one — skipping leads to errors.
- **Checkpoint between phases** when AUTO_PROCEED=false. Present results and wait for approval.
- **Manual figures are logged, not always blocking.** If the paper needs architecture diagrams or qualitative results, create `figures/MANUAL_FIGURES_NEEDED.md` and continue with text placeholders unless the missing figure is essential to a core claim.
- **Compilation must succeed** before entering the improvement loop. Fix all errors first.
- **Markdown first, LaTeX second.** Phase 3 must create complete `paper/drafts/*.md` before finalizing `paper/sections/*.tex`.
- **Problem-first writing.** The plan must define 2-3 central problems, and the Introduction, Related Work, Method, and Results must align to them.
- **Review is explicit.** Phase 5 must invoke `paper-review`; do not replace it with an informal self-check.
- **Formal polish precedes human polish.** Use `paper-polish-human` only after formal polishing, and keep grammar-error mode disabled unless explicitly requested.
- **Pause only on blocking scientific changes.** Do not auto-change core claims, add experiments, replace/remove citations, delete uncertain files, install dependencies, or invent evidence during review or polish. Non-blocking uncertainty goes to `paper/AUTONOMY_LOG.md`.
- **Preserve all PDFs.** The user needs round0/round1/round2 for comparison.
- **Document autonomy decisions.** The pipeline report should be self-contained and include assumptions, unresolved issues, skipped checks, and user-confirmation gaps.
- **Respect page limits.** If the paper exceeds the venue limit, suggest specific cuts before the improvement loop.

## Composing with Other Workflows

```
/paper-plan "materials or topic"       ← plan claims, evidence, sections, and figures
/experiment-plan "method idea"         ← design claim-driven experiments
/experiment-result-to-claim "results"  ← confirm, supplement, or pivot claims
/experiment-ablation-planner           ← plan focused ablations when needed
/paper-writing "NARRATIVE_REPORT.md"   ← generate Markdown drafts, LaTeX sections, and verified PDF
                                          submit! 🎉
/paper-review "paper/"                ← explicit reviewer-style improvement pass
/paper-polish-workflow "paper/"       ← staged formal polish after review fixes
/paper-polish-human "paper/"          ← optional final human-style pass after formal polish
```

## Typical Timeline

| Phase | Duration | Background-friendly? |
|-------|----------|------------|
| 1. Paper Plan | 5-10 min | No |
| 2. Figures | 5-15 min | No |
| 3. Markdown + LaTeX writing | 20-40 min | Yes ✅ |
| 4. Compilation | 2-5 min | No |
| 5. Review + polish | 20-45 min | Yes ✅ |
| 5.5/5.8 Audits | 10-60 min | Yes ✅ |

**Total: ~60-150 min** for a full paper from narrative report to polished, reviewed PDF. Submission-mode citation and claim audits may take longer when the bibliography or result tables are large.
