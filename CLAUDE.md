# Project notes

Academic website for Nam-Yoon Kim, an undergraduate researcher in Statistics at
Jeonbuk National University applying to PhD programs in Statistics and
Biostatistics. Built with Quarto, deployed to GitHub Pages by GitHub Actions.

**The user writes in Korean. Reply in Korean.**

- Local: `C:\Users\user\OneDrive\Documents\namyoon-kim-website`
  (OneDrive moved this out of `C:\Users\user\Documents` — that older path is dead)
- Repository: <https://github.com/gimnamyun5/gimnamyun5.github.io>
- Live: <https://gimnamyun5.github.io/>

## Deploying

```bash
quarto render
git add -A
git commit -m "..."
git push
```

Pushing to `main` triggers `.github/workflows/publish.yml`, which renders on the
runner and deploys. A run takes about a minute. `_site/` is git-ignored — the
deployed HTML is built by the workflow, not committed.

Quarto is at `C:\Program Files\Quarto\bin\quarto.exe` and is not on the shell
PATH by default; prepend the machine and user PATH before calling it.

## Pages

| File | Page | Notes |
| --- | --- | --- |
| `index.qmd` | Home | `body-classes: home-page` |
| `about.qmd` | About | Background, teaching duties, contact details |
| `activities.qmd` | Activities & Photos | Photo entries |
| `research.qmd` | Research (titled "Research experience") | Entry cards |
| `publications.qmd` | Publications | Entry cards, papers only |
| `project-*.qmd` | Four project detail pages | Linked from the research cards |
| `_parked-community-detection.qmd` | — | Not rendered; the leading `_` excludes it |

Navbar order is Home, About, Activities & Photos, Research, Publications, all
right-aligned. There is no CV page: the button links straight to the PDF.

## Design system

All of it lives in `styles.css`.

**Colour.** Three accents, all above 4.5:1 on white: navy `#1f3a5f` (links and
the default), rust `#a8452f`, amber `#9c5f16`. Add `.accent-rust` or
`.accent-amber` to a card to switch its accent. The page sits on `#f4f3f0`; the
header strip, cards, tables and code blocks are pinned white.

**Type.** Most pages use Georgia over the system sans. Research and Publications
carry `body-classes: serif-page`, which overrides `--font-display` and
`--font-body` to EB Garamond and Inter — every rule reads those variables, so
nothing else needs changing. Inside entry cards: Playfair Display 700 for
titles, Inter for text, IBM Plex Mono for the letter-spaced labels and buttons.
Web fonts load from Google Fonts via `include-in-header` in `_quarto.yml`, with
Georgia and the system stack as fallbacks.

**Components.** `.info-card` in a `.card-grid` (home summary panels, three
across); `.research-card` and `.pub-card` (entry cards with a coloured left
rule); `.photo-entry` (activities, photo left and caption right).

## Still to fill in

Search for `PLACEHOLDER`:

```bash
grep -rn "PLACEHOLDER" --include="*.qmd" .
```

- **`files/NamYoon_Kim_CV.pdf` is a dummy.** One page reading "PLACEHOLDER". The
  CV button on the home page and the About page both open it. Replace it,
  keeping the filename.
- **ICU manuscript author list** — unknown, on `research.qmd`,
  `publications.qmd` and `project-icu-transfer.qmd`.
- **ICU project detail** — data, methods and findings on
  `project-icu-transfer.qmd` are all placeholder.
- **A personal sentence or two** on `index.qmd` and `about.qmd`, marked optional.
- Google Scholar and ORCID links on `about.qmd`, if they exist.
- Poster PDFs: hold until the co-authors agree — the dementia poster is joint
  with Jiyoun Sung (SNU), the Granger poster has six authors.

## Things that have bitten us

- **Never rewrite a `.qmd` or `.css` through PowerShell's `Get-Content`/
  `Set-Content`.** It decodes UTF-8 as the Korean ANSI codepage and turns every
  em dash and middle dot into mojibake. Use the Edit tool, or Python with an
  explicit `encoding="utf-8"`.
- **A running `quarto preview` overwrites `_site`.** Stop the preview server
  before `quarto render`, or you will verify stale output.
- **GitHub Pages caches CSS for 10 minutes.** After a style change, tell the
  user to hard-refresh (Ctrl+F5); a normal reload keeps the old stylesheet.
- **Pandoc wraps things in `<p>`.** A div holding a link or an image gets an
  extra paragraph, which carries margins and blocks flex layout. Several rules
  exist to undo this (`.profile-photo p`, `.research-card > p:has(...)`,
  `.profile-links p`).
- **`border-bottom` is both an underline and a box edge.** Link underlines are
  drawn with `text-decoration` for this reason; using a border once erased the
  bottom edge of every outline button.
- Photographs arrive as phone originals of 3–14 MB. Resize to about 1200–1400px
  on the long side, JPEG quality 85, and apply `ImageOps.exif_transpose` before
  saving so orientation survives the metadata being dropped.

## Verifying

The browser pane blocks plain localhost, so preview through
`.claude/launch.json` (`preview_start` with `name: namyoon-site`) — note it
holds an absolute path to the project and needs updating if the folder moves.
Screenshots fail when the pane is hidden; measure with `javascript_tool`
instead — element geometry, computed styles, contrast ratios.
