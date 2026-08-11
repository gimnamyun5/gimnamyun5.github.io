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

**Colour.** Two blues everywhere except the publication cards. `--navy` `#1f3a5f`
(11.5:1 on white) is the link colour; `--steel` `#3a6ea8` (5.28:1) is the accent
on the home cards, the Research cards and the heading bars. Steel is **sampled**
from the publication cards on <https://ankush-gupta04.github.io/Ankush/>, whose
page colour `#faf7f2` is also matched exactly as `--page`.

`.pub-card` is the exception: colours as well as metrics are sampled from
<https://vic-dragon.github.io>, so that one component is warm. Rust `#a8452f`,
amber `#9c5f16` and vermilion `#d1401a` were all tried and dropped along the way
and are not coming back; the terracotta family below is the sampled set, not a
palette to extend.

**The publication cards are low contrast on purpose.** The user wants that page to
read quietly — little separation between the parts — and chose these values
knowing the numbers. Do not "fix" them, and do not raise it again as a defect.
Measured on the card (`#f1ede8`), against the 4.5:1 the rest of the site holds:

| Part | Colour | Contrast |
| --- | --- | --- |
| title | `#2d2926` | 12.37 ✓ |
| journal, DOI link, hover border | `#c4603a` | 3.55 |
| authors | `#9c8874` | 2.91 |
| `.tag-status` | `#c4603a` | 2.99 |
| `.tag-coauthor` | `#9c8874` | 2.42 |
| `.tag-index` | `#6b8fd4` | 2.19 |
| `.tag-first` | `#e07a52` | 2.03 |
| `.tag-cofirst` | `#5aad82` | 1.76 |
| `.tag-corresponding` | `#d4915a` | 1.67 |
| `.tag-kci` | `#6bb8d4` | 1.57 |

Only the title passes, and these figures match the source page to within 0.01 —
both the match and the softness are wanted. If the decision is ever reversed,
darkening each text colour while keeping the tints holds the look and clears
4.5:1; that is the only change to make, and only when asked.

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
<https://vic-dragon.github.io>** — metrics and colours both — and deliberately
does not match `.research-card`: a warm tinted panel (`#f1ede8`) with a hairline
all the way round rather than a coloured left rule, terracotta held back for the
hover border and the two footer items, and a title at Inter 500 / 0.92rem rather
than as a display line. Everything there is a sampled value; changing any of it
drifts away from the thing it was matched to.

The card is set to the flat colour the source's translucent tint composites to
over *its* page, so it reads identically here on a different white. The badge
tints stay translucent, which lands them on the source values exactly because the
card underneath now matches.

One badge class per kind, each its own hue: `.tag-index` (SCIE), `.tag-kci`,
`.tag-first`, `.tag-cofirst`, `.tag-corresponding`, `.tag-coauthor`,
`.tag-status`. The template in `publications.qmd` lists them all. The tabs on the
source (Published / Under Review / In Preparation) were **not** ported: there is
one entry, so two panels would be empty.

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
