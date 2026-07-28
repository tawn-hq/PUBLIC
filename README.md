# Tawn — public site

The marketing and onboarding site for [Tawn](https://github.com/tawn-hq/tawn),
served at <https://tawn-hq.github.io/PUBLIC/>.

## What this is

One static page. No framework, no build step, no dependencies.

That is deliberate. The job of this site is to be found and understood — by
people and increasingly by AI search engines — and both read HTML best when it
is simply there in the document rather than assembled by JavaScript after the
fact. A build step would add failure modes and buy nothing.

```
index.html       the landing page
docs.html        full documentation, written for non-developers
contribute.html  how to help, including the ways that are not code
styles.css       one stylesheet, tokens taken from the product's own theme
llms.txt         a plain-text summary written for AI crawlers
robots.txt       explicitly welcomes AI crawlers
sitemap.xml
og.png           social preview card
favicon.svg      the cairn glyph, from the product's brand assets
tawn-*.svg       official mark and lockup, copied from tawn/brand/
```

Brand assets are copies of `tawn/brand/*.svg` — if the mark changes there,
recopy rather than editing here.

## Editing

Open `index.html`. It is ordinary HTML with semantic sections.

Three things to keep in step when you change the copy:

1. **`llms.txt`** — the summary AI crawlers read. If a claim on a page changes,
   change it here too, or the two will disagree and the wrong one may be what
   gets quoted back at you.
2. **The `FAQPage` JSON-LD block** in `index.html` — search engines render
   those answers directly, so they must match the visible FAQ exactly.
3. **`sitemap.xml`** — add any new page.

## Local preview

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

## The feedback form

The form posts to [Formspree](https://formspree.io) at
`https://formspree.io/f/mpqvbelp`, which forwards submissions to the configured
email address without any backend here.

It is built as **progressive enhancement**, which is worth preserving if you
edit it:

- The `<form>` has a real `action` and `method="POST"`. With JavaScript off, or
  if unpkg is unreachable, submitting still works — it just redirects to
  Formspree's own thank-you page.
- The `@formspree/ajax` script loaded at the bottom upgrades that: submissions
  stay on the page and the `data-fs-success` / `data-fs-error` elements fill in.

A hidden `_gotcha` field traps bots. It is positioned off-screen rather than
`display:none`, because some bots skip hidden fields — which would defeat it.

To point the form somewhere else, change the `action` URL **and** the `formId`
in the inline script. Both must match.

## Deploying

Pushing to `main` publishes via `.github/workflows/pages.yml`. Enable it once
in **Settings → Pages → Source → GitHub Actions**.

For a custom domain, add a `CNAME` file containing the domain and set the same
value in Settings → Pages. Then update the absolute URLs in `index.html`
(`og:url`, `canonical`, the JSON-LD `url`) and in `sitemap.xml` and
`robots.txt` — those must be absolute and must point at the real host to be
useful.
