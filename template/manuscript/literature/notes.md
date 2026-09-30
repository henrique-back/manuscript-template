# Literature notes

Per-paper triage records for everything in `literature/`, so a future session (human or Claude)
never has to re-open a PDF just to remember what it says or whether it's worth citing. Each
paper added to this folder gets one record in `notes/<bibkey>.md` (citation, summary, relevance
tier, suggested manuscript use — copy `notes/TEMPLATE.md`) and a matching entry in
`literature/refs.bib` if its metadata was independently confirmed. See `../CLAUDE.md` §3/§4 for
how citations and the decision log are meant to be used together — this file plays the same
role for literature that `decisions/README.md` plays for design decisions.

Every fact recorded here must trace to either the paper's own PDF or a live search result, with
the date of the pass that found it — none from training-data recall (see `../CLAUDE.md` §3).
Record each triage or `/find-reference` pass as its own dated sub-heading below the Index.

## Index

| Key | Title (short) | Year | Relevance | In `refs.bib`? |
|---|---|---|---|---|

### Web-only comparators — no local PDF

| Key | Title (short) | Year | Relevance | In `refs.bib`? |
|---|---|---|---|---|

## The gap this corpus does not fill

<!-- What no paper found — locally or via websearch — does, stated narrowly enough to be
defensible. Record each narrowing as a dated sub-heading with the counterexample that forced
it, and list the gap-verification comparators found along the way. This section feeds
../CLAUDE.md §0.3/§0.5. -->

## Pending verification — do NOT cite yet

Found during a search but with incomplete metadata (missing full author list, volume, or DOI) —
do not add a `refs.bib` entry until confirmed by a follow-up search or a direct look at the
paper itself.

## Open issues — need a decision from the author

<!-- Attribution problems, suspected duplicates, papers that contradict a framing in
../CLAUDE.md §0. Mark each RESOLVED with a date rather than deleting it. -->
