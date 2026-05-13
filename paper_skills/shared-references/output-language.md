# Output Language Protocol

## Language Detection

Determine the output language using this priority:
1. Follow the user's explicit language request when one is present.
2. Check project instructions such as `AGENTS.md` for a language preference.
3. If the user's most recent message is in Chinese, output user-facing analysis in Chinese.
4. Default to English for manuscript-ready academic prose unless the skill says otherwise.

## What to Localize

- Section headings and labels
- Descriptions, analysis, commentary, recommendations
- Template boilerplate text
- Status messages and warnings

## What NOT to Localize

- Code, shell commands, file paths, directory names
- Paper titles, author names, venue names, BibTeX entries
- Technical terms with no standard Chinese translation (keep English, optionally annotate: "attention mechanism (注意力机制)")
- LaTeX manuscript content for English-language venues, unless the user explicitly asks for Chinese LaTeX
- JSON state files — keys and structure remain English
- **Machine-parsed markers** — never localize the following, regardless of language setting:
  - Markdown frontmatter keys (e.g., `name:`, `description:`, `type:`)
  - `MANIFEST.md` column headers and table structure
  - Any field that downstream tools or scripts read programmatically

## Skill-Specific Rules

| Skill | Language Support | Notes |
|-------|-----------------|-------|
| /paper-plan | Full | Outline, section logic, and planning notes follow the user's language |
| /paper-writing | Partial | Manuscript LaTeX is English by default; diagnostics follow the user's language |
| /paper-write | Partial | Manuscript LaTeX is English by default; diagnostics follow the user's language |
| /experiment-plan | Full | Experiment roadmap follows the user's language |
| /experiment-result-to-claim | Full | Claim verdicts and rationale follow the user's language |
| /experiment-claim-audit | Full | Audit findings follow the user's language |
| /proof-* | Partial | Formal symbols and theorem statements stay stable; explanations follow the user's language |
| /rebuttal-* | Partial | Rebuttal letter language follows the target venue; planning notes follow the user's language |
| /paper-figure-* | Full | Prompts and captions follow the user's language unless the target figure text must be English |
