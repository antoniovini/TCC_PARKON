# Implementation Plan: TCC PARKON LaTeX Migration

## Overview

This plan migrates the content of TCC_PARKON_V3.pdf into a complete LaTeX project using the PPGC-UFF-Latex-Abnt template. Tasks follow the order: project scaffolding → main file configuration → pre-textual elements → chapter content → bibliography → acronyms → post-textual elements → cross-references → compilation verification. Each task produces LaTeX source files that build incrementally toward a compilable project.

## Tasks

- [x] 1. Set up project structure and copy template files
  - [x] 1.1 Create the TCC-PARKON project directory structure
    - Create root directory `TCC-PARKON/` with subdirectories: `pre-textuais/`, `capitulos/`, `capitulos/figuras/`, `pos-textuais/`, `biblatex-abnt/`
    - Copy `PPGC_UFF.cls` and `cvs-id.def` from the template to the project root
    - Copy all files from `PPGC-UFF-Latex-Abnt/biblatex-abnt/` to `TCC-PARKON/biblatex-abnt/`
    - Copy `brazil-abnt.lbx` and `brazilian-abnt.lbx` from template root to project root
    - _Requirements: 2.7_

  - [x] 1.2 Create and configure the main file (tese.tex)
    - Create `tese.tex` with `\documentclass[ruledheader, 12pt, openright, a4paper, oneside, english, brazil]{PPGC_UFF}`
    - Add all packages in the exact Template order: babel → fontenc → inputenc → epsf/subfigure → epsfig/graphicx → chngcntr → pifont/textcomp/latexsym → amssymb/amstext/amsthm/icomma/amsmath → algorithm/algorithmic → url → longtable → lscape → quoting → bigdelim/booktabs/colortbl/multirow/multicol/rotating → glossaries → hyperref → biblatex
    - Configure `\counterwithout{figure}{chapter}` and `\counterwithout{table}{chapter}`
    - Configure hyperref with bookmarksopen=true, linktoc=page, colorlinks=true, linkcolor=blue, citecolor=blue, filecolor=magenta, urlcolor=blue
    - Configure biblatex with style=abnt, language=brazil, backend=biber and `\addbibresource{bibliografia.bib}`
    - Set font commands: `\renewcommand{\ABNTchapterfont}{\bfseries\fontfamily{ptm}\fontseries{b}\selectfont}` and `\renewcommand{\ABNTsectionfont}{\bfseries\fontfamily{ptm}}`
    - Add `\makeglossaries` and `\input{pre-textuais/acronimos}` before `\begin{document}`
    - Add `\include` commands for all files in ABNT order: pre-textuais/cap0, chapters, \printbibliography, post-textual files
    - Set page numbering to arabic after pre-textual include with `\setcounter{page}{1}` and `\pagenumbering{arabic}`
    - _Requirements: 2.1, 2.5, 2.6, 8.1, 8.2, 8.3, 8.5_

- [x] 2. Checkpoint - Verify project scaffolding
  - Ensure the project structure is complete with all template support files copied and tese.tex properly configured. Ask the user if questions arise.

- [x] 3. Create pre-textual elements
  - [x] 3.1 Create pre-textuais/cap0.tex with cover and front matter
    - Extract author name, title, institution, advisor, co-advisor (if present), location, and year from TCC_PARKON_V3.pdf
    - Populate `\autor{}`, `\titulo{}`, `\instituicao{}`, `\orientador{}`, `\coorientador{}` (if applicable), `\local{}`, `\data{}`
    - Write `\comentario{}` with "Trabalho de Conclusão de Curso apresentado ao Curso de Sistemas de Informação da Universidade Federal Fluminense como requisito parcial para a obtenção do Grau de Bacharel em Sistemas de Informação." plus concentration area
    - Add `\capa` and `\folhaderosto` commands
    - Set page numbering: `\setcounter{page}{1}` and `\pagenumbering{roman}`
    - _Requirements: 3.1, 3.2, 3.3_

  - [x] 3.2 Create the approval page (termo de aprovação) in cap0.tex
    - Extract banca examinadora members from TCC_PARKON_V3.pdf (max 5 including orientador)
    - Write the approval page block with student name, title, approval date, and each banca member with name, title, and institution
    - Follow the Template structure with `\rule` separators and centered layout
    - _Requirements: 3.4_

  - [x] 3.3 Create dedication, acknowledgments, resumo, and abstract in cap0.tex
    - Extract dedication text and format as right-aligned italic block on dedicated page (if present in source)
    - Extract acknowledgments text and place in `\pretextualchapter{Agradecimentos}` environment
    - Extract resumo text and place in `\begin{resumo}` environment with `Palavras-chave` as bold-labeled list
    - Extract abstract text and place in `\begin{abstract}` environment with `Keywords` as bold-labeled list
    - _Requirements: 3.5, 3.6, 3.7, 3.8_

  - [x] 3.4 Add lists and table of contents to cap0.tex
    - Add `\listoffigures` for list of figures
    - Add `\listoftables` for list of tables
    - Add `\printglossary[type=\acronymtype,title={Lista de Abreviaturas e Siglas}]` for abbreviation list
    - Add `\tableofcontents` for table of contents
    - Include `\cleardoublepage` between each list element as per Template
    - _Requirements: 7.3, 9.3_

- [x] 4. Migrate chapter content
  - [x] 4.1 Create capitulos/cap1.tex - Introdução
    - Extract all text from Chapter 1 (Introdução) of TCC_PARKON_V3.pdf
    - Structure with `\chapter{Introdução}` and `\label{cap:introducao}`
    - Map all headings to `\section{}`, `\subsection{}`, `\subsubsection{}` based on visual hierarchy
    - Preserve all paragraphs separated by blank lines, encoded in UTF-8
    - Convert parenthetical citations to `\cite{}` and narrative citations to `\textcite{}`
    - Format block quotes (>3 lines) using `\begin{quoting}[rightmargin=0cm,leftmargin=4cm]` with `\begin{singlespace}` and `\footnotesize`
    - Insert TODO comments for any content that cannot be extracted
    - _Requirements: 1.1, 1.2, 1.5, 1.6, 2.3, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6_

  - [x] 4.2 Create capitulos/cap2.tex - Referencial Teórico / Trabalhos Relacionados
    - Extract all text from Chapter 2 of TCC_PARKON_V3.pdf
    - Structure with `\chapter{}` using the exact chapter title from source and `\label{cap:referencial}`
    - Apply same formatting rules as cap1: headings hierarchy, paragraphs, citations, block quotes
    - Use `\acrfull{}` for first acronym occurrences, `\acrshort{}` for subsequent
    - Insert TODO comments for any unresolvable content
    - _Requirements: 1.1, 1.2, 1.5, 1.6, 2.3, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 7.2_

  - [x] 4.3 Create capitulos/cap3.tex - Metodologia / Proposta
    - Extract all text from Chapter 3 of TCC_PARKON_V3.pdf
    - Structure with `\chapter{}` using the exact chapter title from source and `\label{cap:proposta}`
    - Apply same formatting rules: headings, paragraphs, citations, block quotes
    - Include any algorithms using `\begin{algorithm}` and `\begin{algorithmic}` environments
    - Convert cross-references to `\autoref{}` commands
    - _Requirements: 1.1, 1.2, 1.5, 1.6, 2.3, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 7.2_

  - [x] 4.4 Create capitulos/cap4.tex - Resultados
    - Extract all text from Chapter 4 of TCC_PARKON_V3.pdf
    - Structure with `\chapter{}` using the exact chapter title from source and `\label{cap:resultados}`
    - Apply same formatting rules: headings, paragraphs, citations, block quotes
    - Pay special attention to tables and figures in results chapter
    - Convert all cross-references to `\autoref{}` commands
    - _Requirements: 1.1, 1.2, 1.5, 1.6, 2.3, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 7.2_

  - [x] 4.5 Create capitulos/cap5.tex - Conclusão
    - Extract all text from Chapter 5 of TCC_PARKON_V3.pdf
    - Structure with `\chapter{}` using the exact chapter title from source and `\label{cap:conclusao}`
    - Apply same formatting rules: headings, paragraphs, citations, block quotes
    - _Requirements: 1.1, 1.2, 1.5, 1.6, 2.3, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6_

- [x] 5. Migrate figures and tables
  - [x] 5.1 Extract and place all figures
    - Extract each figure from TCC_PARKON_V3.pdf as PNG files into `capitulos/figuras/`
    - Name figures descriptively (e.g., `fig_arquitetura.png`, `fig_diagrama.png`)
    - For each figure in the chapter files, create `\begin{figure}[!ht]` environment with `\centering`, `\includegraphics[width=0.8\linewidth]{capitulos/figuras/filename}`, `\caption{original caption}`, and `\label{fig:identifier}`
    - For figures that cannot be extracted, insert placeholder with TODO comment and `\fbox` containing description
    - _Requirements: 1.3, 5.1, 5.2, 5.3, 5.5, 5.6_

  - [x] 5.2 Format all tables in chapter files
    - For each table in the source, create `\begin{table}[!ht]` with `\begin{center}`, `\caption{original caption}` above `\begin{tabular}`, and `\label{tab:identifier}`
    - Reproduce table structure using appropriate column specifiers
    - Preserve all cell content exactly as in source
    - _Requirements: 1.3, 5.4, 5.5_

- [x] 6. Checkpoint - Verify chapter content
  - Ensure all five chapter files are created with proper structure, all figures are in place, and tables are formatted. Ask the user if questions arise.

- [x] 7. Create bibliography
  - [x] 7.1 Create bibliografia.bib with all references
    - Extract all references from the bibliography section of TCC_PARKON_V3.pdf
    - Create one BibTeX entry per reference with at minimum: author, title, year
    - Map each reference to correct entry type: @article, @book, @inproceedings, @phdthesis, @mastersthesis, @techreport, @online, @misc
    - Generate keys using pattern {firstAuthorLastName}{year}, with letter suffix for duplicates
    - Include all available fields: journal, booktitle, volume, number, pages, publisher, doi, url
    - Add TODO comments for entries with missing required fields
    - _Requirements: 1.4, 6.1, 6.2, 6.3, 6.7_

  - [x] 7.2 Verify citation-bibliography consistency
    - Cross-check all `\cite{}` and `\textcite{}` commands in chapter files against bibliografia.bib keys
    - Replace any unmatched citations with `\cite{MISSING_key}` placeholder and add TODO comment
    - Ensure no orphan entries exist without being cited (acceptable but flagged)
    - _Requirements: 4.5, 4.6, 4.7, 6.6_

- [x] 8. Create acronym definitions
  - [x] 8.1 Create pre-textuais/acronimos.tex
    - Identify all acronyms and abbreviations used in TCC_PARKON_V3.pdf
    - Define each with `\newacronym{label}{SHORT}{Long Form}` in alphabetical order
    - Verify that chapter files use `\acrfull{}` for first occurrence and `\acrshort{}` for subsequent
    - For acronyms appearing only once, ensure `\acrfull{}` is used for that single occurrence
    - _Requirements: 7.1, 7.2, 7.4, 7.5_

- [x] 9. Create post-textual elements
  - [x] 9.1 Create appendices and annexes
    - If source has appendices: create `pos-textuais/appendixA.tex` (and B, C, etc.) with `\chapter{APÊNDICE [letter] – [Title]}`
    - If source has annexes: create `pos-textuais/anexoA.tex` (and B, C, etc.) with `\chapter{ANEXO [letter] – [Title]}`
    - Ensure `\appendix` command is in tese.tex before appendix/annex includes
    - Order: all appendix includes before all annex includes
    - If no appendices or annexes exist in source, omit `\appendix` and leave pos-textuais empty
    - _Requirements: 2.4, 10.1, 10.2, 10.3, 10.4_

- [x] 10. Finalize cross-references and navigation
  - [x] 10.1 Verify and fix all cross-references
    - Ensure all `\label{}` identifiers are unique across the project using naming convention: `cap:`, `sec:`, `fig:`, `tab:`, `eq:`, `alg:`, `apend:`, `anexo:`
    - Convert all internal references to `\autoref{}` commands pointing to correct labels
    - For unresolvable cross-references, insert `\ref{UNDEFINED_identifier}` with TODO comment
    - Verify page numbering: roman for pre-textual, arabic for textual
    - _Requirements: 9.1, 9.2, 9.4_

- [x] 11. Compilation verification
  - [x] 11.1 Compile the complete LaTeX project
    - Run the full compilation pipeline: `pdflatex tese.tex` → `biber tese` → `makeglossaries tese` → `pdflatex tese.tex` → `pdflatex tese.tex`
    - Verify zero compilation errors in log output (warnings are acceptable)
    - Fix any compilation errors found (missing references, undefined commands, encoding issues)
    - Ensure all `\include` paths match actual file names
    - Verify bibliography renders with ABNT formatting
    - Verify abbreviation list generates correctly
    - _Requirements: 8.4, 9.3_

- [x] 12. Final checkpoint - Project complete
  - Ensure all tests pass (compilation succeeds without errors), all content is migrated, and the output PDF matches the source structure. Ask the user if questions arise.

## Notes

- No property-based tests apply — this is a one-time document migration with no algorithmic functions
- Verification is done via LaTeX compilation and manual content review
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation at key milestones
- TODO comments mark any content that cannot be extracted or matched during migration
- The source content comes from TCC_PARKON_V3.pdf which needs text extraction
- Template files (PPGC_UFF.cls, biblatex-abnt/, .lbx files) are copied unchanged

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1.1"] },
    { "id": 1, "tasks": ["1.2"] },
    { "id": 2, "tasks": ["3.1", "8.1"] },
    { "id": 3, "tasks": ["3.2", "3.3"] },
    { "id": 4, "tasks": ["3.4"] },
    { "id": 5, "tasks": ["4.1", "4.2", "4.3", "4.4", "4.5"] },
    { "id": 6, "tasks": ["5.1", "5.2", "7.1"] },
    { "id": 7, "tasks": ["7.2", "9.1"] },
    { "id": 8, "tasks": ["10.1"] },
    { "id": 9, "tasks": ["11.1"] }
  ]
}
```
