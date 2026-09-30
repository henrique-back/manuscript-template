---
name: manuscript-init
description: "Scaffold a journal-agnostic research manuscript (manuscript/ folder with CLAUDE.md writing rules, LaTeX skeleton, literature triage system, decision log) into the current repo from the shared template at ~/code/research/manuscript-template. Optionally triages a folder of literature PDFs and applies journal guidelines in the same run. Use when the author says '/manuscript-init', 'set up a manuscript', 'start a paper in this repo', or similar. Args (all optional, free-form): a literature folder path, a journal guidelines PDF/URL, a journal name."
---

# manuscript-init

Creates `<repo>/manuscript/` from the shared template and wires it to this repo. The template
is journal-agnostic; a journal is applied only if guidelines are given (step 6) or later via
`/set-journal`.

**Template location:** this skill's base directory is `<template>/skills/manuscript-init/`
(it is symlinked into `~/.claude/skills/`). Resolve the real path with
`realpath ~/.claude/skills/manuscript-init` and take `../../template` from it — call that
`$TPL`. It contains `manuscript/` (copied verbatim) and `vscode-settings.json`.

## Inputs

Parse `args` for, in any order:
- a directory of PDFs → **literature folder**;
- a `.pdf`/`.md`/`.txt` file or URL describing author guidelines → **guidelines**;
- anything else → **journal name**.

Missing inputs are fine — skip the corresponding step.

## Procedure

### 1. Locate the target

`REPO=$(git rev-parse --show-toplevel)`. If not in a git repo, ask for the target directory.
If `$REPO/manuscript` already exists, **stop and ask** — never merge into or overwrite an
existing manuscript.

### 2. Copy the skeleton

```
cp -R "$TPL/manuscript" "$REPO/manuscript"
```

### 3. Fill placeholders

`grep -rn '{{' "$REPO/manuscript"` lists them. Fill from the repo, asking only for what can't
be found:
- `{{PROJECT_NAME}}` — from the repo's `CLAUDE.md`/`README.md`/`pyproject.toml`.
- `{{PARENT_CLAUDE_MD}}` — `../CLAUDE.md` if the repo has one, else `../README.md`; if neither
  exists, say so in the text and flag it.
- `{{PROJECT_SUMMARY}}` — one paragraph (what is studied, data, method, targets) distilled from
  the repo docs. Don't invent specifics that aren't documented; leave a TODO for gaps.
- `{{RESULTS_SOURCES}}` — a bullet list of the repo's actual results/output locations (look
  for `results/`, `outputs/`, `data/processed`, notebooks writing metrics, summary logs). If
  none exist yet, write a single TODO bullet and flag it.
- `{{AUTHOR_AFFILIATION}}` — **ask the author** (use their git `user.name` only as a
  suggestion, never as fact).
- `{{UNCONFIRMED_DETAILS}}` — whatever of author order, postal address, target submission
  date the author didn't confirm.

Don't fill `CLAUDE.md` §0 — it stays UNSETTLED until the literature exists and the author
agrees the story.

### 4. VS Code LaTeX settings

Merge `$TPL/vscode-settings.json` into `$REPO/.vscode/settings.json` with a short Python/`jq`
merge: add missing keys only, never overwrite existing ones. Report any key that was skipped
because the repo already sets it differently.

### 5. Literature (if a folder was given)

1. Copy each PDF into `manuscript/literature/<bibkey>.pdf`
   (`<firstauthorfamily><year><shortword>`, lowercase; read the title page to get these).
   Don't move or delete the originals.
2. Triage in parallel: one Agent per batch of ~6 PDFs, each told to read `manuscript/CLAUDE.md`
   §3, `literature/notes.md`'s header and `literature/notes/TEMPLATE.md`, then write one
   `notes/<bibkey>.md` per paper and return its Index row plus verified BibTeX (or "unverified").
   Metadata must come from the PDF itself or a live search — never recall.
3. Run a separate verification pass (WebSearch/DOI lookup) for any entry whose DOI, volume or
   pages weren't on the PDF.
4. Collect results: Index rows into `notes.md` under a dated "Triage YYYY-MM-DD" note;
   verified entries into `refs.bib`; unverified ones under "Pending verification".
5. Draft "The gap this corpus does not fill" as a **proposal**, marked for the author to
   review — it is not settled until they confirm it.
6. Never write to `literature/personal_notes.md` — it is author-only.

### 6. Journal (if guidelines were given)

Follow the `/set-journal` skill's procedure (`$TPL/../skills/set-journal/SKILL.md`) with the
guidelines and journal name. Otherwise leave the manuscript journal-agnostic.

### 7. Build check

From `$REPO/manuscript`:
```
mkdir -p _build
pdflatex -interaction=nonstopmode -file-line-error -output-directory=_build main.tex
bibtex _build/main
pdflatex -interaction=nonstopmode -file-line-error -output-directory=_build main.tex
pdflatex -interaction=nonstopmode -file-line-error -output-directory=_build main.tex
```
`bibtex` exits non-zero on an empty `refs.bib` ("I found no \citation commands") — that is
expected before any citation exists. Any `pdflatex` error is not; fix it. `_build/` is
git-ignored by `manuscript/.gitignore`.

### 8. Report

In a few lines: files created, placeholders still open (`grep -rn '{{' manuscript`), literature
counts (triaged / in `refs.bib` / pending verification), whether a journal was applied, and
the next step (settle `CLAUDE.md` §0 with the author; `/set-journal` when the journal is
chosen). Don't commit — leave that to the author.
