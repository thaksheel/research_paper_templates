# LaTeX Research Paper Template

A single-source LaTeX template for research papers. Write the content once, then switch between a two-column layout, a simple article, a NeurIPS submission, or a custom journal style by editing one line.

## Contents

| File | Purpose |
|------|---------|
| `main.tex` | **The only file you compile.** Configuration block, style selection, packages, macros |
| `title.tex` | Title and authors |
| `abstract.tex` | Abstract and keywords |
| `body.tex` | Introduction, background, methods, results, conclusion |
| `table.tex` | Results table template |
| `fig.tex` | Figure templates (uses `figures/image.png` as a placeholder) |
| `ref.bib` | Bibliography database |
| `styles/` | Style files: `style-twocolumn.sty`, `style-article.sty`, `style-neurips.sty`, `neurips_2026.sty`. Put journal `.cls`, `.sty` and `.bst` files here |
| `figures/` | Image files referenced by `\includegraphics` |
| `response_to_reviewers.tex` | Separate response letter for revisions |

## Requirements

- A TeX distribution: TeX Live (Linux, Windows via MiKTeX) or MacTeX (macOS)
- `latexmk` (included in TeX Live and MacTeX), or `pdflatex` and `bibtex` run manually
- Git, if you are cloning from GitHub

## Getting started

```bash
git clone https://github.com/<your-user>/<your-repo>.git
cd <your-repo>
latexmk -pdf main.tex
```

This produces `main.pdf`. `latexmk` runs LaTeX and BibTeX as many times as needed.

To clean auxiliary files: `latexmk -c`. To remove the PDF too: `latexmk -C`.

If you do not use `latexmk`:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

## Choosing a style

Open `main.tex` and edit the configuration block at the top:

```latex
\def\PaperStyle{twocolumn}   % twocolumn | article | neurips | custom
```

- **`twocolumn`**: compact two-column layout, Times font, 10 pt
- **`article`**: one column, Latin Modern font, 11 pt, with optional line spacing
- **`neurips`**: NeurIPS submission format. Set the track with `\NeurIPSOptions`, for example `main`, `main,final`, `preprint` or `dblblindworkshop`. For workshops, also set `\NeurIPSWorkshopTitle`
- **`custom`**: any journal template. Set `\CustomClass`, `\CustomClassOptions`, `\CustomStyle` and `\CustomBibStyle`. Commented examples for common journals are in `main.tex`

For a journal template, copy its files into `styles/` and set the class, options and bibliography style to match its instructions.

## Revision highlighting

When you revise a paper after review, mark changed text so reviewers can find it:

```latex
\rev{This sentence was changed after reviewer comments.}

\begin{revision}
A longer passage, equation or list that changed.
\end{revision}
```

Changes appear in blue by default. To change the colour, edit `\RevisionColor` in `main.tex`. To print the final version with black text, set:

```latex
\highlightrevisionsfalse
```

This keeps the markup in place, so you never need to delete it. You do not need to edit the body text to switch modes.

## Writing the paper

1. Enter the title and authors in `title.tex`.
2. Write the abstract in `abstract.tex`.
3. Write the body in `body.tex`. Use `\section`, `\subsection` and `\paragraph` as normal.
4. Add references to `ref.bib`, then cite them with `\citep{key}` (parenthetical) or `\citet{key}` (textual).
5. Put table and figure content in `table.tex` and `fig.tex`. Add your images to `figures/`.
6. Refer to labelled items with `\cref{...}`, which fills in the word and number, for example "Figure 2".

The math packages and macros are already loaded in `main.tex`:
`\R`, `\E`, `\Var`, `\Cov`, `\argmin`, `\argmax`, `\norm{x}`, `\inner{x}{y}`, `\abs{x}`, `\bm{x}`, `\T` (transpose). The `optidef` package provides `\begin{mini}...\end{mini}` for optimization problems.

## Response to reviewers

`response_to_reviewers.tex` is a separate document:

- `\editor` starts the editor's comments
- `\reviewer` starts each reviewer's section. Reviewers are numbered automatically
- `\reviewercomment{...}` contains the reviewer's text. Comments are numbered automatically, for example R1.1 and R2.3
- `\begin{response}...\end{response}` contains your reply, in blue by default
- `\begin{revisedtext}...\end{revisedtext}` quotes the new manuscript text

To cross-reference a comment, give it a label with `\label{R1.1}` and refer to it with `\ref{R1.1}`.

Compile it separately:

```bash
latexmk -pdf response_to_reviewers.tex
```

## Tips

- Keep `main.tex` as the only file you compile. Do not put content directly in `main.tex`.
