---
name: handwriting-to-latex
description: Use this skill whenever the user uploads handwritten notes, problem sets, scanned documents, or iPad handwriting PDFs and wants them converted into LaTeX format. Trigger phrases include: convert to LaTeX, transcribe to tex, handwriting to LaTeX, LaTeX this, typeset my notes, put this in Overleaf format. Activate proactively whenever the user uploads an image or PDF containing mathematical expressions or handwritten academic content.
---

# Handwriting to LaTeX Skill

Recognise handwritten notes or problem sets (images or PDFs) and convert them into a complete, Overleaf-ready `.tex` file.

## Supported Input Types

| Format | Handling |
|---|---|
| JPG / PNG images | Direct visual recognition |
| Scanned PDFs | Page-by-page recognition |
| iPad handwriting PDFs | Same as above — pay attention to stylus stroke recognition |

If the user uploads multiple pages, process them in order and merge into a single `.tex` file.

## Output Structure

Generate a complete, self-contained `.tex` file with the following structure:

```latex
\documentclass[12pt, a4paper]{article}

% ─── Packages ────────────────────────────────────
\usepackage{amsmath, amssymb, amsthm}  % Mathematics
\usepackage{physics}                    % \dv, \pdv, \bra, \ket etc.
\usepackage{geometry}                   % Page margins
\usepackage{graphicx}                   % Images
\usepackage{booktabs}                   % Tables
\usepackage{array}                      % Enhanced matrices
\usepackage{hyperref}                   % Hyperlinks
\usepackage{setspace}                   % Line spacing
\usepackage{enumitem}                   % List formatting
\geometry{margin=2.5cm}
\setstretch{1.3}

% ─── Cover page (optional — remove if not needed) ─
\title{Assignment / Notes Title}
\author{Your Name \\ Your Institution}
\date{\today}
% ──────────────────────────────────────────────────

\begin{document}

\maketitle   % Remove this line if no cover page is needed

% Body content starts here
...

\end{document}
```

## Recognition and Conversion Rules

### Mathematical Expressions
- Inline expressions: `$...$`
- Multi-step derivations: `\begin{align*}...\end{align*}` (with `*` — **no line numbering**)
- Single standalone equations: `\[ ... \]` — do not use `\begin{equation}`
- **Never output numbered equation tags** — do not use `\tag{}`, `\label{}`, or `\ref{}` unless the user explicitly requests them
- Preserve every step of a derivation exactly as written — do not skip steps
- Common symbol reference:

| Handwritten | LaTeX |
|---|---|
| Vector arrow | `\vec{v}` or `\mathbf{v}` |
| Partial derivative | `\frac{\partial f}{\partial x}` or `\pdv{f}{x}` |
| Integral | `\int_{a}^{b} f(x)\,dx` |
| Summation | `\sum_{i=1}^{n}` |
| Infinity | `\infty` |
| Quantum state | `\ket{\psi}`, `\bra{\phi}` |
| Expectation value | `\langle \hat{A} \rangle` |
| Reduced Planck constant | `\hbar` |
| Hamiltonian | `\hat{H}` |

### Matrices and Tables
- Matrices: `\begin{pmatrix}...\end{pmatrix}` (round brackets) or `bmatrix` (square brackets)
- Augmented matrices: `\left[\begin{array}{cc|c}...\end{array}\right]`
- Data tables: `booktabs` style — use `\toprule`, `\midrule`, `\bottomrule`

### Text Paragraphs
- Write all paragraphs and explanations in English
- If handwriting contains non-English annotations, translate them to English naturally
- Preserve the paragraph structure as written
- Problem numbers: use `\section*{Problem 1}` or `\subsection*{(a)}`

### Cover Page (Optional)
Only add a cover page when the user **explicitly requests** one. When included, prompt the user for:
- Title (assignment name / course name)
- Author name
- Institution
- Date (defaults to `\today`)

## Workflow

### Step 1 — Receive Input
- Obtain the image or PDF from the conversation
- If multiple images are uploaded, confirm the page order with the user:
  > "Please confirm the page order: Page 1 → Page 2 → Page 3, or do you need to reorder?"

### Step 2 — Pre-conversion Confirmation
Before starting the conversion, show the user a brief summary and ask for confirmation:

```
I can see the following:
- Content: {brief description, e.g. "QM derivation, ~3 equations, 1 matrix"}
- Sections: {detected section or problem numbers}
- Cover page: Yes / No

Shall I proceed with the conversion?
```

Wait for user confirmation before generating the `.tex` file.

### Step 3 — Recognise Content
- Read every line of handwritten content carefully
- For ambiguous passages, add a LaTeX comment: `% [Unclear: transcribed as X, please verify]`
- Do not skip any step, even intermediate calculations

### Step 4 — Convert Block by Block
- Text → paragraphs
- Equations → `align*` environment
- Matrices → `pmatrix` / `bmatrix`
- Tables → `booktabs` tables

### Step 5 — Generate File
- Filename: `{course-code}-{content-description}.tex` (e.g. `PHYS101-QM-hw3.tex`)
- Save to `/mnt/user-data/outputs/`
- Also display the complete LaTeX source in the chat for easy copying

### Step 6 — Quality Check
- Confirm every `\begin` has a matching `\end`
- Confirm math environments are not nested (e.g. no `equation` inside `align`)
- If anything is uncertain, append a **"Please verify"** list at the end of the reply

## Important Notes

- **Never simplify derivations** — if the handwriting has five steps, output five steps, aligned at each equals sign
- **Physics packages included by default** — the `physics` package is always loaded, providing `\dv`, `\pdv`, `\ket`, `\bra`, and other commands commonly used in physics and physical chemistry
- **Compiler target** — optimised for Overleaf **pdfLaTeX** by default; no settings changes needed, just hit Compile
- **Language** — all body text and explanations should be in English; translate any non-English handwritten annotations naturally
- **Ambiguous handwriting** — always add a comment rather than silently guessing: `% [Unclear: transcribed as X, please verify]`
- **Cover page** — omitted by default unless the user explicitly says "add cover page"
