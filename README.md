# Local LaTeX Workspace

This folder is your local Overleaf-style base.

## Open in VS Code

- Open `LATEX.code-workspace`.
- Keep projects in `projects/`.
- Keep reusable styles in `shared/styles/`.
- Keep reusable bibliographies in `shared/bib/`.

## Build behavior

- LaTeX Workshop auto-builds on save.
- Output is generated in the project folder for maximum MiKTeX compatibility.
- PDF opens in a VS Code tab.

## Recommended project layout

- `main.tex`
- `sections/`
- `figures/`
- `refs.bib`

## Notes

- `latexmk` is currently unavailable because Perl is not installed.
- This setup is intentionally single-compiler: `pdflatex` only.



## Compiler Policy

This setup is intentionally pdflatex-first.
Other TeX engines are not configured as primary compilers.


# LaTeX
