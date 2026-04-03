# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this repository.

## Project Overview

`tikz-infographic` is a LaTeX/TikZ library for creating modern infographic elements. It can be loaded as a package (`\usepackage{tikz-infographic}`) or as a TikZ library (`\usetikzlibrary{infographic}`).

## Building and Testing

There is no build system. Compile examples manually with any LaTeX engine:

```sh
TEXINPUTS=../tex/latex/tikz-infographic: pdflatex annotation-01.tex
```

Run from the `examples/` directory.

## Code Architecture

All library source lives under `tex/latex/tikz-infographic/`.

**Entry points:**
- `tikz-infographic.sty` — loaded via `\usepackage{tikz-infographic}`; requires TikZ and loads the common infrastructure plus all elements
- `tikzlibraryinfographic.code.tex` — loaded via `\usetikzlibrary{infographic}`; thin wrapper that delegates to the `.sty`

**Module pattern** — each element is split into two files:
- `infographic-<element>-keys.code.tex` — PGFKeys definitions under `/infographic/<element>/`
- `infographic-<element>-pics.code.tex` — TikZ `\tikzset{pics/<element>/.style=...}` implementation

**Common infrastructure** (`infographic-common.code.tex`):
- Defines angle/corner lookup tables used by elements to compute connector directions from label anchor names
- Provides shared helper macros

**Currently implemented element:** `annotation` — a dot at the placement point, a text label offset by `label/dx` and `label/dy`, and a curved Bezier connector between them. Keys:

| Key | Default | Description |
|-----|---------|-------------|
| `color` | `gray` | Color for all three components |
| `dot/radius` | `2pt` | Dot size |
| `label/text` | (empty) | Label content |
| `label/dx` | `0pt` | Horizontal label offset |
| `label/dy` | `1em` | Vertical label offset |
| `label/anchor` | `south` | TikZ anchor on the label node |
| `connector/line width` | `1pt` | Connector stroke width |
| `connector/looseness` | `1` | Bezier curve looseness |

**Adding a new element:**
1. Create `infographic-<element>-keys.code.tex` with keys under `/infographic/<element>/`
2. Create `infographic-<element>-pics.code.tex` implementing the `pics/<element>` TikZ picture
3. Add two `\input{...}` lines to `tikz-infographic.sty`

## Branch Convention

- `main` — stable/release branch
- `dev` — active development branch; open PRs against `main`
