# دُردِ جام — Homepage (GitHub Pages)

This directory contains the static homepage for دُردِ جام, served at
**https://durde-jam.github.io/** via GitHub Pages.

## Structure

```
site/
├── index.html       ← Homepage (RTL, Persian, responsive)
├── issues.json      ← Issue manifest (auto-loaded by index.html)
└── issues/          ← Published editions (HTML + PDF)
    ├── issue_13.html
    ├── issue_13.pdf
    └── issue_13-mobile.pdf
```

## How the homepage works

1. `index.html` is a single static HTML file with inline CSS and JS.
2. On page load, the JS fetches `/issues.json` and renders:
   - The **latest edition** prominently (with "خواندن مجله", "PDF چاپی", "PDF موبایل" buttons)
   - An **archive grid** of all previous editions
3. No server-side rendering, no build step, no framework.

## Publishing a new edition

1. **Add the edition files** to `site/issues/`:
   ```
   site/issues/issue_14.html
   site/issues/issue_14.pdf
   site/issues/issue_14-mobile.pdf
   ```

2. **Add an entry to `site/issues.json`** at the TOP of the `issues` array:
   ```json
   {
     "number": 14,
     "date_jalali": "۱۴۰۵/۰۸/۱۶",
     "date_gregorian": "2026-11-07",
     "title": "عنوان گزارش جلد این شماره",
     "subtitle": "شماره ۱۴ — ۱۶ آبان ۱۴۰۵",
     "url": "/issues/issue_14.html",
     "pdf_print": "/issues/issue_14.pdf",
     "pdf_mobile": "/issues/issue_14-mobile.pdf",
     "cover_summary": "خلاصه یک‌دو خطی از محتوای این شماره."
   }
   ```

3. **Commit and push to `main`**. The GitHub Actions workflow
   (`.github/workflows/pages.yml`) auto-deploys. The homepage updates
   within ~1 minute — no rebuild needed.

## GitHub Pages setup

The repository must be configured to serve from **GitHub Actions**
(not from `docs/` or `main` root):

1. Go to **Settings → Pages**
2. Under **Build and deployment → Source**, select **GitHub Actions**
3. The `pages.yml` workflow handles the rest

## Visual identity

- **Surface**: `#f4eddf` (warm cream — matches the magazine PDF)
- **Primary text/links**: `#2d5016` (deep green — matches the magazine theme)
- **Accent**: `#b8860b` (dark goldenrod — matches the magazine accent)
- **Font stack**: Vazirmatn → IRANYekan → Tahoma (all ship with the
  magazine's HTML output; the homepage relies on system fonts for speed)

## Responsive behavior

- Desktop (>640px): single-column latest + multi-column archive grid
- Mobile (≤640px): single-column everything, larger touch targets,
  smaller masthead

## Limitations

- The homepage does NOT embed edition content — it links to the
  self-contained HTML edition. This keeps the homepage fast (~5KB)
  and avoids duplicating the magazine's rendering logic.
- `issues.json` is maintained manually. A future improvement could
  auto-generate it from the pipeline's `final_output.json` metadata.
- Search and filtering are not implemented. For a small archive
  (<50 issues), the grid is sufficient.
