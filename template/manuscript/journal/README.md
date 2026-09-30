# journal/

**No target journal defined yet.** The manuscript is journal-agnostic: `../main.tex` uses the
generic `article` class with `natbib` author-year citations and `plainnat`.

When the journal is chosen, run `/set-journal <journal name> <guidelines PDF or URL>`. It will:

1. distil the guidelines into `author_guidelines.md` (scope fit, length, file format, section
   order, abstract/keywords/highlights, units/figures/tables, citation format, declarations,
   submission checklist);
2. install the publisher's LaTeX class and templates under `journal/<class>/`;
3. port `../main.tex` onto that class (as a reviewable diff, TODOs preserved);
4. update `../CLAUDE.md` §1 and §7 and flag any conflict with the §5 style rules.
