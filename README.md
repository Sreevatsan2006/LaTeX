# LaTeX

A suite of three drop-in `.sty` files for mathematical writing. Each one is a batteries-included preamble replacement — one `\usepackage` line and you get fonts, theorem environments, a large math macro library, colored callout boxes, bibliography setup, and more.
Font options: `palatino` (default), `classic` (Computer Modern), `charter`.

| File | Use case |
|---|---|
| `Math.sty` | Notes, lecture notes, general math documents |
| `Paper.sty` | Research papers, formal writeups |
| `Assignment.sty` | Homework, problem sets |

## AI Writing Assistant?
If you use an LLM to help write (and at this point, who doesn't) — there's a [`CONTEXT.md`](CONTEXT.md) file written specifically so your LLM doesn't have to reverse-engineer how everything works.Feed it at the start of your conversation as a reference. It covers the almost all useful macros, all environments, color modes, and common mistakes.Also doubles as a decent human-readable reference too.


## Examples

The `examples/` folder has compiled PDFs showing all three font styles (`palatino`, `classic`, `charter`) in both color and no-color modes, for both a notes-style and an assignment-style document. Worth a look so you can see exactly how your actual math writing will look before picking a style.

## Installation

You just need the `.sty` file in the same directory as your `.tex` file. How you get it there is up to you.

**Option 1 — Overleaf "From External URL"**

1. Copy the raw link for whichever file you want:
   ```
   https://raw.githubusercontent.com/Sreevatsan2006/LaTeX/main/templates/Math.sty
   https://raw.githubusercontent.com/Sreevatsan2006/LaTeX/main/templates/Paper.sty
   https://raw.githubusercontent.com/Sreevatsan2006/LaTeX/main/templates/Assignment.sty
   ```
2. In your Overleaf project, click the **New File** icon (top left).
3. Select **From External URL**.
4. Paste the raw link in the top box, type `Math.sty` (or whichever) in the bottom box, click **Create**.

Overleaf will pull it straight from GitHub. You can refresh it later if the file updates.

**Option 2 — Git clone or direct download**

```bash
git clone https://github.com/Sreevatsan2006/LaTeX.git
```

Or just download the individual `.sty` file from the repo and drop it next to your `.tex`.

**Option 3 — Caveman copy-paste**

1. Open the raw file link (any of the ones above).
2. `Ctrl+A`, `Ctrl+C`.
3. In Overleaf, create a new blank file named `Math.sty` (or whichever).
4. Paste. Done.

Do not underestimate the speed of this approach.

## Quick Start

```latex
% Notes / general math
\documentclass[11pt]{article}
\usepackage[palatino]{Math}
\MathColorMode{balanced}
\begin{document}
...
\end{document}
```

```latex
% Research paper
\documentclass[11pt]{article}
\usepackage[palatino]{Paper}
\PaperColorMode{none}
\begin{document}
...
\end{document}
```

```latex
% Homework
\documentclass[11pt]{article}
\usepackage[palatino]{Assignment}
\renewcommand{\YourName}{Your Name}
\renewcommand{\CourseName}{MATH 101}
\renewcommand{\AssignmentNumber}{1}
\AssignmentColorMode{color}
\begin{document}
\MakeTitleBlock
...
\end{document}
```
The `\[Package]ColorMode{...}` call must be in the preamble, before `\begin{document}`.

