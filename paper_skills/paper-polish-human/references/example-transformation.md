# Example Transformations

These examples show how `paper-polish-human` applies AI pattern detection within authorial stance guardrails.

## Introduction Example

**Before (formally polished, AI-sounding)**:

> Furthermore, our proposed framework demonstrates robust performance across diverse scenarios, marking a pivotal advancement in the evolving landscape of embodied AI. Additionally, the experimental results underscore the crucial role of attention mechanisms in achieving state-of-the-art outcomes. This work serves as a testament to the potential of integrating multimodal signals, highlighting the broader implications for future research.

**After (human-style polished)**:

> Our framework performs consistently across scenarios. The experiments show that attention mechanisms are central to performance—removing them drops accuracy by 8%. Combining visual and tactile signals improved results, though the gain (12%) is modest compared to the computational overhead.

**Patterns addressed**:
- MECHANICAL_TRANSITIONS: "Furthermore/Additionally" sequence → removed
- VOCAB_DENSITY: "demonstrates robust" → "performs consistently"
- SIG_INFLATION: "marking a pivotal advancement", "evolving landscape" → removed
- VOCAB_DENSITY: "underscore the crucial role" → "are central to"
- SUPERFICIAL_ING: "highlighting the broader implications" → removed
- COPULA_PROXY: "serves as a testament" → removed

**Why this is safe**:
- No new claims added: the 8% drop and 12% gain were in the original results section
- Trade-off acknowledgment: the original limitation section mentioned computational overhead
- Calibrated confidence: "modest" matches the original author's hedged interpretation

**Claim safety verified**: no new claims, numbers, citations, limitations, or comparisons added.

---

## Related Work Example

**Before (formally polished, AI-sounding)**:

> It is widely accepted that transformer architectures have revolutionized natural language processing. Many studies have demonstrated their effectiveness across diverse tasks. Smith et al. (2020) and Jones (2021) pioneered this direction, and their groundbreaking work stands as a foundation for subsequent research. Notably, these approaches not only improve accuracy but also reduce training time, showcasing the intricate interplay between model architecture and computational efficiency.

**After (human-style polished)**:

> Transformer architectures now dominate NLP. Vaswani et al. (2017) introduced the design; subsequent work has applied it to translation, summarization, and question answering. Smith et al. (2020) showed accuracy gains on long documents; Jones (2021) reported faster training on parallel GPU setups. The accuracy and speed gains come from parallelized attention, though the memory cost grows with sequence length.

**Patterns addressed**:
- OVERGENERALIZED_CONSENSUS: "It is widely accepted" → removed, attributed to specific papers
- VAGUE_ATTRIBUTION: "Many studies have demonstrated" → named representative studies
- PUFFERY: "groundbreaking work" → removed
- COPULA_PROXY: "stands as a foundation" → removed
- CONTRAST_TEMPLATE: "not only...but also" → stated directly
- VOCAB_DENSITY: "showcase the intricate interplay" → removed
- PERSUASIVE_TROPE: implicit "at its core" framing → removed

**Why this is safe**:
- Citations added were from original reference list (Vaswani, Smith, Jones)
- Memory cost limitation was in original experimental setup section
- No new comparative claims beyond what original results reported

**Claim safety verified**: no new claims, numbers, citations, limitations, or comparisons added.

---

## Results Example

**Before (formally polished, AI-sounding)**:

> Our method achieves a remarkable 94.5% accuracy on benchmark X, significantly outperforming all baseline approaches. This profound improvement underscores the effectiveness of our design choices. Furthermore, the results showcase the vital importance of multimodal fusion, reflecting a broader trend in the field. Despite challenges in real-world deployment, the future looks bright for this line of research.

**After (human-style polished)**:

> Our method achieves 94.5% accuracy on benchmark X. The strongest baseline (Method Y) scored 89.2%. The 5.3-point gap comes primarily from the multimodal fusion component; ablating it drops accuracy to 90.1%. Deployment challenges remain: the model requires 4GB GPU memory and runs at 12ms per inference, which may limit edge-device use.

**Patterns addressed**:
- PUFFERY: "remarkable", "profound improvement" → removed
- VOCAB_DENSITY: "significantly outperforming", "underscores" → stated with numbers
- SUPERFICIAL_ING: "reflecting a broader trend" → removed
- SIG_INFLATION: "vital importance", "future looks bright" → removed
- FORMULAIC_DISCUSSION: "Despite challenges... future looks bright" → replaced with specific challenges
- MECHANICAL_TRANSITIONS: "Furthermore" → removed

**Why this is safe**:
- Baseline comparison (Method Y, 89.2%) was in original results table
- Ablation numbers (90.1%) were in original ablation section
- GPU memory (4GB) and latency (12ms) were in original implementation details
- Edge-device limitation was in original discussion section

**Claim safety verified**: no new claims, numbers, citations, limitations, or comparisons added.

---

## Rebuttal / Revision Letter Example

**Context**: This example applies to rebuttal, revision letter, or response to reviewers context. The REBUTTAL_DIFF_ANCHORING pattern is NOT flagged here because diff-anchored writing is appropriate in this context.

**Before (AI-sounding rebuttal)**:

> We thank the reviewer for their insightful comments. Furthermore, we have addressed all the concerns raised. This revision marks a pivotal improvement in our work, showcasing our commitment to excellence. Additionally, we added a new experiment that demonstrates the robustness of our approach, underscoring its vital importance to the field.

**After (human-style polished rebuttal)**:

> We thank the reviewer for the comments. We have revised Section 3 to clarify the training procedure and added an ablation experiment (Table 4) that shows the fusion component contributes 5.3 percentage points to accuracy. The original wording about "robustness" has been replaced with the specific numbers from that experiment.

**Patterns addressed**:
- MECHANICAL_TRANSITIONS: "Furthermore/Additionally" → removed
- VOCAB_DENSITY: "demonstrates robust", "underscores vital importance" → replaced with specific numbers
- SIG_INFLATION: "marks a pivotal improvement" → removed
- PUFFERY: "showcasing our commitment to excellence" → removed
- BLOG_STYLE_SIGNPOSTING: implicit announcement → removed

**Patterns NOT flagged in rebuttal context**:
- REBUTTAL_DIFF_ANCHORING: NOT applied because diff-anchored writing ("we added", "revised Section 3", "original wording has been replaced") is appropriate in rebuttal/revision letter context.

**Why this is safe**:
- The 5.3 percentage points number comes from the new ablation experiment
- "Table 4" is the actual table added in the revision
- No overclaiming beyond what the new experiment shows

**Claim safety verified**: no new claims beyond the documented revision changes.

---

## Key Principles Demonstrated

1. **Numbers come from original text**: All concrete figures in After versions existed in original sections.

2. **Limitations are pre-existing**: Trade-off acknowledgments reference original discussion/setup sections.

3. **Citations are original**: No new references introduced; existing citations made specific.

4. **Confidence is calibrated**: Strong claims preserved when evidence supported; inflated claims reduced to evidence level.

5. **Minimal intervention**: Pattern removal preferred over elaborate replacement.

6. **Context-aware**: In rebuttal/revision letter context, diff-anchored writing is preserved, not flagged as AI-like.