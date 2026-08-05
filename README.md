# Nam-Yoon Kim — Academic Website

A minimal academic website built with [Quarto](https://quarto.org), deployed to
GitHub Pages via GitHub Actions.

## Structure

```
.
├── _quarto.yml                  # site configuration: navbar, theme, footer
├── index.qmd                    # Home
├── research.qmd                 # Research
├── activities.qmd               # Activities & Photos
├── contact.qmd                  # Contact
├── styles.css                   # custom styling layered over the cosmo theme
├── files/                       # PDFs (CV, posters, manuscripts)
├── images/                      # profile photograph, favicon
├── .github/workflows/publish.yml
├── .gitattributes               # keeps PDFs/images out of line-ending conversion
├── .gitignore
└── _site/                       # render output (git-ignored)
```

## Prerequisites

- [Quarto CLI](https://quarto.org/docs/get-started/) (1.4 or newer)
- Git

No R or Python installation is required: the pages contain no executable code
chunks.

## Local development

Render the whole site once:

```bash
quarto render
```

Start a live-reloading preview in your browser:

```bash
quarto preview
```

Check that the project is recognized and see which version of Quarto is in use:

```bash
quarto check
```

## Placeholders to fill in

Search the project for `PLACEHOLDER` and replace every occurrence:

```bash
grep -rn "PLACEHOLDER" --include="*.qmd" --include="*.yml" .
```

Identity and links (name, email, GitHub, LinkedIn, affiliation, site URL) are
already filled in. What remains is research and CV content:

| Location | What to replace |
| --- | --- |
| `index.qmd` | news items, coursework/honors |
| `research.qmd` | research questions, data, methods, findings, repo and poster links |
| `activities.qmd` | further activities and photographs |
| `contact.qmd` | Google Scholar / ORCID links (or delete those lines) |

## Files to add

- `files/NamYoon_Kim_CV.pdf` — the CV PDF the button on the CV page opens.
- `images/profile.jpg` — a professional profile photograph (square crop,
  roughly 600×600 px).
- `images/favicon.png` — optional; remove the `favicon:` line in `_quarto.yml`
  if you do not add one.

## Publishing to GitHub Pages

1. Create a GitHub repository named exactly `gimnamyun5.github.io` and push this
   project to the `main` branch. It must be **public** for GitHub Pages to serve
   it on a free account.
2. In the repository, go to **Settings → Pages** and set **Source** to
   **GitHub Actions**.
3. Push to `main`. The workflow in `.github/workflows/publish.yml` renders the
   site and deploys it.
4. The site appears at <https://gimnamyun5.github.io/>.

`site-url` in `_quarto.yml` is already set to that address. If you rename the
repository, update `site-url` and `repo-url` to match.

## License

PLACEHOLDER: choose a license, or state that the content is all rights reserved.
