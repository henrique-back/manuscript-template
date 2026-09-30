# manuscript-template

A reusable, journal-agnostic manuscript structure for research repos. It gives any repo a `manuscript/` folder containing:

- `CLAUDE.md`: the writing rules. These are the narrative-spine scaffold, source-of-truth and
  citation rules, the decision-log usage, the author's calibrated style/voice, "write the
  science, not the repository", the working process and the always-flag list.
- `main.tex` + `sections/01–04`: a generic `article`/`natbib` skeleton with the usual
  end-matter TODOs.
- `literature/`: `refs.bib`, a `notes.md` index, `notes/<bibkey>.md` per-paper triage
  records and the author-only `personal_notes.md`.
- `decisions/`: a two-tier decision log (`log/NNN-slug.md` plus standalone `.tex` docs).
- `journal/`: empty until a journal is chosen.

## Layout

```
template/manuscript/     copied verbatim into <repo>/manuscript/ ({{PLACEHOLDERS}} filled on init)
template/vscode-settings.json   LaTeX Workshop recipe (pdflatex → bibtex → pdflatex ×2, into _build/)
skills/manuscript-init/  /manuscript-init — scaffold, fill placeholders, optional literature + journal
skills/set-journal/      /set-journal     — distil guidelines, install class, port main.tex
skills/find-reference/   /find-reference  — find, verify and file one or two citations
```

## Install (once per machine)

```bash
ln -s ~/code/research/manuscript-template/skills/manuscript-init ~/.claude/skills/manuscript-init
ln -s ~/code/research/manuscript-template/skills/set-journal     ~/.claude/skills/set-journal
ln -s ~/code/research/manuscript-template/skills/find-reference  ~/.claude/skills/find-reference
```

Because these are symlinks, editing the files here updates the skills in every repo. If a repo
has its own `.claude/skills/find-reference` (e.g. `wave_forecast`), that project-level copy
takes precedence inside that repo.

## Usage

From inside the target repo, in Claude Code:

```
/manuscript-init                                        # skeleton only, journal-agnostic
/manuscript-init ~/papers/lit/                          # + triage a folder of PDFs
/manuscript-init ~/papers/lit/ ~/Downloads/guide.pdf    # + apply a journal's guidelines
/set-journal "Ocean Engineering" https://…/guide-for-authors   # later, once the journal is chosen
/find-reference <topic or claim you want a citation for>
```

`/manuscript-init` refuses to touch an existing `manuscript/` and never commits.

## Changing the template

Edit `template/manuscript/` directly. Anything true for every paper goes here: rules, voice,
folder conventions. Project-specific content goes in the repo's own `manuscript/CLAUDE.md` §0/§1.
The template must stay free of domain and journal specifics.
