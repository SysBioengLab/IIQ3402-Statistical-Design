# How to Update the Jupyter Book

## Quick reference

| Task | Command |
|------|---------|
| Build HTML | `conda run -n SaaLab jupyter-book build book/` |
| Build PDF | `conda run -n SaaLab jupyter-book build book/ --builder pdflatex` |
| Preview locally | open `book/_build/html/index.html` in browser |
| Clean build cache | `conda run -n SaaLab jupyter-book clean book/` |

---

## 1. Updating content in a chapter

Each chapter is a notebook inside `book/chapters/`. Edit the notebook directly:

```
book/chapters/
├── 01_fundamentos_python.ipynb        ← Python basics
├── 02_estadistica_descriptiva.ipynb   ← EDA & descriptive stats
├── 03_distribuciones_discretas.ipynb  ← Discrete distributions
├── 04_distribuciones_continuas.ipynb  ← Continuous distributions
├── 05_inferencia_tlc_ic.ipynb         ← CLT & confidence intervals
└── 06_tests_hipotesis.ipynb           ← Hypothesis tests
```

After editing, rebuild:
```bash
conda run -n SaaLab jupyter-book build book/
```

> **Do not edit** `Ayudantias/Torpedo_iiq3402.ipynb` for book content —  
> the chapter notebooks are the source of truth for the book.

---

## 2. Adding a new chapter

**Step 1 — Create the notebook:**
```
book/chapters/07_my_new_chapter.ipynb
```
Make sure the first cell is a markdown cell with a `#` H1 heading.

**Step 2 — Register it in `book/_toc.yml`:**
```yaml
chapters:
  ...
  - file: chapters/07_my_new_chapter
    title: "7. My New Chapter"
```

**Step 3 — Rebuild:**
```bash
conda run -n SaaLab jupyter-book build book/
```

---

## 3. Changing the theme / appearance

All visual settings are in two files:

### `book/_config.yml` — layout & theme engine
```yaml
sphinx:
  config:
    html_theme: pydata_sphinx_theme        # swap theme here
    html_theme_options:
      show_toc_level: 2                    # depth of right-side TOC
      pygments_light_style: tango          # code highlight style (light)
      pygments_dark_style: monokai         # code highlight style (dark)
```

Other available themes (all pip-installable):
| Theme | Install | `html_theme` value |
|-------|---------|-------------------|
| PyData (current) | pre-installed | `pydata_sphinx_theme` |
| Sphinx Book | pre-installed | `sphinx_book_theme` |
| Furo | `pip install furo` | `furo` |
| Read the Docs | `pip install sphinx-rtd-theme` | `sphinx_rtd_theme` |

### `book/_static/custom.css` — colours, fonts, spacing
Edit the CSS variables at the top to change brand colours:
```css
:root {
  --pst-color-primary:   #0f4c75;   /* deep teal */
  --pst-color-accent:    #f5a623;   /* amber */
}
```

---

## 4. Publishing to GitHub Pages

The book auto-deploys on every push to `main` via GitHub Actions.

To trigger manually:
1. Go to the repo on GitHub
2. Actions → **Deploy Jupyter Book to GitHub Pages** → **Run workflow**

To enable GitHub Pages for the first time:
1. Repo → **Settings** → **Pages**
2. Source: **GitHub Actions**

The published URL will be:
```
https://ggmirandac.github.io/IIQ3402-Statistical-Design/
```

---

## 5. Building a PDF locally

Requires pdflatex (BasicTeX, installed via Homebrew):
```bash
eval "$(/usr/libexec/path_helper)"   # only needed if pdflatex not in PATH
conda run -n SaaLab jupyter-book build book/ --builder pdflatex
```
Output: `book/_build/latex/book.pdf`

---

## 6. Troubleshooting

**Build fails with "kernel not found"**  
→ The notebooks use `python3`. Make sure the SaaLab env is active:
```bash
conda activate SaaLab
jupyter-book build book/
```

**Chapters show wrong numbering or missing TOC**  
→ Clean the cache and rebuild:
```bash
conda run -n SaaLab jupyter-book clean book/
conda run -n SaaLab jupyter-book build book/
```

**GitHub Actions fails with dependency error**  
→ Update the `pip install` line in `.github/workflows/deploy-book.yml` to match your local packages.

**CSS changes not showing in browser**  
→ Hard refresh: `Cmd+Shift+R` (Mac) or `Ctrl+Shift+R` (Windows/Linux).
