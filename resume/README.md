# Yunqi Guo - Resume/CV

This folder contains the LaTeX source files for Yunqi Guo's professional resume/CV,
maintained together with the personal website. The compiled PDFs are served by the
site from `assets/pdf/` (this folder is excluded from the Jekyll build in `_config.yml`).

## Files

- `main.tex` - Main LaTeX document containing the resume content
- `resume.cls` - Custom LaTeX class file that defines the resume formatting and structure

The Chinese version of the resume is intentionally kept out of this public
repository and maintained separately.

## Updating the website copy

Just edit the sources and push to `master`: the `build-resume` GitHub Actions
workflow compiles the LaTeX in CI, commits the fresh `assets/pdf/resume.pdf`
(where the CV page `_pages/cv.md` embeds it), and the site redeploys
automatically. No local LaTeX toolchain is required.

To preview locally before pushing:

```bash
latexmk -pdf main.tex
cp main.pdf ../assets/pdf/resume.pdf
latexmk -C main.tex
```

Compiled PDFs are intentionally not tracked inside this folder (see `.gitignore`);
only the copy in `assets/pdf/` is committed.

## Requirements

To compile this resume, you need a LaTeX distribution installed on your system:

- **TeX Live** (Linux/Unix/Mac) - https://www.tug.org/texlive/
- **MiKTeX** (Windows) - https://miktex.org/
- **MacTeX** (macOS) - https://www.tug.org/mactex/

### Required LaTeX Packages

The following packages are used and should be available in your LaTeX distribution:

- `geometry` - Page layout and margins
- `hyperref` - Hyperlinks and PDF metadata
- `xcolor` - Color definitions
- `graphicx` - Graphics support
- `tabularx` - Extended table functionality
- `fontawesome` - Font Awesome icons
- `amsmath` - Advanced math typesetting
- `array` - Enhanced array and tabular support
- `ifthen` - Conditional statements

## Compilation

### Using pdflatex

```bash
pdflatex main.tex
```

You may need to run it twice to resolve all references:

```bash
pdflatex main.tex
pdflatex main.tex
```

### Using latexmk (Recommended)

```bash
latexmk -pdf main.tex
```

Or for continuous compilation:

```bash
latexmk -pdf -pvc main.tex
```

## Output

The compilation will generate `main.pdf` in the same directory.

## Template Credits

This resume template is based on the "Medium Length Professional CV" template from [LaTeXTemplates.com](http://www.LaTeXTemplates.com), originally created by Trey Hunner.

## License

This resume template is provided as-is. Please refer to the original template license for usage terms.
