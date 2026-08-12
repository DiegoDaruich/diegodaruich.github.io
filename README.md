# diegodaruich.github.io

Personal academic website. Plain HTML + CSS in a single file — no build step, no dependencies.

## Files

- `index.html` — the whole site (styles are in the `<style>` block at the top).
- `CNAME` — tells GitHub Pages to serve the site at `www.diegodaruich.com`.
- `images/profile.png`, `images/universities.png` — your photo and the university logo strip.

## One-time setup

### 1. Turn on GitHub Pages

Repo → **Settings → Pages** → Source: `Deploy from a branch`, Branch: `main`, folder: `/ (root)`.
Under "Custom domain" enter `www.diegodaruich.com` and save. Tick **Enforce HTTPS** once it becomes available (can take an hour).

### 2. Point the domain at GitHub

Wherever `diegodaruich.com` is registered (check the Weebly/Squarespace Domains panel), set:

| Type  | Host  | Value                    |
| ----- | ----- | ------------------------ |
| CNAME | `www` | `diegodaruich.github.io` |
| A     | `@`   | `185.199.108.153`        |
| A     | `@`   | `185.199.109.153`        |
| A     | `@`   | `185.199.110.153`        |
| A     | `@`   | `185.199.111.153`        |

The four A records make the bare `diegodaruich.com` redirect to `www`. DNS changes take 10 minutes to a few hours.

Once the new site loads at your domain, cancel the Weebly plan but **keep the domain registration** (or transfer it to Cloudflare/Namecheap, ~$12/yr).

## Adding a paper

Open `index.html`, find the right section, copy an existing `<article>` block and edit it. The full pattern:

```html
<article class="entry">
  <h3 class="ptitle">Title of the Paper</h3>
  <p class="meta"><em>Journal Name</em>, 2027 · with <a href="https://coauthor.com/">Coauthor Name</a></p>
  <div class="links">
    <a href="https://..." class="lnk">Paper</a>
    <a href="https://..." class="lnk">Slides</a>
    <a href="https://..." class="lnk">BibTeX</a>
    <a href="https://..." class="lnk press">New York Times</a>
  </div>
  <details>
    <summary><span class="caret">›</span>Abstract</summary>
    <p class="abstract">Abstract text.</p>
  </details>
</article>
```

Notes:
- The **last** article in a section carries `class="entry last"` — that's what draws the closing rule. If you add a new paper at the bottom, move `last` down to it.
- `class="lnk press"` is the sage-green pill used for press coverage; plain `class="lnk"` is the outlined pill for papers, slides and BibTeX.
- Work-in-progress and discussion entries are the same block without the `links`/`details` parts.

## Editing the look

Everything is driven by the variables in `:root` at the top of the `<style>` block — background, text, accent colors, and the two fonts. Change a value there and it propagates through the page.

## Email

The address is not written anywhere in the HTML. It's assembled by the small script at the bottom of the file when someone clicks the link, so scrapers find nothing to harvest. If the address ever changes, edit that script.

## Google Analytics

The GA4 tag is in the `<head>` of `index.html`. **Replace both occurrences of `G-XXXXXXXXXX` with your Measurement ID** (GA admin → Data Streams → your web stream; it looks like `G-ABC123DEF4`).

Weebly injected its own tag, which is why tracking stopped when you moved.

### Custom events sent

| Event | When | Parameters |
| --- | --- | --- |
| `paper_link_click` | Any Paper / Slides / BibTeX / press link | `paper_title`, `link_type`, `link_url` |
| `cv_click` | The CV button (header or footer) | `link_url` |
| `abstract_open` | Someone expands an abstract | `paper_title` |

To see `paper_title` and `link_type` as columns in reports, register them once as custom dimensions: GA4 → **Admin → Custom definitions → Create custom dimension**, scope *Event*, event parameter `paper_title` (repeat for `link_type`). Data appears in reports ~24h later; **Realtime → Events** shows them immediately for testing.
