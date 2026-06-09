# Requirements Document

## Introduction

This document specifies the requirements for migrating the TCC PARKON V3 document (PDF) to LaTeX format using the PPGC-UFF-Latex-Abnt template. The migration preserves all original content verbatim while restructuring the formatting to comply with the PPGC_UFF class and ABNT norms as defined by the template. The source of truth for content is exclusively TCC_PARKON_V3.pdf. The target LaTeX project uses MikTeX as its distribution and biblatex-abnt for bibliography management.

## Glossary

- **Migration_System**: The process and tooling responsible for converting content from TCC_PARKON_V3.pdf into LaTeX source files following the PPGC-UFF-Latex-Abnt template structure
- **Source_Document**: The file TCC_PARKON_V3.pdf located at c:\Users\anton\Documents\TCC\TCC_PARKON_V3.pdf, containing the complete TCC content
- **Template**: The PPGC-UFF-Latex-Abnt LaTeX template located at c:\Users\anton\Documents\TCC\PPGC-UFF-Latex-Abnt\, which defines the formatting rules, class file, and folder structure
- **PPGC_UFF_Class**: The document class file PPGC_UFF.cls that provides ABNT-compliant commands for cover, title page, approval sheet, abstract, and chapter formatting
- **Pre_Textuais**: The folder pre-textuais\ containing LaTeX files for elements before the main text (cover data, acronyms, abstracts, lists)
- **Capitulos**: The folder capitulos\ containing LaTeX files for each chapter of the main text
- **Pos_Textuais**: The folder pos-textuais\ containing LaTeX files for appendices and annexes
- **Main_File**: The root LaTeX file (tese.tex) that includes all other files and configures packages
- **Bibliografia_File**: The BibTeX file (bibliografia.bib) containing all bibliographic references in biblatex-abnt format
- **MikTeX**: The LaTeX distribution used for compiling the output documents on Windows

## Requirements

### Requirement 1: Content Fidelity

**User Story:** As the document author, I want all textual content from TCC_PARKON_V3.pdf to be preserved exactly in the LaTeX output, so that no information is lost or altered during migration.

#### Acceptance Criteria

1. THE Migration_System SHALL transfer all body text from the Source_Document to the LaTeX output files without alteration of wording or numerical data, including paragraphs, bullet lists, numbered lists, block quotes, and inline citations
2. THE Migration_System SHALL preserve all section titles, subsection titles, and sub-subsection titles from the Source_Document with character-for-character identical text
3. THE Migration_System SHALL preserve all figure captions, table captions, and footnotes from the Source_Document with character-for-character identical text
4. THE Migration_System SHALL preserve all bibliographic citation content from the Source_Document in the Bibliografia_File, including author names, titles, publication year, publisher, and identifiers such as DOI or URL where present in the source
5. IF content from the Source_Document cannot be extracted or represented in LaTeX, THEN THE Migration_System SHALL insert a clearly visible comment in the corresponding LaTeX file identifying the location and description of the unresolved content for manual review
6. THE Migration_System SHALL preserve all special characters and diacritics from the Source_Document using their equivalent LaTeX representations, maintaining the same visible output when compiled

### Requirement 2: Template Structure Compliance

**User Story:** As the document author, I want the LaTeX project to follow the PPGC-UFF-Latex-Abnt template folder structure, so that the output is organized according to university standards.

#### Acceptance Criteria

1. THE Migration_System SHALL produce a Main_File that uses the PPGC_UFF_Class with document class options matching the Template configuration (ruledheader, 12pt, openright, a4paper, oneside, english, brazil)
2. THE Migration_System SHALL place pre-textual elements (cover data, approval page, dedication, acknowledgments, abstracts, lists of figures, lists of tables, acronyms, and table of contents) in files within the pre-textuais folder
3. THE Migration_System SHALL place each chapter in a separate .tex file within the capitulos folder, where a chapter corresponds to each top-level heading (Heading 1) from the source document
4. THE Migration_System SHALL place appendices and annexes in separate .tex files within the pos-textuais folder
5. THE Migration_System SHALL produce a Main_File that includes all files using \include commands in ABNT order: pre-textual elements first, then chapter files in their original document sequence, then bibliography via \printbibliography, then appendices, then annexes
6. THE Migration_System SHALL place the bibliography database in a .bib file in the project root directory and reference it in the Main_File using \addbibresource
7. THE Migration_System SHALL place the PPGC_UFF.cls class file, the biblatex-abnt folder, and language support files (.lbx) in the project root directory so that the Main_File compiles without missing dependencies

### Requirement 3: Pre-Textual Elements

**User Story:** As the document author, I want all pre-textual elements from TCC_PARKON_V3.pdf to be properly formatted using PPGC_UFF_Class commands, so that the cover page, title page, and front matter comply with ABNT norms.

#### Acceptance Criteria

1. THE Migration_System SHALL populate the \autor, \titulo, \instituicao, \orientador, \local, and \data commands in Pre_Textuais with the corresponding values extracted from the Source_Document cover page and title page
2. IF the Source_Document contains a co-advisor (coorientador), THEN THE Migration_System SHALL populate the \coorientador command with the co-advisor name; otherwise the \coorientador line SHALL be omitted
3. THE Migration_System SHALL populate the \comentario command with the TCC description text following the structure: "Trabalho de Conclusão de Curso apresentado ao Curso de Sistemas de Informação da Universidade Federal Fluminense como requisito parcial para a obtenção do Grau de Bacharel em Sistemas de Informação." followed by the concentration area from the Source_Document
4. THE Migration_System SHALL generate the approval page (termo de aprovação) containing the student name, work title, approval date (month and year), and each banca examinadora member with their full name, academic title, and affiliated institution, with a maximum of 5 banca members including the orientador
5. THE Migration_System SHALL transfer the resumo text from the Source_Document into the \begin{resumo} environment, followed by a Palavras-chave entry formatted as a bold-labeled list of comma-separated terms
6. THE Migration_System SHALL transfer the abstract text from the Source_Document into the \begin{abstract} environment, followed by a Keywords entry formatted as a bold-labeled list of comma-separated terms
7. WHEN the Source_Document contains a dedication section, THE Migration_System SHALL transfer its content into a right-aligned italic block on a dedicated page before the acknowledgments
8. WHEN the Source_Document contains an acknowledgments section, THE Migration_System SHALL transfer its content into a \pretextualchapter{Agradecimentos} environment

### Requirement 4: Chapter Content Migration

**User Story:** As the document author, I want each chapter from TCC_PARKON_V3.pdf converted to a properly structured LaTeX file, so that all sectioning, paragraphs, and formatting follow ABNT norms.

#### Acceptance Criteria

1. THE Migration_System SHALL create one .tex file per chapter in the capitulos folder, named sequentially (cap1.tex through cap5.tex), where each file is a LaTeX fragment containing no \documentclass or preamble declarations and is includable via the \include command in tese.tex
2. THE Migration_System SHALL use \chapter for primary sections, \section for secondary sections, \subsection for tertiary sections, and \subsubsection for quaternary sections, mapping each heading level from the Source_Document based on its visual hierarchy (font size and weight) in the PDF
3. THE Migration_System SHALL represent each paragraph from the Source_Document as text separated by exactly one blank line, and SHALL encode all output files in UTF-8
4. WHEN the Source_Document contains a direct quotation that occupies more than three printed lines in the source PDF, THE Migration_System SHALL format the quotation using a \begin{quoting}[rightmargin=0cm,leftmargin=4cm] block containing \begin{singlespace} and \footnotesize, as defined in the Template
5. WHEN the Source_Document contains a parenthetical citation (author-year reference enclosed in parentheses), THE Migration_System SHALL convert it to a \cite command referencing the matching entry key in the Bibliografia_File
6. WHEN the Source_Document contains a narrative citation (author name used as part of the sentence followed by the year in parentheses), THE Migration_System SHALL convert it to a \textcite command referencing the matching entry key in the Bibliografia_File
7. IF a citation in the Source_Document cannot be matched to any existing entry key in the Bibliografia_File, THEN THE Migration_System SHALL insert a placeholder \cite{MISSING_key} comment with the original citation text so the author can resolve it manually

### Requirement 5: Figures and Tables

**User Story:** As the document author, I want all figures and tables from TCC_PARKON_V3.pdf to be included in the LaTeX output with proper formatting, so that visual content is preserved.

#### Acceptance Criteria

1. WHEN the Source_Document contains figures, THE Migration_System SHALL include each figure using the \includegraphics command within a figure environment with [!ht] positioning, \centering alignment, and a width parameter of 0.8\linewidth as default scaling
2. THE Migration_System SHALL place all figure image files in the capitulos/figuras/ directory in PNG or EPS format
3. THE Migration_System SHALL assign each figure a \caption with the original caption text and a \label with the prefix "fig:" followed by a descriptive identifier for cross-referencing
4. WHEN the Source_Document contains tables, THE Migration_System SHALL reproduce each table using the tabular environment inside a table environment with [!ht] positioning, \begin{center} wrapping, \caption placed above the tabular content, and a \label with the prefix "tab:" followed by a descriptive identifier for cross-referencing
5. THE Migration_System SHALL use the figure and table counter configuration from the Template (counterwithout chapter, producing sequential numbering like "Figura 1", "Tabela 1")
6. IF a figure cannot be extracted from the Source_Document as an image file, THEN THE Migration_System SHALL insert a placeholder figure environment containing a comment indicating the missing figure and its original caption text

### Requirement 6: Bibliography and Citations

**User Story:** As the document author, I want all references from TCC_PARKON_V3.pdf to be converted to biblatex-abnt entries, so that citations and the reference list comply with ABNT formatting.

#### Acceptance Criteria

1. THE Migration_System SHALL create a Bibliografia_File containing one BibTeX entry for each reference listed in the Source_Document, where each entry includes at minimum the fields: author, title, and year
2. THE Migration_System SHALL map each reference to a BibTeX entry type according to the following rules: journal articles to @article, books to @book, conference papers to @inproceedings, theses and dissertations to @phdthesis or @mastersthesis, technical reports to @techreport, websites and URLs to @online, and all other sources to @misc
3. THE Migration_System SHALL generate a BibTeX key for each entry using the pattern {firstAuthorLastName}{year} (e.g., silva2020), and if duplicates arise, append a lowercase letter suffix (e.g., silva2020a, silva2020b)
4. THE Migration_System SHALL configure biblatex in the Main_File with style=abnt, language=brazil, and backend=biber, and include \addbibresource pointing to the Bibliografia_File, as defined in the Template
5. THE Migration_System SHALL use \printbibliography with title={REFERÊNCIAS} to produce the reference list
6. WHEN the Source_Document uses numbered citations, THE Migration_System SHALL replace each numbered citation in the text with the corresponding \cite command using the BibTeX key of the matching entry in the Bibliografia_File
7. IF a reference in the Source_Document is missing required fields (author, title, or year), THEN THE Migration_System SHALL create the BibTeX entry with the available fields and insert a comment in the .bib file indicating which fields are missing

### Requirement 7: Acronyms and Abbreviations

**User Story:** As the document author, I want all acronyms and abbreviations from TCC_PARKON_V3.pdf to be managed through the glossaries package, so that the abbreviation list is generated automatically.

#### Acceptance Criteria

1. THE Migration_System SHALL define each acronym and abbreviation identified in the Source_Document using a \newacronym{<label>}{<short form>}{<long form>} command in the file pre-textuais/acronimos.tex, with one command per acronym
2. WHEN generating chapter content files, THE Migration_System SHALL use \acrfull{<label>} for the first occurrence of each acronym within the document body and \acrshort{<label>} for every subsequent occurrence of the same acronym
3. THE Migration_System SHALL include the command \printglossary[type=\acronymtype,title={Lista de Abreviaturas e Siglas}] in pre-textuais/cap0.tex to generate the abbreviation list
4. THE Migration_System SHALL include the \makeglossaries command and \input{pre-textuais/acronimos} in the main .tex file before \begin{document} to enable glossary compilation
5. IF an acronym appears only once in the document body, THEN THE Migration_System SHALL still use \acrfull{<label>} for that single occurrence to ensure it appears in the generated abbreviation list

### Requirement 8: Package Configuration and Compilation

**User Story:** As the document author, I want the LaTeX project to compile successfully with MikTeX using the same package configuration as the Template, so that the output PDF is generated without errors.

#### Acceptance Criteria

1. THE Migration_System SHALL include all packages listed in the Template Main_File in the following order: babel (with option brazil), fontenc (with option T1), inputenc (with option utf8), epsf, subfigure, epsfig and graphicx (with option dvips), chngcntr, pifont, textcomp, latexsym, amssymb, amstext, amsthm, icomma, amsmath, algorithm, algorithmic, url, longtable, lscape, quoting, bigdelim, booktabs, colortbl, multirow, multicol, rotating, glossaries (with options acronym, nopostdot, shortcuts, nonumberlist), hyperref, and biblatex (with options style=abnt, language=brazil, backend=biber)
2. THE Migration_System SHALL configure hyperref with bookmarksopen=true, linktoc=page, colorlinks=true, linkcolor=blue, citecolor=blue, filecolor=magenta, and urlcolor=blue as defined in the Template
3. THE Migration_System SHALL configure the font family using ptm (Times) with bold series for chapter headers (via \ABNTchapterfont with \bfseries\fontfamily{ptm}\fontseries{b}\selectfont) and section headers (via \ABNTsectionfont with \bfseries\fontfamily{ptm}) as defined in the Template
4. WHEN the output LaTeX project is compiled with pdflatex followed by biber followed by makeglossaries followed by pdflatex (twice), THE Migration_System SHALL produce a PDF file with no compilation errors in the log output (warnings are acceptable)
5. THE Migration_System SHALL load hyperref after all other content packages and load biblatex as the last package, preserving the package load order defined in the Template Main_File

### Requirement 9: Cross-References and Navigation

**User Story:** As the document author, I want all internal cross-references from TCC_PARKON_V3.pdf to function as hyperlinked references in the LaTeX output, so that the document is navigable.

#### Acceptance Criteria

1. WHEN the Source_Document references a figure, table, section, equation, or algorithm by number, THE Migration_System SHALL convert the reference to a \autoref command pointing to a \label assigned to the corresponding element, using the naming convention `type:identifier` (e.g., `fig:nome`, `tab:nome`, `sec:nome`)
2. IF a cross-reference in the Source_Document cannot be matched to any labeled element in the generated LaTeX output, THEN THE Migration_System SHALL insert a placeholder \ref command with a clearly identifiable undefined label and emit a warning indicating the unresolved reference and its location in the source
3. THE Migration_System SHALL generate a table of contents using \tableofcontents, a list of figures using \listoffigures, and a list of tables using \listoftables in Pre_Textuais
4. THE Migration_System SHALL configure page numbering with roman numerals for pre-textual pages and arabic numerals starting from the first textual chapter page, as defined in the Template

### Requirement 10: Post-Textual Elements

**User Story:** As the document author, I want appendices and annexes from TCC_PARKON_V3.pdf to be properly structured in the LaTeX output, so that they follow ABNT norms for post-textual elements.

#### Acceptance Criteria

1. WHEN the Source_Document contains appendices, THE Migration_System SHALL create a separate .tex file for each appendix in the Pos_Textuais directory, named sequentially (e.g., appendixA.tex, appendixB.tex), each starting with a \chapter command whose title follows the format "APÊNDICE [letter] – [Title]" using uppercase consecutive letters (A through Z)
2. WHEN the Source_Document contains annexes, THE Migration_System SHALL create a separate .tex file for each annex in the Pos_Textuais directory, named sequentially (e.g., anexoA.tex, anexoB.tex), each starting with a \chapter command whose title follows the format "ANEXO [letter] – [Title]" using uppercase consecutive letters (A through Z)
3. THE Migration_System SHALL include the \appendix command in the Main_File before including any appendix or annex files, and SHALL order all appendix \include directives before all annex \include directives
4. IF the Source_Document contains no appendices and no annexes, THEN THE Migration_System SHALL omit the \appendix command and not generate any files in the Pos_Textuais directory for post-textual elements
