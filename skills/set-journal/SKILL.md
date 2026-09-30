---
name: set-journal
description: "Apply a target journal to a manuscript created by /manuscript-init: distil the journal's author guidelines into manuscript/journal/author_guidelines.md, install the publisher's LaTeX class, port main.tex onto it, and update manuscript/CLAUDE.md §1/§7. Use when the author says '/set-journal', 'we're submitting to <journal>', 'switch the manuscript to <journal>', or hands over a journal's guide for authors. Args: journal name plus a guidelines PDF, file or URL (either may be missing — ask for the other)."
---

# set-journal

Turns the journal-agnostic manuscript into one targeting a specific journal. Also used to
**switch** journals later — the procedure is the same, but the old `journal/` contents are
kept (move them to `journal/_previous/<old-journal>/`) rather than deleted.

Requires `manuscript/` at the repo root; if it doesn't exist, suggest `/manuscript-init`.

## Inputs

`args`: the journal name and its guidelines (a PDF/text file path or a URL to the "Guide for
authors" page). If only the name is given, WebSearch for the official guide for authors and
confirm the URL with the author before using it. Never distil guidelines from memory.

## Procedure

### 1. Distil the guidelines → `manuscript/journal/author_guidelines.md`

Read the whole source. Write a checklist (not a copy) with these headings, keeping every hard
limit as an exact number and quoting mandated wording (funding statements, AI-use
declarations) verbatim:

```
# <Journal> — Author Guidelines Checklist
Distilled from <source title/URL> (<publisher>, last updated <date on the source>), on <today>.

## Scope fit (check before writing)        — in/out of scope; how this project should be framed to fit
## Article type & length                   — word/page limits per article type
## File format                             — accepted source formats, columns, figure/table files
## Section order (title page → references) — numbered list, including every declaration section
## Abstract rules                          — length, structure, citations allowed?
## Keywords / highlights / graphical abstract
## Units, formulae, tables, figures
## Citation format                         — numbered vs author-year, et al. rules, DOI, reference-list style, BibTeX style file
## Ethics / declarations to prepare before submission
## Submission checklist (final pass)
```

Mark anything the source is silent on as "not specified" rather than filling it with a generic
convention. Keep the source PDF (if a file) in `journal/`.

### 2. Install the LaTeX class → `manuscript/journal/<class>/`

Use the class the guidelines name (publisher template page, or CTAN). Download the official
bundle — class/`.cls` or `.dtx`/`.ins`, `.bst` files, template `.tex`, README/manifest — with
`curl` into `journal/<class>/`. If a `.dtx`/`.ins` bundle needs generating, run
`latex <class>.ins` there. If no official LaTeX template exists (Word-only journal) or it can't
be downloaded, **stop and ask** — don't hand-roll a lookalike class.

First check whether the class is already in the TeX distribution (`kpsewhich <class>.cls`);
still copy the template `.tex` and `.bst` into `journal/<class>/` so the reference
implementation is in the repo. If it is **not** in the distribution, the build needs
`TEXINPUTS=journal/<class>//:` prefixed to each `pdflatex` call — add that to `CLAUDE.md` §8's
build line and to the `latex-workshop` tool `env` in the repo's `.vscode/settings.json`.

### 3. Port `manuscript/main.tex`

Rebuild the preamble and frontmatter on the journal's template `.tex` (the variant matching its
citation style), keeping:
- every existing `% TODO` and every filled-in value (title, authors, abstract, keywords);
- the `\input{sections/...}` lines (reorder only if the journal's section order requires it);
- the bibliography path `literature/refs`, with `\bibliographystyle{journal/<class>/<style>}`
  (or the installed style name).

Add/remove/reorder end-matter sections (CRediT, competing interests, funding, data
availability, AI-use declaration, acknowledgements) to match the guidelines' section order,
carrying the journal's mandated wording into the TODO comments. Add a header comment naming
the journal, the class, and the template file the port is based on.

Present the change as a reviewable diff of `main.tex` before moving on. Never delete a TODO or
filled-in content without asking.

If the citation style changes between numbered and author-year, grep `sections/*.tex` for
`\citet`/`\citep`/`\cite` and report usages whose meaning changes — don't rewrite prose.

### 4. Update `manuscript/CLAUDE.md`

- §1: replace the "Target journal: not yet defined" paragraph with the journal, publisher,
  class and citation style, plus one line on the scope framing from the checklist.
- §3 last bullet: point citation format at `journal/author_guidelines.md`.
- §7: replace the "No journal defined yet" paragraph with the pointer to
  `journal/author_guidelines.md` and `journal/<class>/` (name the template file used).
- §5: if the journal mandates American spelling, a different person/voice, or a structured
  abstract that conflicts with the style rules, **flag it to the author and ask which wins**
  — record the answer in §5, don't switch silently.

Also update `manuscript/journal/README.md` to say which journal is applied and when.

### 5. Build check

Run the build from `manuscript-init` step 7 (`pdflatex` → `bibtex` → `pdflatex` ×2 into
`_build/`). Fix any error introduced by the port. Report page count from the log if the
journal has a page limit.

### 6. Report

Journal applied, class/style installed, sections added/removed/reordered in `main.tex`, any
§5 conflicts awaiting the author's decision, citation-command changes flagged, and whether the
build passed. Don't commit.
