# Decision log

This directory records *why* the method, data pipeline, and evaluation methodology ended up
the way they did — not just what the code currently does. Development is often
ablation-driven (implement a change → run it → compare against the current best → keep or
revert), so most entries are short records of one such step. Read this before writing any
methodological justification in the manuscript itself (Methods, Discussion, reviewer
response) — see `../CLAUDE.md` section 4 for that usage rule.

## Two tiers

**`log/`** — one short Markdown file per design/ablation decision, using
[`log/TEMPLATE.md`](log/TEMPLATE.md). This is the default tier. Every step that changed the
method, data, training/fitting procedure, or evaluation and produced a keep/revert call
belongs here, however small. Numbered by rough chronological order of the underlying
decision (not the order files were written), `NNN-slug.md`.

**Top-level `.tex` docs** — a full standalone LaTeX document (own title, sections,
bibliography), for a decision that needs citation-backed prose because it will be argued
in the manuscript itself, not just recorded for internal reference. Promote a `log/` entry
to one of these when you actually start drafting the corresponding Methods/Discussion
paragraph and it needs more than a paragraph of justification. List each here when created.

Most decisions will never need promotion — the `log/` entry is the final record for them.

## Index

| # | Decision | Status | Date |
|---|----------|--------|------|

## Adding an entry

1. Copy `log/TEMPLATE.md` to `log/NNN-slug.md`, next number in the index above.
2. Fill it in from primary sources only — a results file listed in `../CLAUDE.md` §2, or the
   commit that made the change. If a clean isolated before/after number doesn't exist
   (common — real ablations often bundle more than one change in the same commit/run), say
   so explicitly in the entry rather than presenting a confounded comparison as if it
   isolated one variable.
3. Add a row to the index table above.
4. If a future finding supersedes an entry, don't delete it — add a `Superseded by: NNN`
   line to its Status and leave the historical reasoning in place.
