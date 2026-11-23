# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-document LaTeX CV (in Russian) built on the **FortySecondsCV** class (a rewrite of twentysecondcv). `CV_Cigankov.tex` is the only document; `cv_class.cls` is the class file (renamed copy of `fortysecondscv.cls`). `README.md` is the upstream FortySecondsCV documentation and is the reference for every class option and command (`\pointskill`, `\barskill`, `\cvitem`, `\wheelchart`, `\social`, etc.). Its mention of `template.tex` refers to the upstream repo; that file is not present here.

## Build

MiKTeX on Windows provides `xelatex`, `lualatex` and `latexmk`.

```sh
latexmk -xelatex CV_Cigankov.tex      # preferred: reruns automatically
xelatex CV_Cigankov.tex               # manual; run twice (hyperref PageLabels / .out need a rerun)
latexmk -c                            # clean aux files
```

- Compile with **XeLaTeX or LuaLaTeX**, not pdfLaTeX. The content is Cyrillic and is loaded through `fontspec`; under pdfLaTeX the class also falls back to the older `fontawesome` package, which is missing icons such as `\faTelegram`.
- The warning `requested document class 'cv_class', but the document class provides 'fortysecondscv'` is expected and harmless.
- `*.aux`, `*.log`, `*.out` and `*.synctex.gz` are build artifacts. `template.synctex.gz` is left over from upstream.

## Layout model (spans the .tex and the .cls)

- **Personal data** (`\cvname`, `\cvjobtitle`, `\cvprofilepic`, `\cvbirthday`, `\cvphone`, `\cvmail`, `\cvaddress`, …) is declared in the preamble. The order of declaration sets the order in the sidebar's personal info table.
- **Sidebar content** is collected in the preamble with `\addtofrontsidebar{...}` (and `\addtobacksidebar{...}` for later pages). Inside `document`, it is rendered by `\makefrontsidebar` / `\makebacksidebar`. The sidebar is an absolutely positioned `textblock*` over a TikZ background drawn at shipout, so it does not flow: overflowing sidebar content is clipped or overlaps silently instead of moving to the next page.
- **Body (right column)** is ordinary flowing text after `\makefrontsidebar`: `\cvsection` plus `cvtable` environments holding `\cvitem{dates}{title}{location}{description}`. The optional `cvtable` argument is `arraystretch` (use `[2]` for items with descriptions, as this file does).
- Layout and colors are adjusted with key-value class options, listed commented out at the top of `CV_Cigankov.tex` (`showframes` and `vline=` help with alignment). Change those options rather than editing `cv_class.cls`.
- Images: the profile picture comes from `pics/`, and flags come from `pics/flags/` via `\graphicspath` inside the sidebar block. Use `\flag{XX.png}` (ISO 3166-1 alpha-2 code) so the icon isn't tinted with `maincolor`.
