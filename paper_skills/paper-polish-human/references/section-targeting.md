# Section Targeting

## Eligible Sections

Apply human-style variation only to these sections unless the user explicitly asks otherwise:

- Introduction
- Related Work
- Related Works
- Literature Review, when it functions as Related Work
- Results
- Experimental Results
- Results and Discussion, only for the results-facing prose

## Excluded Sections

Do not add conversational phrasing or intentional grammar imperfections to:

- Title
- Abstract
- Keywords
- Method, Methodology, Approach, Model, Framework, or Algorithm
- Theory, Preliminaries, Problem Formulation, or Proofs
- Experimental Setup, Datasets, Metrics, or Implementation Details
- Ablation Study, unless the paragraph is clearly results interpretation rather than setup
- Limitations
- Conclusion
- Acknowledgments
- References or Bibliography
- Tables, figure captions, equations, algorithms, theorem environments, and code blocks

## Boundary Handling

For LaTeX, use section commands such as `\section`, `\subsection`, and `\paragraph` to detect boundaries. Preserve the commands exactly.

For plain prose, use visible headings. If headings are missing or ambiguous, state the assumption and only edit paragraphs that clearly belong to Introduction, Related Work, or Results.

When a section combines setup and results, edit only the interpretive result sentences and leave setup details formal.
