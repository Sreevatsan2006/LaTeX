# CONTEXT.md — LLM Guide for This LaTeX Style Suite

This file is written for LLMs (Claude, GPT, etc.) assisting with documents that use this style suite. Read this before generating or editing any `.tex` .

---

## What This Is

Three custom `.sty` files for mathematical writing, each a one-shot preamble replacement:

| File | Use case | Color system |
|---|---|---|
| `Math.sty` | Notes, lecture notes, general math documents | `none` / `minimal` / `balanced` / `rich` |
| `Paper.sty` | Research papers, formal writeups | `none` / `minimal` / `balanced` / `rich` |
| `Assignment.sty` | Homework, problem sets | `none` / `color` (binary) |

Each file loads its own full dependency stack (amsmath, amsthm, hyperref, cleveref, mdframed, tikz, biblatex, etc.). **Do not add `\usepackage{amsmath}` or similar — they are already loaded.**

---

## Quick Start

```latex
% Notes / general math
\documentclass[11pt]{article}
\usepackage[palatino]{Math}
\MathColorMode{balanced}  % must be before \begin{document}
\begin{document}
...
\end{document}

% Research paper
\documentclass[11pt]{article}
\usepackage[palatino]{Paper}
\PaperColorMode{none}     % must be before \begin{document}
\begin{document}
...
\end{document}

% Homework
\documentclass[11pt]{article}
\usepackage[palatino]{Assignment}
\renewcommand{\YourName}{Your Name}
\renewcommand{\CourseName}{MATH 101}
\renewcommand{\AssignmentNumber}{1}
\renewcommand{\AssignmentTitle}{Optional Title}
\AssignmentColorMode{color} % must be before \begin{document}
\begin{document}
\MakeTitleBlock
...
\end{document}
```

**Important:** All `\[Package]ColorMode{...}` calls must appear in the preamble, before `\begin{document}`.

---

## Package Options

Pass as `\usepackage[option]{PackageName}`.

### Font (all three files)
| Option | Font |
|---|---|
| `palatino` | Palatino + Pazo math **(default)** |
| `classic` | Computer Modern |
| `charter` | Charter |

### Paper size (all three files)
| Option | Behavior |
|---|---|
| *(none)* | US Letter, 1-inch margins **(default)** |
| `a4` | A4 paper, text block locked to US Letter width (identical pagination) |
| `a4native` | A4 paper, true 1-inch margins (text reflows) |

### Color mode as load option (Math / Paper only)
Can be passed at load time instead of calling `\MathColorMode{...}` / `\PaperColorMode{...}`:
```latex
\usepackage[none]{Math}      % same as \MathColorMode{none}
\usepackage[balanced]{Math}  % same as \MathColorMode{balanced}
```

### Behavior flags (Assignment only)
These are unique to `Assignment.sty`. `Math.sty` and `Paper.sty` do not have equivalents — their subsystems are always loaded.

| Option | Effect | Math/Paper equivalent |
|---|---|---|
| `notikz` | Skip TikZ loading (faster compile) | Also available in Math/Paper |
| `nomacros` | Disable the entire math macro library | None — use `\renewcommand` to override individual macros |
| `nolayout` | Disable spacing/header/section overrides | None — manually override `\parindent`, `\parskip`, `\linespread` etc. after loading |
| `notheoremui` | Disable all theorem environments and framed boxes | Closest: `\MathColorMode{none}` strips frames but environments still exist |

---

## Color Modes

### Math: `\MathColorMode{mode}` / Paper: `\PaperColorMode{mode}`
Controls theorem frame intensity, heading accents, and hyperlink coloring.

| Mode | Effect |
|---|---|
| `none` | Plain amsthm blocks, black links |
| `minimal` | Very light accents |
| `balanced` | Moderate structure cues **(Math default)** |
| `rich` | Strongest visual accents |

**Defaults:** `balanced` for Math, `none` for Paper.

### Assignment: `\AssignmentColorMode{mode}`
Binary only.

| Mode | Effect |
|---|---|
| `none` | Plain, black links **(default)** |
| `color` | Colored frames and links (aliases: `true`, `on`, `1`) |

---

## Environments

### Theorem-like (Math, Paper, Assignment — shared)

All follow standard amsthm syntax: `\begin{env}[Optional note]`.
Unnumbered variants available with `*` suffix (e.g., `\begin{theorem*}`).
Always use `\label{type:name}` and `\cref{}` for cross-references.

| Family | Environments | Frame color |
|---|---|---|
| Theorems | `theorem`, `lemma`, `proposition`, `corollary` | Blue |
| Support | `fact`, `claim`, `property`, `conjecture`, `question`, `statement`, `obs` | Violet/Navy |
| Definitions | `definition`, `example`, `remark` | Orange / plain |

### Assignment-specific

```latex
\begin{problem}[2][Optional title]   % number is optional, overrides counter
...
\end{problem}

\begin{chosenproblem}{3}[Optional title]
...
\end{chosenproblem}

\begin{solution}
...
\end{solution}

\begin{parts}         % multi-level enumerate: (a), (i), (1)
  \item ...
  \item ...
\end{parts}
```

### Callout boxes (all three files)

```latex
\begin{infobox}[Optional title]     % blue
\begin{successbox}[Optional title]  % green
\begin{warningbox}[Optional title]  % amber
\begin{dangerbox}[Optional title]   % red
\begin{ideabox}[Optional title]     % violet
\begin{examplebox}[Optional title]  % brown/plain
\begin{hintbox}[Optional title]     % cyan

% Custom color:
\begin{ColorNoteBox}{Cobalt}[Optional title]   % full name
\begin{ColorBox}{Cobalt}[Optional title]       % short alias

% Always-visible frame (even in none mode):
\begin{Frame}[Optional title]
```

### Pseudocode / algorithms

```latex
\begin{algo}[Optional title]          % or \begin{pseudocode}[...] — identical alias
\begin{algorithmic}
  \Require ...
  \Ensure ...
  \State ...
  \While{condition} ... \EndWhile
\end{algorithmic}
\end{algo}
```

### Inline formatting

| Macro | Effect |
|---|---|
| `\key{text}` | Bold cobalt — key vocabulary |
| `\warn{text}` | Bold red — pitfalls |
| `\result{math}` | Gold-highlighted background — key takeaway |

---

## Math Macros

**These are all custom-defined.** Do not replace them with standard LaTeX equivalents — they exist precisely to be shorter and more consistent.

### Number systems and standard sets

These override or extend standard LaTeX commands. Do not use `\mathbb{...}` manually for these.

| Macro | Output | Notes |
|---|---|---|
| `\N`, `\Z`, `\Q`, `\R`, `\C` | $\mathbb{N}, \mathbb{Z}, \mathbb{Q}, \mathbb{R}, \mathbb{C}$ | Standard number fields |
| `\F`, `\K` | $\mathbb{F}, \mathbb{K}$ | Generic scalar fields |
| `\P` | $\mathbb{P}$ | Probability measure (blackboard bold). **Overrides LaTeX's paragraph sign** — use `\pilcrow` if you need ¶ |
| `\E` | $\mathbb{E}$ | Expectation operator (blackboard bold) |
| `\one` | $\mathbbm{1}$ | Indicator function / constant-one |
| `\zero` | $\mathbf{0}$ | Zero vector / zero element |
| `\id` | $\mathrm{id}$ | Identity map |
| `\Rn`, `\Rd`, `\Rm` | $\mathbb{R}^n, \mathbb{R}^d, \mathbb{R}^m$ | Common real spaces |
| `\Rp` | $\mathbb{R}_+$ | Strictly positive reals |
| `\Rnneg` | $\mathbb{R}_{\geq 0}$ | Nonnegative reals |
| `\Rge{a}` | $\mathbb{R}_{\geq a}$ | Reals $\geq a$ |
| `\Zp` | $\mathbb{Z}_{>0}$ | Positive integers |
| `\Znneg` | $\mathbb{Z}_{\geq 0}$ | Nonneg integers |

**Note on probability notation:** `\P` is the blackboard-bold measure-theoretic $\mathbb{P}$. For ordinary probability in a less measure-theoretic context, `\Pr` (LaTeX native) is also available and some writers prefer it. Use whichever fits the document's register; both are valid.

### Bold/vector convention

**If it is bold, it is not a scalar.** This is enforced consistently throughout.

- `x`, `A` — scalars (plain)
- `\vx`, `\vA` — bold vector / matrix (`\mathbf`)
- `x_i` — the $i$-th *scalar entry* of vector $\mathbf{x}$ (scalar, so not bold)
- `\vx_i` — the $i$-th *vector* in a sequence of vectors (bold, not an entry)
- `(\vx_i)_j` — the $j$-th entry of the $i$-th vector in a sequence (scalar)
- `\balpha`, `\btheta`, `\bmu` — bold Greek (`\bm{...}`)

This convention avoids the mess of superscript/subscript overloading. Follow it strictly.

### Symbol prefixes
| Prefix | Meaning | Example |
|---|---|---|
| `\c` | Calligraphic | `\cH` → $\mathcal{H}$ |
| `\s` | Script | `\sF` → $\mathscr{F}$ |
| `\v` (lowercase) | Bold vector | `\vx` → $\mathbf{x}$ |
| `\v` (uppercase) | Bold matrix | `\vA` → $\mathbf{A}$ |
| `\b` | Bold Greek | `\balpha` → $\boldsymbol{\alpha}$ |

All 26 letters are defined for each prefix.

### Delimiters
All auto-scale with `*` variant (e.g., `\abs*{\frac{a}{b}}`).

| Macro | Output | Description |
|---|---|---|
| `\abs{x}` | $|x|$ | Absolute value |
| `\norm{x}` | $\lVert x \rVert$ | Norm |
| `\paren{x}` | $(x)$ | Parentheses |
| `\bracket{x}` | $[x]$ | Square brackets |
| `\set{x}` | $\{x\}$ | Set braces |
| `\ceil{x}` | $\lceil x \rceil$ | Ceiling |
| `\floor{x}` | $\lfloor x \rfloor$ | Floor |
| `\ang{x}` | $\langle x \rangle$ | Angle brackets |
| `\ip{u}{v}` | $\langle u, v \rangle$ | Inner product |
| `\of{x}` | $(x)$ | Tight parens for function arguments |
| `\setbuild{x \in \R}{\abs{x} < \eps}` | $\{x \in \R \mid |x| < \varepsilon\}$ | Set-builder notation |
| `\restr{f}{A}` | $f\!\upharpoonright_A$ | Restriction of $f$ to $A$ |
| `\eval{expr}` | $\left. \mathrm{expr} \right\|$ | Evaluation bar |
| `\closure{A}` | $\overline{A}$ | Topological closure |
| `\interior{A}` | $A^\circ$ | Interior |
| `\boundary{A}` | $\partial A$ | Boundary |
| `\compl{A}` | $A^{\mathsf{c}}$ | Complement |
| `\inv` | ${}^{-1}$ | Inverse (postfix) |

### Sums, products, integrals

| Macro | Output | Description |
|---|---|---|
| `\sumn` | $\sum_{n=1}^{\infty}$ | Infinite sum over $n$ |
| `\sumi` | $\sum_{i=1}^{\infty}$ | Infinite sum over $i$ |
| `\sumj` | $\sum_{j=1}^{\infty}$ | Infinite sum over $j$ |
| `\sumk` | $\sum_{k=1}^{\infty}$ | Infinite sum over $k$ |
| `\prodn` | $\prod_{n=1}^{\infty}$ | Infinite product over $n$ |
| `\prodi` | $\prod_{i=1}^{\infty}$ | Infinite product over $i$ |
| `\prodj` | $\prod_{j=1}^{\infty}$ | Infinite product over $j$ |
| `\sumsub{cond1}{cond2}` | $\sum_{\substack{\text{cond1}\\\text{cond2}}}$ | Sum with two stacked conditions |
| `\prodsub{cond1}{cond2}` | $\prod_{\substack{\text{cond1}\\\text{cond2}}}$ | Product with two stacked conditions |
| `\intR` | $\int_{\mathbb{R}}$ | Integral over $\mathbb{R}$ |
| `\intRd` | $\int_{\mathbb{R}^d}$ | Integral over $\mathbb{R}^d$ |
| `\intX` | $\int_{X}$ | Integral over $X$ |
| `\bigun` | $\bigcup$ | Big union |
| `\bigint` | $\bigcap$ | Big intersection |

### Differentials and derivatives

| Macro | Output | Description |
|---|---|---|
| `\dd` | $\,\mathrm{d}$ | Base differential |
| `\dx`, `\dy`, `\dt`, `\ds`, `\du` | $\,\mathrm{d}x$ etc. | Standard differentials |
| `\dmu`, `\dlambda` | $\,\mathrm{d}\mu$ etc. | Measure differentials |
| `\dP`, `\dQ` | $\,\mathrm{d}\mathbb{P}$ etc. | Probability measure differentials |
| `\deriv{f}{x}` | $\frac{\mathrm{d}f}{\mathrm{d}x}$ | Ordinary derivative |
| `\nderiv{f}{x}{n}` | $\frac{\mathrm{d}^n f}{\mathrm{d}x^n}$ | $n$-th derivative |
| `\pderiv{f}{x}` | $\frac{\partial f}{\partial x}$ | Partial derivative |
| `\npderiv{f}{x}{n}` | $\frac{\partial^n f}{\partial x^n}$ | $n$-th partial |
| `\mixpderiv{f}{x}{y}` | $\frac{\partial^2 f}{\partial x \partial y}$ | Mixed partial |
| `\grad` | $\nabla$ | Gradient |
| `\laplacian` | $\Delta$ | Laplacian |
| `\jac` | $\mathrm{J}$ | Jacobian symbol |
| `\hess` | $\mathrm{Hess}$ | Hessian symbol |
| `\del` | $\partial$ | Short partial alias |

### Limits and convergence

| Macro | Output | Description |
|---|---|---|
| `\limn` | $\lim_{n \to \infty}$ | Limit as $n \to \infty$ |
| `\limk` | $\lim_{k \to \infty}$ | Limit as $k \to \infty$ |
| `\limx` | $\lim_{x \to \infty}$ | Limit as $x \to \infty$ |
| `\ntoinf` | $n \to \infty$ | Subscript shorthand |
| `\ktoinf` | $k \to \infty$ | Subscript shorthand |
| `\epsdown` | $\varepsilon \downarrow 0$ | Epsilon down to zero |
| `\pto` | $\xrightarrow{\mathbb{P}}$ | Convergence in probability |
| `\asto` | $\xrightarrow{\text{a.s.}}$ | Almost sure convergence |
| `\dto` | $\Rightarrow$ | Convergence in distribution |
| `\lpto` | $\xrightarrow{L^p}$ | $L^p$ convergence |
| `\wto` | $\rightharpoonup$ | Weak convergence |
| `\wstarto` | $\overset{*}{\rightharpoonup}$ | Weak-* convergence |

### Linear algebra operators

| Macro | Output | Description |
|---|---|---|
| `\traceof{\vA}` | $\operatorname{tr}(\mathbf{A})$ | Trace of matrix |
| `\detof{\vA}` | $\operatorname{det}(\mathbf{A})$ | Determinant of matrix |
| `\rankof{\vA}` | $\operatorname{rank}(\mathbf{A})$ | Rank of matrix |
| `\nullityof{\vA}` | $\operatorname{nullity}(\mathbf{A})$ | Nullity of matrix |
| `\opnorm{\vA}` | $\lVert \mathbf{A} \rVert_{\mathrm{op}}$ | Operator norm |
| `\frobnorm{\vA}` | $\lVert \mathbf{A} \rVert_{\mathrm{F}}$ | Frobenius norm |
| `\spnorm{\vA}` | $\lVert \mathbf{A} \rVert_{2}$ | Spectral norm |
| `\condnum{\vA}` | $\kappa(\mathbf{A})$ | Condition number |
| `\Rayleigh{v}{A}` | $\frac{\langle v, Av \rangle}{\langle v, v \rangle}$ | Rayleigh quotient |
| `\trans` | ${}^{\mathsf{T}}$ | Transpose (postfix) |
| `\ctrans` | ${}^{*}$ | Conjugate transpose (postfix) |
| `\pinv` | ${}^{\dagger}$ | Pseudoinverse (postfix) |
| `\Ker` | $\operatorname{Ker}$ | Kernel |
| `\Null` | $\operatorname{Null}$ | Null space |
| `\Row` | $\operatorname{Row}$ | Row space |
| `\Col` | $\operatorname{Col}$ | Column space |
| `\im` | $\operatorname{im}$ | Image |
| `\rank`, `\nullity`, `\tr`, `\Det` | standalone operators | Use without parens; use `\rankof{}` etc. for function-style |
| `\eigvals` | $\operatorname{spec}$ | Spectrum / eigenvalues |
| `\diag` | $\operatorname{diag}$ | Diagonal |
| `\proj` | $\operatorname{proj}$ | Projection |

### Probability

| Macro | Output | Description |
|---|---|---|
| `\Prob{A}` | $\Pr(A)$ | Probability — operator style with auto-scaling parens |
| `\Probcond{A}{B}` | $\Pr(A \mid B)$ | Conditional probability |
| `\Ex{X}` | $\mathbb{E}[X]$ | Expectation |
| `\Excond{X}{Y}` | $\mathbb{E}[X \mid Y]$ | Conditional expectation |
| `\Varof{X}` | $\operatorname{Var}(X)$ | Variance |
| `\Covof{X}{Y}` | $\operatorname{Cov}(X, Y)$ | Covariance |
| `\Corrof{X}{Y}` | $\operatorname{Corr}(X, Y)$ | Correlation |
| `\indep` | $\perp\!\!\!\perp$ | Independence |
| `\given` | $\mid$ | Conditioning bar (scales in delimiters) |
| `\Normal` | $\mathcal{N}$ | Normal distribution |
| `\Unif`, `\Ber`, `\Bin`, `\Poi` | distribution names | Standard distributions |
| `\iid` | i.i.d. | Independent and identically distributed |
| `\Law` | $\mathcal{L}$ | Law / distribution of a r.v. |
| `\sgfield{X}` | $\sigma(X)$ | Sigma-algebra generated by $X$ |
| `\esssup`, `\essinf` | $\operatorname{ess\,sup}$, $\operatorname{ess\,inf}$ | Essential sup/inf |
| `\TV` | $\operatorname{TV}$ | Total variation |
| `\KL` | $\operatorname{KL}$ | KL divergence |

### Arrows and maps

| Macro | Output | Description |
|---|---|---|
| `\into` | $\hookrightarrow$ | Injective map |
| `\onto` | $\twoheadrightarrow$ | Surjective map |
| `\toiso` | $\xrightarrow{\sim}$ | Isomorphism arrow |
| `\isomto` | $\xrightarrow{\sim}$ | Alias for `\toiso` |
| `\xto{label}` | $\xrightarrow{\text{label}}$ | Labeled arrow |
| `\xfrom{label}` | $\xleftarrow{\text{label}}$ | Labeled reverse arrow |
| `\longto` | $\longrightarrow$ | Long arrow |
| `\embed` | $\hookrightarrow$ | Embedding |
| `\isom` | $\cong$ | Isomorphic |
| `\bij` | $\xrightarrow{\sim}$ | Bijection arrow |
| `\directsum` | $\oplus$ | Direct sum |
| `\tensor` | $\otimes$ | Tensor product |
| `\semidirect` | $\rtimes$ | Semidirect product |

### Logic and proof

| Macro | Output | Description |
|---|---|---|
| `\impl` | $\Longrightarrow$ | Implication |
| `\iff` | $\Longleftrightarrow$ | If and only if |
| `\impliesby{label}` | $\xRightarrow{\text{label}}$ | Labeled implication |
| `\iffby{label}` | $\xLeftrightarrow{\text{label}}$ | Labeled equivalence |
| `\fa` | $\forall$ | For all |
| `\ex` | $\exists$ | There exists |
| `\defeq` | $:=$ | Defined as |
| `\eqdef` | $\stackrel{\mathrm{def}}{=}$ | Equals by definition |
| `\byih` | $\overset{\mathrm{IH}}{\Longrightarrow}$ | By induction hypothesis |
| `\bycs` | $\overset{\mathrm{CS}}{\leq}$ | By Cauchy-Schwarz |
| `\byamgm` | $\overset{\mathrm{AM\text{-}GM}}{\geq}$ | By AM-GM |
| `\bydef` | $\overset{\mathrm{def}}{=}$ | By definition |
| `\contradiction` | $\Rightarrow\!\Leftarrow$ | Contradiction marker |
| `\qedbox` | $\square$ | Manual QED box |

### Abstract algebra

| Macro | Output | Description |
|---|---|---|
| `\ideal{a, b}` | $\langle a, b \rangle$ | Generated ideal |
| `\genby{g}` | $\langle g \rangle$ | Generated subgroup |
| `\normal` | $\trianglelefteq$ | Normal subgroup |
| `\quot{G}{N}` | $G/N$ | Quotient |
| `\Hom`, `\Aut`, `\Gal`, `\End` | operators | Standard algebra operators |
| `\Ext`, `\Tor` | operators | Homological algebra |
| `\Ann` | $\operatorname{Ann}$ | Annihilator |
| `\coker` | $\operatorname{coker}$ | Cokernel |
| `\ord` | $\operatorname{ord}$ | Order |
| `\Char` | $\operatorname{char}$ | Characteristic |
| `\Spec`, `\Frac` | operators | Commutative algebra |
| `\GL`, `\SL`, `\SO`, `\SU` | group names | Classical matrix groups |
| `\units{R}` | $R^\times$ | Unit group of $R$ |
| `\quot{G}{N}` | $G/N$ | Quotient |

### Optimization

| Macro | Description |
|---|---|
| `\argmin`, `\argmax` | Arg min / arg max operators |
| `\argminset{x \in X}` | $\operatorname*{arg\,min}\{x \in X\}$ |
| `\proxof{f}{x}` | Proximal operator $\operatorname{prox}_f(x)$ |
| `\convof{S}` | Convex hull of $S$ |
| `\epiof{f}` | Epigraph of $f$ |
| `\dom` | Domain of a function |
| `\supp` | Support |
| `\conv`, `\cone`, `\aff` | Standalone hull operators |
| `\LP`, `\ILP`, `\SDP`, `\KKT` | Program / condition names |
| `\OPT` | Optimal value |
| `\Lag` | $\mathcal{L}$ — Lagrangian |

### Asymptotics and complexity

| Macro | Output | Description |
|---|---|---|
| `\O` | $O$ | Big-O (**overrides LaTeX's Ø** — use `\Oslash` if you need Ø) |
| `\o` | $o$ | Little-o (**overrides LaTeX's ø** — use `\oslashletter` if needed) |
| `\Thetabig` | $\Theta$ | Big-Theta |
| `\Omegabig` | $\Omega$ | Big-Omega |
| `\softO` | $\widetilde{\mathcal{O}}$ | Soft-O |
| `\Pclass` | $\mathsf{P}$ | Complexity class P |
| `\NPclass` | $\mathsf{NP}$ | Complexity class NP |
| `\poly` | $\operatorname{poly}$ | Polynomial (operator) |

### Text in math

| Macro | Output |
|---|---|
| `\qqand` | $\quad\text{and}\quad$ |
| `\qqfor` | $\quad\text{for}\quad$ |
| `\qqif` | $\quad\text{if}\quad$ |
| `\qqwhere` | $\quad\text{where}\quad$ |
| `\qqwith` | $\quad\text{with}\quad$ |
| `\WLOG` | w.l.o.g. |
| `\iid` | i.i.d. |
| `\aev` | a.e. |
| `\as` | a.s. |
| `\wrt` | w.r.t. |
| `\rhs`, `\lhs` | RHS, LHS |

### Constants and shorthands

| Macro | Output | Description |
|---|---|---|
| `\e` | $\mathrm{e}$ | Euler's number |
| `\ii` | $\mathrm{i}$ | Imaginary unit |
| `\half` | $\tfrac{1}{2}$ | One-half |
| `\third` | $\tfrac{1}{3}$ | One-third |
| `\eps` | $\varepsilon$ | Epsilon |
| `\vphi` | $\varphi$ | Phi (variant) |

---

## Available Colors

The full named palette (use with `\textcolor{Name}{...}` or `\colorbox{Name}{...}`):

**Accent colors:** `Blue`, `LightBlue`, `Teal`, `Orange`, `Green`, `Red`, `Yellow`, `Purple`, `Pink`, `Brown`, `Cyan`

**Neutrals:** `Ink`, `Slate`, `Steel`, `Paper`

**Cool accents:** `Navy`, `Cobalt`, `Indigo`, `Violet`

**Green/teal accents:** `DeepTeal`, `Forest`, `Emerald`

**Warm accents:** `Amber`, `Gold`, `Coral`, `Rose`, `Burgundy`

**Semantic backgrounds:** `InfoBg`, `SuccessBg`, `WarningBg`, `DangerBg`, `HighlightBg`

---

## Display Math Usage

Use `\[ ... \]` for display math only when it genuinely needs to stand alone — multi-line equations, key results, anything that would be cramped inline. Do **not** display-math simple expressions that read naturally inline.

```latex
% Bad — needlessly breaks flow
\[x = y\]
We conclude that \[f(x) > 0\].

% Good
Since $x = y$, we conclude $f(x) > 0$.

% Good — display math earns its place
The main result is:
\[
  \int_{\R} f(x) \dx = \sumn a_n.
\]
```

The baseline rule: if it fits in a sentence without being cramped, inline it.

---

## Common Mistakes to Avoid

These are the most frequent LLM hallucinations when working with this codebase.

| ❌ Wrong | ✅ Correct | Why |
|---|---|---|
| `\mathbb{R}` | `\R` | Number system macro exists |
| `\mathbb{E}[X]` | `\Ex{X}` | Custom macro exists |
| `\mathbb{P}(A)` | `\Prob{A}` | Custom macro exists |
| `\operatorname{Var}(X)` | `\Varof{X}` | Custom macro exists |
| `\lvert x \rvert` | `\abs{x}` | Custom delimiter macro |
| `\lVert x \rVert` | `\norm{x}` | Custom delimiter macro |
| `\frac{df}{dx}` | `\deriv{f}{x}` | Custom macro exists |
| `\frac{\partial f}{\partial x}` | `\pderiv{f}{x}` | Custom macro exists |
| `\sum_{n=1}^{\infty}` | `\sumn` | Shorthand exists |
| `\prod_{n=1}^{\infty}` | `\prodn` | Shorthand exists |
| `\xrightarrow{\sim}` | `\toiso` or `\isomto` | Custom arrow exists |
| `\hookrightarrow` | `\into` or `\embed` | Custom arrow exists |
| `\twoheadrightarrow` | `\onto` | Custom arrow exists |
| `\operatorname{id}` | `\id` | Custom macro exists |
| `\coloneqq` | `\defeq` | Use the custom version |
| `\langle u, v \rangle` | `\ip{u}{v}` | Custom macro exists |
| `\mathbf{x}^{(i)}` for sequence | `\vx_i` | Convention: bold subscript = sequence index |
| `\usepackage{amsmath}` | *(nothing)* | Already loaded |
| `\usepackage{amsthm}` | *(nothing)* | Already loaded |
| `\R_{\geq 0}` | `\Rnneg` | Shorthand exists |
| `\mathbb{R}^n` | `\Rn` | Shorthand exists |
| `\[x = y\]` alone | `$x = y$` inline | Unnecessary display math |

**General rule:** If you find yourself writing out a long standard LaTeX expression for something common (a norm, a derivative, a limit, a sum, a number field), assume there is a macro for it and use the short form. The macro library is large by design. When in doubt, use the macro — the worst case is a compile error, which is easy to fix.