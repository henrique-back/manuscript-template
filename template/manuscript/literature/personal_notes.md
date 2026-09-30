# Personal notes

Free-form, author-written notes about related work — ideas, half-formed connections, framings to
use in the manuscript, quotes or claims worth checking — kept separate from the per-paper triage
records in `notes.md` / `notes/*.md`, which are Claude-generated summaries produced by a formal
triage pass over PDFs already vetted into `refs.bib`. This file is the author's own working notes
and is not itself produced or verified by that triage process; some entries will have a solid
reference, some won't yet.

## How to use this file (for Claude)

- This file is **author-written**. Don't edit it unless the author explicitly asks (e.g. a
  `/find-reference` run filing an entry they requested).
- Every note here is authorial intent/knowledge — don't re-verify it the way you'd treat a web
  result, but *do* still check whether its reference actually resolves to a `refs.bib` entry
  before citing it in the manuscript.
- Each entry is tagged with one of:
  - `Reference: <bibkey>` — already in `literature/refs.bib`, safe to `\cite{}` directly.
  - `Reference: <freeform citation the author has — author/year/title/DOI, etc.>` — not yet in
    `refs.bib`. A `refs.bib` entry needs to be added (and metadata confirmed, per the sourcing
    rules in `../CLAUDE.md` §3) before this can be cited — flag it rather than inventing the
    BibTeX entry from the freeform text alone.
  - `Reference: none yet` — an idea, observation, or framing with no source attached. Fine to use
    as the author's own reasoning/argument in prose that doesn't require a citation; if the
    surrounding claim *does* need one, say so explicitly rather than pulling a plausible citation
    from memory or from an unrelated entry in `literature/notes/`.
- If an entry's freeform reference looks like it might already match something in
  `literature/notes/` or `literature/refs.bib`, say so and propose consolidating rather than
  creating a duplicate bib entry.

## Entry template

```
### <short topic>

Reference: <bibkey> | <freeform citation> | none yet

<the note>
```

## Entries
