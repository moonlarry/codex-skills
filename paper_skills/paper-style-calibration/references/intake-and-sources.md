# Intake and Sources

## Input Handling

This skill accepts two inputs:
- `source`: Reference paper/style source
- `target`: Target paper to calibrate

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

## Output Path Convention

All outputs go to current working directory:

```
./paper-style/
└── runs/
    └── <timestamp>-<target_slug>/
```

**No `psmfiles/` output** - completely replaced by new structure.