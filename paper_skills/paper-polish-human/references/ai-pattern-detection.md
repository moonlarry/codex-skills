# AI Pattern Detection for Academic Papers

## Scope and Boundary

This reference is a post-polish diagnostic aid for `paper-polish-human`. It should be used only after the manuscript has already received formal academic polishing through `paper-polish-workflow`, `paper-refine-special-en`, `paper-refine-special-zh`, or an equivalent author-led revision.

The patterns below are weak prose signals, not proof of AI authorship. They are used to identify wording that may sound mechanical, inflated, generic, or insufficiently grounded in evidence.

Revisions must preserve the manuscript's claims, citations, equations, terminology, section structure, reported numbers, and evidential strength. If reducing an AI-like pattern would make the sentence less precise, less conventional for the target venue, or less faithful to the original evidence, keep the original wording.

This reference does not validate factual claims, audit citations, check experimental numbers, or restructure the paper's argument. Use `citation-audit`, `experiment-claim-audit`, `paper-polish-workflow`, or the relevant refinement skill for those tasks.

**Do not optimize prose for AI detectors; optimize for accurate, evidence-calibrated academic writing.**

## Academic Humanization Priority

Preserve correctness and scholarly convention first; reduce only generic, unsupported, or stylistically unsafe AI-like patterns second.

## How to Use This Reference

- Patterns are weak signals. Single-word hits rarely justify revision.
- Prefer precision over anti-AI rewriting.
- Revise only when the pattern harms clarity, evidence fit, or academic tone.
- Combine patterns to raise confidence before acting.

## Context Detection

Before applying patterns, detect the document context:

| Context | Pattern Scope | Notes |
| --- | --- | --- |
| **Regular paper** | All patterns except REBUTTAL_DIFF_ANCHORING | Introduction, Related Work, Results sections only |
| **Rebuttal / Revision letter / Response to reviewers** | All patterns including REBUTTAL_DIFF_ANCHORING | Full document scope; diff-anchored writing is acceptable here |

### Positive triggers for rebuttal/revision context

Detect rebuttal/revision context when the document contains:
- Explicit headers: "Response to Reviewers", "Rebuttal", "Revision Letter", "Author Response"
- Reviewer references: "Reviewer #1", "R1", "C2", "Comment 3"
- Revision action phrases: "We have revised", "As requested by the reviewer", "In response to R1", "We clarify this point"
- Document structure matching rebuttal format (point-by-point response, numbered comments)

### Negative triggers (NOT rebuttal/revision context)

Do NOT classify as rebuttal/revision context when:
- The document is a revised manuscript (the paper itself, not the letter explaining changes)
- The document contains normal paper sections: Introduction, Method, Results, Discussion, Conclusion
- "revised" appears only as version metadata, not as explanation of changes to reviewers
- No reviewer comments or response structure is present

### Mixed-document rule

If a single document contains both rebuttal content and paper sections:
- Apply REBUTTAL_DIFF_ANCHORING only to the rebuttal/response sections
- Apply regular paper patterns (excluding REBUTTAL_DIFF_ANCHORING) to paper content sections
- Use section headers and content type to determine local context

Apply REBUTTAL_DIFF_ANCHORING only when the document context is clearly rebuttal/revision letter. Do NOT apply it to regular manuscript sections or revised manuscripts.

## Pattern Index

| ID | Pattern | Typical Cue | Default Action |
| --- | --- | --- | --- |
| VOCAB_DENSITY | AI vocabulary density | pivotal, underscore, showcase | Replace with standard academic terms |
| COPULA_PROXY | Copula avoidance | serves as, stands as, boasts | Use is/are when precise |
| SIG_INFLATION | Significance inflation | testament, pivotal moment, broader impact | Remove or state factually |
| SUPERFICIAL_ING | Superficial -ing analysis | highlighting, underscoring, reflecting | Remove or expand with evidence |
| PUFFERY | Promotional language | groundbreaking, profound, renowned | Replace with measured terms |
| VAGUE_ATTRIBUTION | Vague attribution | Experts believe, many studies show | Use specific citations or remove |
| OVERGENERALIZED_CONSENSUS | Overgeneralized consensus | It is widely accepted that | Check if 1-2 papers are overstated as consensus |
| CONTRAST_TEMPLATE | Contrast-reframe template | not only X but also Y | State claims directly |
| TRIADIC_LISTING | Rule of three | innovation, inspiration, and insights | Use natural number of items |
| ELEGANT_VARIATION | Synonym cycling | protagonist → main character → hero | Repeat key terms for clarity |
| FORMULAIC_DISCUSSION | Formulaic challenge/future | Despite these challenges... future prospects | Remove template or state specifics |
| MECHANICAL_TRANSITIONS | Mechanical paragraph openings | Furthermore, Moreover, Additionally in sequence | Vary or fuse sentences |
| AGENTLESS_PASSIVE_CHAIN | Passive fragment chains | It was observed. The results were preserved. | Restore agent when obscured |
| NONTECH_HYPHEN_OVERLOAD | Decorative hyphen stacking | cross-functional, data-driven, client-facing | Reduce non-technical hyphens |
| PERSUASIVE_TROPE | Fake depth phrases | at its core, fundamentally, the real question is | Remove or replace with evidence |
| BLOG_STYLE_SIGNPOSTING | Tutorial-style announcements | Let's dive in, here's what you need to know | Remove or convert to academic roadmapping |
| EMPTY_SECTION_OPENER | Header restatement | Section title followed by empty one-liner | Delete or add substance |
| REBUTTAL_DIFF_ANCHORING | Diff-anchored writing | This function was added to replace... | Apply ONLY in rebuttal/revision context |

## Applicable Patterns

### VOCAB_DENSITY: Vocabulary Density

**Flag when**: High concentration of post-2023 AI-associated words: additionally, align with, crucial, delve, emphasizing, enduring, enhance, fostering, garner, highlight, interplay, intricate, key, landscape, pivotal, showcase, tapestry, testament, underscore, valuable, vibrant.

**Safe revision**: Replace with standard academic equivalents or remove when filler.
- "Additionally" → "Also" or merge into previous sentence
- "showcase" → "demonstrate" or "present"
- "underscore" → remove or use direct statement

**Do not flag when**: The word is a precise technical term or standard academic expression in context.

**Mini example**:
> Before: "Additionally, this approach showcases the pivotal role of attention mechanisms."
> After: "This approach demonstrates that attention mechanisms are central to performance."

---

### COPULA_PROXY: Copula Avoidance

**Flag when**: Substituting "is/are/has" with elaborate constructions where no genuine action or role nuance is conveyed: serves as, stands as, marks, represents, boasts, features, offers.

**Safe revision**: Use simple copula for definitional statements; keep proxy verbs when they convey genuine action or role nuance.
- "serves as a testament" → remove or restate factually
- "boasts 3,000 square feet" → "has 3,000 square feet"

**Do not flag when**: "serves as" accurately describes functional role, e.g., "serves as the primary interface".

**Mini example**:
> Before: "The framework serves as a foundation for future research."
> After: "The framework provides a basis for future research."

---

### SIG_INFLATION: Significance Inflation

**Flag when**: Elevating ordinary findings into legacy/broader-impact claims: testament, pivotal moment, key turning point, evolving landscape, focal point, indelible mark, deeply rooted, marking a shift, shaping the future.

**Safe revision**: Remove the inflation layer; keep the factual core.
- "marking a pivotal moment in the evolution of X" → remove
- "stands as a testament to the potential of Y" → remove or restate Y's actual contribution

**Do not flag when**: The contribution is genuinely significant and the evidence supports the claim.

**Mini example**:
> Before: "This work stands as a testament to the transformative potential of multimodal learning."
> After: "This work demonstrates that multimodal learning can improve accuracy on benchmark X."

---

### SUPERFICIAL_ING: Superficial -ing Analysis

**Flag when**: Present participle phrases tacked onto sentence ends to add fake depth without new information: highlighting, underscoring, emphasizing, ensuring, reflecting, symbolizing, contributing to, cultivating, fostering, encompassing, showcasing.

**Safe revision**: Remove the -ing phrase if it adds no new information; expand with concrete evidence if the connection is real.
- "highlighting the importance of X" → remove or cite specific evidence for X's importance
- "reflecting the broader trend" → remove or name the specific trend

**Do not flag when**: The -ing phrase genuinely introduces a supported follow-up point.

**Mini example**:
> Before: "The results improved by 12%, underscoring the effectiveness of our approach."
> After: "The results improved by 12%." (remove if 12% already shows effectiveness)

---

### PUFFERY: Promotional Language

**Flag when**: Marketing-like descriptors inappropriate for neutral academic prose: groundbreaking, profound, renowned, vibrant, breathtaking, must-visit, stunning, seamless, intuitive, game-changing, revolutionary.

**Safe revision**: Replace with measured academic terms or remove.
- "groundbreaking approach" → "new approach" or name the specific novelty
- "seamless integration" → "integration" or describe the mechanism

**Do not flag when**: The results genuinely justify strong terms with evidence.

**Mini example**:
> Before: "Our groundbreaking framework delivers seamless, intuitive experiences."
> After: "Our framework combines X and Y to reduce latency by 30%."

---

### VAGUE_ATTRIBUTION: Vague Attribution

**Flag when**: Claims attributed to unspecified authorities without specific sources: Experts believe, Industry reports show, Many studies suggest, It is widely recognized, Scholars argue.

**Safe revision**: Replace with specific citations or remove if unverifiable.
- "Experts believe X plays a crucial role" → cite specific paper(s) or remove
- "Many studies have shown" → cite representative studies or quantify

**Do not flag when**: The claim is a genuine consensus with adequate citation support, or the attribution style is venue-appropriate.

**Mini example**:
> Before: "Experts believe that attention mechanisms are crucial for performance."
> After: "Vaswani et al. (2017) showed that attention mechanisms improve translation quality."

---

### OVERGENERALIZED_CONSENSUS: Overgeneralized Consensus

**Flag when**: One or two papers presented as universal agreement: It is widely accepted that, The community agrees, There is consensus that.

**Safe revision**: Check if the cited evidence actually represents broad consensus. If not, narrow the claim.
- "It is widely accepted that X causes Y" → "Smith (2020) and Jones (2021) argue that X causes Y"

**Do not flag when**: Broaden a claim beyond what the cited evidence supports.

---

### CONTRAST_TEMPLATE: Contrast-Reframe Template

**Flag when**: Overused "not only X but also Y" or "It's not just X, it's Y" constructions.

**Safe revision**: State the combined claim directly without the template.
- "not only improves accuracy but also reduces latency" → "improves accuracy and reduces latency"

**Do not flag when**: The contrast genuinely serves rhetorical emphasis and isn't overused nearby.

---

### TRIADIC_LISTING: Rule of Three Overuse

**Flag when**: Forcing items into groups of three for apparent comprehensiveness when the natural count differs.

**Safe revision**: Use the natural number of items (two, four, or five if accurate).
- "innovation, inspiration, and insights" → remove or use actual items

**Do not flag when**: Three is genuinely the correct enumeration.

---

### ELEGANT_VARIATION: Synonym Cycling

**Flag when**: AI's repetition-penalty causing excessive synonym substitution that harms term consistency.

**Safe revision**: Repeat key technical terms when clarity demands it.
- "The model... The architecture... The system... The framework..." → use "The model" consistently if it's the central subject

**Do not flag when**: Introduce synonyms that confuse the reader about whether a new entity is being introduced.

---

### FORMULAIC_DISCUSSION: Formulaic Discussion/Future Paragraphs

**Flag when**: Template "Challenges and Future Prospects" sections: "Despite these challenges... the future looks bright... exciting times lie ahead..."

**Safe revision**: Remove the template; state specific challenges and concrete next steps.
- "Despite challenges, the system continues to thrive" → name specific challenges and mitigation efforts

**Do not flag when**: Legitimate challenges or future work sections that are venue-appropriate.

---

### MECHANICAL_TRANSITIONS: Mechanical Paragraph Openings

**Flag when**: Dense sequence of template transition words starting paragraphs: Furthermore, Moreover, Additionally, In addition, Specifically, In particular, Overall, In summary.

**Safe revision**: Vary openings; fuse sentences when flow allows.
- Three consecutive paragraphs starting with "Furthermore/Moreover/Additionally" → restructure

**Do not flag when**: Transitions genuinely aid logical flow.

---

### AGENTLESS_PASSIVE_CHAIN: Passive Fragment Chains

**Flag when**: Consecutive passive sentences or subjectless fragments obscure the agent, method, or responsibility, e.g., "It was observed. The results were preserved. No configuration needed."

**Safe revision**: Restore the agent when clarity demands it, or convert to active voice when the actor is known and relevant.
- "It was observed that X increased" → "We observed that X increased" (if author is the actor)
- "The model was trained on Y" → PRESERVE (normal methods style)

**Do not flag when**: Normal methods/experiments passive voice, e.g., "The model was trained...", "Experiments were conducted..."

---

### NONTECH_HYPHEN_OVERLOAD: Non-Technical Hyphen Overload

**Flag when**: Decorative, marketing-style, or stacked hyphenated adjectives in non-technical prose, e.g., "a cross-functional, data-driven, client-facing approach delivering high-quality, end-to-end solutions."

**Safe revision**: Reduce non-technical hyphen pairs; drop hyphens when the compound follows the noun.
- "high-quality report" → preserve (attributive position)
- "the report is high-quality" → "the report is high quality" (predicate position)

**Do not flag when**: Technical terms, model/dataset names, range notation, LaTeX notation, CLI flags, file paths: state-of-the-art, zero-shot, end-to-end (as method name), long-tailed, real-time.

---

### PERSUASIVE_TROPE: Persuasive Authority Tropes

**Flag when**: Phrases that pretend to cut through to a deeper truth: at its core, fundamentally, the real question is, what really matters, the heart of the matter, in reality, the deeper issue.

**Safe revision**: Remove the trope; state the point directly with evidence.
- "At its core, what really matters is X" → "X is relevant because [evidence]."

**Do not flag when**: Contribution or conclusion statements backed by experimental evidence.

---

### BLOG_STYLE_SIGNPOSTING: Blog-Style Signposting

**Flag when**: Tutorial-script or blog-style announcements: Let's dive into, Here's what you need to know, We will walk through, Without further ado, Now let's look at.

**Safe revision**: Remove the announcement; start directly with content, or use standard academic roadmapping.
- "Let's dive into how caching works" → "Caching works as follows:"
- "Here's what you need to know" → REMOVE

**Do not flag when**: Normal academic roadmapping, e.g., "Section 3 presents the methodology", "We first review related work", proof outlines, contribution overviews.

---

### EMPTY_SECTION_OPENER: Empty Section Opener

**Flag when**: A section heading followed by a one-line paragraph that merely restates the heading with no new information, e.g., "## Performance\n\nSpeed matters."

**Safe revision**: Delete the empty opener; let the heading stand alone, or replace with substantive content.
- "## Performance\n\nSpeed matters.\n\nWhen users..." → "## Performance\n\nWhen users..."

**Do not flag when**: Necessary scope-setting, assumption declaration, definition introduction, or legitimate transition sentences.

---

### REBUTTAL_DIFF_ANCHORING: Diff-Anchored Writing (Rebuttal Context Only)

**Flag when**: Documentation or comments written as if narrating a change rather than describing the current state: "This function was added to replace...", "Previously, X did Y, now it does Z."

**Safe revision**: Describe the current method/functionality directly rather than narrating the change history.
- "This function was added to replace the previous approach of iterating through all items" → "This function uses a hash map for O(1) lookups, avoiding the O(n²) cost of naive iteration"

**Do not flag when**: The document is a rebuttal, revision letter, or response to reviewers. In these contexts, diff-anchored writing is appropriate and expected—explaining what changed and why is the purpose of the document.

**Context trigger**: Apply this pattern ONLY when the document context is detected as rebuttal/revision letter. Do NOT apply to regular manuscript sections.

---

## Pattern Combination Guidance

Single patterns are weak signals. Combine for higher confidence:

- **Low confidence**: Single word or phrase hit, sentence still accurate → preserve unless it harms tone.
- **Medium confidence**: Two patterns in the same paragraph, e.g., VOCAB_DENSITY + SUPERFICIAL_ING → consider revision.
- **High confidence**: Pattern combination creates empty inflation, evidence mismatch, or template feel, e.g., SIG_INFLATION + VAGUE_ATTRIBUTION + PUFFERY → revise.

## Dash Guidance in Academic LaTeX

Do not adopt a blanket "cut all dashes" rule. LaTeX academic text has legitimate uses.

### Preserve

- `--` (en dash) for ranges or relations: `2019--2024`, `pp. 12--15`, `pre--post comparison`
- `-` (hyphen) for compound terms, technical names, labels, file names, variables, dataset/model names
- `---` (em dash) in quoted text, titles, or clearly intentional authorial emphasis

### For body-prose em dashes (`---`)

- Do not ADD new em dashes during humanization.
- If an em dash creates a decorative or blog-like aside, prefer a comma, semicolon, colon, or parentheses.
- Replace only when meaning, citation scope, and sentence logic remain unchanged.

### Never rewrite dashes inside

- LaTeX commands or options
- Citations, labels, refs, BibTeX entries
- Equations and math expressions
- Code blocks, pseudocode, CLI flags, file paths, URLs, DOIs
- Dataset/model names and technical compound terms

**Important**: Do not convert LaTeX dash notation (`--`, `---`) into Unicode punctuation in `.tex` files unless explicitly requested.

## What NOT to Flag (Academic Context)

Do not flag a surface pattern as AI-like unless it weakens precision, introduces generic rhetoric, obscures agency, or departs from the manuscript's academic context.

### Preserve: LaTeX and Source Integrity

Never rewrite:
- `\cite{}`, `\ref{}`, `\label{}`, `\textit{}`, `\emph{}` and other LaTeX commands
- Equations, theorem/proof blocks, algorithm pseudocode
- BibTeX keys, table/figure references
- Dataset/model names, CLI flags, URLs, DOIs
- Range notation: `2019--2024`, `pp. 12--15`

### Preserve: Technical Expression

- Technical term consistency and repetition (papers require unified naming)
- Passive voice in methods/experiments: "The model was trained..." is normal academic style
- Fixed technical phrases: state-of-the-art, zero-shot, end-to-end, long-tailed, real-time, few-shot, multi-modal
- Precise but plain result descriptions (tables, metrics, datasets, ablation results)

### Preserve: Academic Structure

- Careful hedging: may, suggest, indicate, appear to (academic hedging is necessary)
- Contribution lists, experiment setup lists (venue conventions)
- Citation-dense sentences, formulaic definitions, theorem/proof structures
- Normal academic roadmapping: "Section 3 presents...", "We first review..."

### When it may still be flagged

- Hedging stacked without evidence alternatives
- Passive chains that hide key agents or responsibility
- Signposting that is empty, blog-style, or merely restates the section title
- Technical term repetition that is mechanical within one sentence or paragraph
- "state-of-the-art" and similar terms without experiment or citation support

### Do NOT introduce during humanization

- Anecdotes, emotions, unsupported background, stronger conclusions
- Decorative dashes or persuasive tropes
- New claims, examples, citations, limitations, motivations, or causal explanations not present in the manuscript

## Excluded or Out-of-Scope Patterns

These are not within `paper-polish-human` scope:

- **Wikipedia-specific**: wikitext, templates, categories, AfC drafts, talk-page behavior
- **Citation integrity**: broken links, invalid/unrelated DOI, unused references → use `citation-audit`
- **Chatbot artifacts**: "Great question!", "I hope this helps!", knowledge cutoff disclaimers → delete if present, but not a core pattern
- **Human-writing signs**: text age, author explanation ability → not applicable

## Detection Principles

1. Accuracy first: Never sacrifice precision for anti-AI styling.
2. Evidence-calibrated: Match language strength to evidence strength.
3. Venue-appropriate: Respect field and journal conventions.
4. Minimal intervention: Prefer removal over elaborate replacement.
5. Context-aware: Apply REBUTTAL_DIFF_ANCHORING only in rebuttal/revision letter context.