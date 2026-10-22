# jamesjgriebler.github.io

Personal academic site. Plain HTML and one stylesheet — no build step, no
dependencies, no JavaScript. Edit a file, commit, push; GitHub Pages serves it
within a minute or so.

Adapted from the site template by Rachel N. Gaines
([rngaines/main](https://github.com/rngaines/main)).

## Layout

```
index.html           Home: name, photo, tagline, about, news
research.html        Research Vision: thrusts, one section each
publications.html    Publication cards grouped by year
resources.html       Protocols, code and mentoring documents
cv.html              CV & Bio: one profile card, degrees, bio, links
404.html             Served for bad URLs
assets/styles/site.css
assets/img/          headshot.jpg, favicon.png, og-card.png
assets/figures/      Research figures
assets/pdf/          Paper PDFs linked from publication cards
assets/resources/    Files linked from the Resources page
CV/Griebler-CV.pdf   Keep this filename stable; old links must not rot
```

## Filling it in

Unwritten text is wrapped in `<span class="placeholder">`, which renders with a
yellow highlight so you can see at a glance what is left. To list everything
outstanding:

```bash
grep -rn "placeholder\|TODO" --include=*.html .
```

When the site is written, delete the `.placeholder` rule from `site.css` — any
highlight you then still see is text you missed.

## Adding a page

Copy the nearest existing page, move `class="active"` to its own nav entry, and
add the page to the nav list in **all** of the other pages. There is no template
engine, so the nav is duplicated per file. Six copies is manageable; if the site
grows past about ten pages, that is the point to add a generator.

The page set mirrors rngaines.github.io: Home, Research Vision, Publications,
Resources, CV & Bio. A standalone Teaching page and Contact page were written and
then removed to match; recover either with `git show a436102:teaching.html`.
Teaching materials otherwise belong under Resources, and contact details on the
CV & Bio card, which is how hers is arranged.

## Adding a publication

Copy one `<div class="card">` block in `publications.html` into the right year's
`card-grid` and edit it. Numbering runs newest-first. Write DOI links absolute —
`https://doi.org/...`, not `doi.org/...`, which the browser would resolve as a
path on this site.

## Design

Palette and type are per the supplied spec, with four changes made for text
contrast (WCAG AA is 4.5:1 for body text, 3:1 for large text):

| Change | Why |
| --- | --- |
| Nav bar, links and buttons use `--primary-deep` `#5E7183`, not `#657A8B` | White on `#657A8B` is 4.46:1, just under the floor. `#5E7183` is 5.0:1 and looks the same. |
| `--primary` `#8A9FB0` is never a background for white text | 2.74:1. It is used for card borders and accents only. |
| `h4`–`h6` use `--text-dark` | `#657A8B` on the page background is 4.34:1 — fine for `h1`–`h3` at large sizes, too low below ~1.2rem. |
| Captions and metadata use `--text-medium` | `--text-light` `#9AA2A8` is 2.6:1. It is for rules and borders. |

`--special-accent` `#6F8E8A` is 3.55:1 on white, so it is used for callout
borders and hover backgrounds, not for text. `--special-accent-dark` `#5C7773`
(4.84:1) carries white text on hover.

Fonts are Cinzel (the name), Inter (headings and UI) and Figtree (body), in one
Google Fonts request. Cinzel stands in for Felix Titling, which is a Monotype
face bundled with Microsoft Office and not available as a web font.

Fixes carried over from the template: `<main>` in place of the invented
`<mainContent>` element, the corrected SVG namespace, a valid width on `.logo`,
de-duplicated `.media-container` rules, and a flexbox nav instead of floats so
it stacks cleanly on a phone.

## Local preview

No server needed — open `index.html` in a browser. For a server anyway:

```bash
python -m http.server 8000
```

## Deploying

Push to `main`. Pages is configured under Settings → Pages → Deploy from branch
→ `main` / root.

For a custom domain later: add a `CNAME` file containing the bare domain, then a
DNS record pointing at GitHub Pages. On Cloudflare that record must be
**DNS-only, not proxied**, or Pages cannot issue the TLS certificate.
