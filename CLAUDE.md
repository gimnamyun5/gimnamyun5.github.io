# Project notes

Academic website for Nam-Yoon Kim, an undergraduate researcher in Statistics at
Jeonbuk National University applying to PhD programs in Statistics and
Biostatistics. Built with Quarto, deployed to GitHub Pages by GitHub Actions.

**The user writes in Korean. Reply in Korean.**

- Local: `C:\Users\user\projects\namyoon-kim-website`
  (deliberately outside OneDrive: syncing a `.git` directory invites file locks
  and conflict copies. Two earlier paths under `Documents` and
  `OneDrive\Documents` are dead.)
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

**Colour.** Two blues, and nothing else. `--navy` `#1f3a5f` (11.5:1 on white) is
the link colour; `--steel` `#3a6ea8` (5.28:1) is the accent that carries every
card. Steel is **sampled** from the publication cards on
<https://ankush-gupta04.github.io/Ankush/>, so it is a match, not a choice.

Warm accents were tried and dropped on purpose: rust `#a8452f`, amber `#9c5f16`,
vermilion `#d1401a` and the lab's terracotta `#c4603a` (`--terra` on
<https://vic-dragon.github.io>). The last of those was ruled out partly because
it measures 4.13:1 on white, under the 4.5:1 everything else holds; `#b95a36` is
the same terracotta at 4.59:1 if it ever comes back. **Reintroducing a warm
accent is a design decision, not a fix.**

`--card-accent`, set on `.info-card` and on `.research-card`, is what the top or
left rule, the labels, the arrows, the award names and the button hovers all
read. Both Research sections share the one accent, so nothing keys off the
section any more.

The page sits on `#f8f7f4` — still a warm off-white, the one warm thing left.
Tables and code blocks are pinned white; the header strip is the only full-width
white band, and the footer is painted `--page`.

**Type.** Most pages use Georgia over the system sans. Research and Publications
carry `body-classes: serif-page`, which overrides `--font-display` and
`--font-body` to EB Garamond and Inter — every rule reads those variables, so
nothing else needs changing. Research cards use Playfair Display 700 for titles,
Inter for text, IBM Plex Mono for the letter-spaced labels and buttons.
Publication cards are all Inter, including the title — see below. Web fonts load
from Google Fonts via `include-in-header` in `_quarto.yml`, with Georgia and the
system stack as fallbacks.

**Components.** `.info-card` in a `.card-grid` (home summary panels, three
across); `.research-card` (entry card with a coloured left rule); `.photo-entry`
(activities, photo left and caption right).

`.pub-card` is **ported from the publication list on
<https://vic-dragon.github.io>** and deliberately does not match
`.research-card`: a faintly tinted panel with a hairline all the way round rather
than a coloured left rule, the accent held back for the hover border and the two
footer items, and a title at Inter 500 / 0.92rem rather than as a display line.
The metrics come from that source — changing them drifts away from the thing it
was matched to. The tabs on the source (Published / Under Review / In
Preparation) were **not** ported: there is one entry, so two panels would be
empty. `.tag-index`, `.tag-role` and `.tag-status` now differ only by depth of
blue tint, so the wording carries the distinction that hue used to.

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
`.claude/launch.json` (`preview_start` with `name: namyoon-site`) — it runs
`quarto preview` on port 4321 from the project directory. The only absolute path
in it is `quarto.exe`, so moving the project needs no change there.
Screenshots fail when the pane is hidden; measure with `javascript_tool`
instead — element geometry, computed styles, contrast ratios.
