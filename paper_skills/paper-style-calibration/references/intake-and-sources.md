# Intake and Sources

## Input Types

The skill accepts four types of style inputs:

| Type | Parameter | Description |
|------|-----------|-------------|
| **Source file** | `source=path` | A file (PDF/TeX/Markdown/Word) to extract style from |
| **Source kind** | `source_kind=own|reference` | Classification of the source (user's own vs. external reference) |
| **Style description** | `description="..."` | Text-based style preferences |
| **Style guide** | `guide=path` | Pre-existing style guide file |

## File Format Handling

### TeX Files (.tex)

**Preferred format**. Direct text extraction with full structure preservation.

1. Read the `.tex` file directly.
2. Preserve all LaTeX commands, environments, and math notation.
3. Parse section structure via `\section`, `\subsection`, `\paragraph` commands.
4. Identify citation patterns via `\cite{}`, `\ref{}`, `\label{}`.
5. Extract prose content (excluding math environments, verbatim, and code blocks).

**Output**: Clean prose paragraphs with LaTeX structure metadata.

### Markdown Files (.md)

**Clean extraction**. Simple text parsing.

1. Read the `.md` file directly.
2. Parse headers (`#`, `##`, `###`) to identify section structure.
3. Extract prose paragraphs (excluding code blocks and tables).
4. Identify link patterns as citation equivalents.

**Output**: Plain prose with section metadata.

### Word Files (.docx/.doc)

**Requires conversion**. Use platform-appropriate extraction.

**On Windows**:
```bash
# If pandoc is available
pandoc input.docx -t markdown -o temp_extracted.md
```

**Fallback** (if pandoc unavailable):
- Ask the user to convert to TeX or Markdown before proceeding.
- Or accept PDF version if available.

**Output**: Converted Markdown for analysis.

### PDF Files (.pdf)

**Last resort**. Text extraction with potential loss.

**Extraction methods**:

1. **Primary method** (if PyMuPDF available):
```bash
python -c "
import fitz
doc = fitz.open('input.pdf')
for page in doc:
    print(page.get_text())
"
```

2. **Fallback method** (if pdftotext available):
```bash
pdftotext input.pdf temp_extracted.txt
```

3. **Final fallback** (if no tools available):
- Ask the user to provide TeX/Markdown/Word version.
- If unavailable, attempt to read embedded text via Read tool (may produce fragmented output).

**PDF extraction limitations**:
- Math formulas may be fragmented or lost
- Table structure may be flattened
- Figure captions may be separated from figures
- Headers/footers may mix with content
- LaTeX commands are not recoverable

**Output**: Plain text with section inference (best-effort).

## Format Selection Logic

When multiple formats are provided:

```
IF .tex available → use TeX
ELSE IF .md available → use Markdown
ELSE IF .docx/.doc available → convert to Markdown
ELSE IF .pdf available → extract from PDF (warn user about limitations)
ELSE → ask user for alternative format
```

## Style Description Parsing

When the user provides a `description="..."` instead of a source file:

Parse the description into a structured style schema:

```yaml
voice: [third-person | first-person | passive-preferred | active-preferred]
sentence_length: [short (<15 words) | medium (15-25) | long (>25)] | target_median: [N]
hedging: [low | medium | high] | specific_rules: [list]
transition_style: [explicit | implicit | sparse | dense]
paragraph_pattern: [topic_first | evidence_first | mixed]
forbidden: [list of words/styles to avoid]
preferred: [list of words/styles to prefer]
```

**Hard vs. Soft constraints**:

| Constraint Type | Examples | Enforcement |
|-----------------|----------|-------------|
| **Hard** | "No first-person", "Passive voice only" | Must follow; skip if conflicts with facts |
| **Soft** | "Prefer shorter sentences", "Light hedging" | Apply when possible; preserve if content demands |

**Ambiguity handling**:
- If description is vague ("more academic"), use defaults: third-person passive, medium sentences, explicit transitions.
- If description conflicts with academic conventions, warn user and prioritize conventions.

## Source Kind Classification

### `source_kind=own` (User's Previous Paper)

- Full style profile extraction allowed.
- Can extract sentence templates (after de-identification).
- Can use paragraph structure patterns.
- Still cannot migrate: claims, results, citations, methods, arguments.

### `source_kind=reference` (External Reference Paper)

- **Restricted extraction**: only abstract patterns.
- Do NOT extract specific phrasings or sentence-level templates.
- Do NOT retain content-bearing rhetorical patterns.
- Extract only: voice preference, sentence length distribution, transition density, paragraph structure type.

**Why restriction matters**:
Reference papers carry risk of:
- Unintentional content copying
- Approximate paraphrasing (plagiarism risk)
- Terminology migration without understanding
- Argument structure adoption

## Target Manuscript Specification

The `target=path` parameter specifies the manuscript to be calibrated.

**Requirements**:
- Must be a TeX or Markdown file (editable format).
- Must contain actual content (not just template/outline).
- Should ideally have passed formal polishing first.

**Scope specification** (optional):
- `scope=introduction,related_work,results`: Only calibrate specified sections.
- If no scope specified, calibrate all prose sections (excluding Method, Theory, Abstract by default).