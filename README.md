# paperback — Phnom Penh TOP page prototypes

## Live Preview

https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/

Direct links:
- V1 (editorial magazine layout): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v1.html
- V2 TOP page (scroll-driven interaction): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v2/index.html
- V2 article page (Field Note 01): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v2/article.html

## Repository

https://github.com/hrkfreelance-droid/paperback-phnom-penh-preview

## What this is

Design prototypes for the TOP page of **paperback**, a curated city guide to Phnom Penh. Not a
production build — a design/interaction exploration comparing two directions:

- **V1** — a static editorial magazine layout (cover → editor's note → feature → contents-style index).
- **V2** — a scroll-driven experience: a hero where giant type is literally made of a photograph of the
  city (`background-clip: text`) that parts on scroll to reveal the photo behind it, and a walking
  "Index" where the place nearest the reading line comes into focus while its photo appears in a
  fixed frame — reinterpreting "discovering a city on foot" as a scroll interaction. V2 also includes
  one article page (`v2/article.html`), reached from the Field Note teaser at the top of the V2 index,
  demonstrating how an individual story would read inside the same world.

All photography is placeholder/mood imagery from [Unsplash](https://unsplash.com) — see
[`v2/assets/images/CREDITS.md`](v2/assets/images/CREDITS.md) for the source of every image and how to
swap in real venue photography later.

## Tech

Static HTML/CSS/vanilla JS. No build step, no framework, no dependencies. Fonts: Helvetica Neue
(system) + Libre Baskerville (Google Fonts).

## Run locally

```bash
git clone https://github.com/hrkfreelance-droid/paperback-phnom-penh-preview.git
cd paperback-phnom-penh-preview
python3 -m http.server 8000
# open http://localhost:8000
```
