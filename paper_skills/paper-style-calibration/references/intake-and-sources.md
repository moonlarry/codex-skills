# Intake and Sources

## Input Handling

### Single Source Mode
- `source`: Reference paper/style source (single file)
- `target`: Target paper to calibrate

### Multi-Source Mode
- `sources`: Comma-separated list of reference papers (2-5 papers)
- `target`: Target paper to calibrate

**Mutually exclusive**: Use `source=` OR `sources=`, not both.

**Source limits**:
- Minimum: 2 papers (for aggregation)
- Maximum: 5 papers (for quality control)
- Recommended: 3-5 papers

## Format Auto-Detection

System automatically detects format from file extension and content:

| Extension | Format | Handling |
|-----------|--------|----------|
| `.tex` | TeX | Direct read, preserve LaTeX commands |
| `.md` | Markdown | Direct read, parse headers |
| `.docx`, `.doc` | Word | Convert via pandoc if available |
| `.pdf` | PDF | Extract text (may lose formatting) |

**If format cannot be determined**, system asks user for clarification.

**No `source_kind` parameter** - format inferred automatically.

## File Format Details

### TeX Files (.tex)

Direct text extraction:
- Preserve all LaTeX commands, environments, math notation
- Parse section structure via `\section`, `\subsection`
- Identify citations via `\cite{}`, `\ref{}`, `\label{}`
- Extract prose content (exclude verbatim, code blocks)

### Markdown Files (.md)

Clean text parsing:
- Parse headers (`#`, `##`, `###`) for section structure
- Extract prose paragraphs (exclude code blocks)
- Identify links as citation equivalents

### Word Files (.docx/.doc)

Conversion-based extraction:

**Primary** (if pandoc available):
```bash
pandoc input.docx -t markdown -o temp_extracted.md
```

**Fallback** (if pandoc unavailable):
- Ask user to convert to TeX or Markdown
- Or use PDF version if available

### PDF Files (.pdf)

Last-resort extraction (formatting may be lost):

**Primary** (if PyMuPDF available):
```python
import fitz
doc = fitz.open('input.pdf')
for page in doc:
    print(page.get_text())
```

**Secondary** (if pdftotext available):
```bash
pdftotext input.pdf temp_extracted.txt
```

**Final fallback**:
- Ask user for TeX/Markdown/Word version
- Or attempt Read tool (may produce fragmented output)

**PDF limitations**:
- Math formulas may be fragmented
- Table structure flattened
- Figure captions separated
- Headers/footers mixed with content
- LaTeX commands not recoverable

## Target File Requirements

**Must be editable format**:
- TeX (`.tex`) - recommended
- Markdown (`.md`) - acceptable

**Should contain actual content**, not just template/outline.

**Ideally pre-polished** with `paper-refine-special-en` before calibration.

## Scope Specification

Default calibration scope (prose-heavy sections):
- Introduction
- Related Work / Related Works
- Results / Experimental Results
- Discussion (interpretive prose only)

**Excluded** (technical precision required):
- Abstract
- Method
- Theory / Preliminaries
- Experiments Setup
- Conclusion
- Limitations

User can override: specify sections in natural language.

## Multi-Source Discovery

When `sources=` provided:

### Discovery Process

1. **Parse paths**: Split by comma
2. **Identify papers**:
   - For files: Check if root document (contains `\documentclass`)
   - For directories: Scan recursively, find all root documents
3. **Deduplication**:
   - Canonical path deduplication
   - Content hash deduplication
4. **Validation**: Ensure 2-5 unique papers

### Root Document Detection

A valid paper root must contain:
```latex
\documentclass{...}
\begin{document}
```

Non-root files (via `\input`, `\include`) are traced to their root.

### Directory Scanning

- Recursive scan for `.tex` files
- Ignore: `.git/`, build directories, output directories
- Do not follow symbolic links
- Sort by canonical path for reproducibility

## Output Path Convention

All outputs go to current working directory:

```
./paper-style/
└── runs/
    └── <timestamp>-<target_slug>/
```

**No `psmfiles/` output** - completely replaced by new structure.