# paperback — Phnom Penh TOP page prototypes

## Live Preview

https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/

Direct links:
- V1 (editorial magazine layout): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v1.html
- V2 TOP page (scroll-driven interaction): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v2/index.html
- V4 TOP page (V2 with the motion reworked): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v4/index.html
- V2 article — "First Films at Meta House" (Anti-Archive Short Film Night): https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v2/articles/anti-archive-short-film-night.html
- V2 article — "The Riverside, Before the Heat Arrives": https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v2/articles/the-riverside-before-the-heat-arrives.html

## Repository

https://github.com/hrkfreelance-droid/paperback-phnom-penh-preview

## What this is

Design prototypes for the TOP page of **paperback**, a curated city guide to Phnom Penh. Not a
production build — a design/interaction exploration comparing two directions:

- **V1** — a static editorial magazine layout (cover → editor's note → feature → contents-style index).
- **V2** — a scroll-driven experience: a hero where giant type is literally made of a photograph of the
  city (`background-clip: text`) that parts on scroll to reveal the photo behind it, and a walking
  "Index" where the place nearest the reading line comes into focus while its photo appears in a
  fixed frame — reinterpreting "discovering a city on foot" as a scroll interaction.

  V2's TOP page features one article at a time (`v2/articles.json` holds the list; the TOP page fetches
  it and renders the first entry as the "Field Note" teaser, so a new article can be added without
  touching `index.html`). Two articles exist:
  - **First Films at Meta House** (`v2/articles/anti-archive-short-film-night.html`) — an editorial piece
    on Anti-Archive's Short Film Night at Meta House (20 Aug 2026), researched from public sources.
    No film stills or posters are used — the copyright status of official production/festival imagery
    couldn't be confirmed for redistribution, so the page is typography-led, with a printed-ticket-stub
    motif standing in for photography. See the in-article Sources list for what was consulted.
  - **The Riverside, Before the Heat Arrives** (`v2/articles/the-riverside-before-the-heat-arrives.html`)
    — a mood piece about Riverside, illustrated with placeholder Unsplash photography.

All non-editorial photography (the TOP index, the Riverside piece) is placeholder/mood imagery from
[Unsplash](https://unsplash.com) — see [`v2/assets/images/CREDITS.md`](v2/assets/images/CREDITS.md) for
the source of every image and how to swap in real photography later.

## Tech

Static HTML/CSS/vanilla JS. No build step, no framework, no dependencies. Fonts: Helvetica Neue
(system) + Libre Baskerville (Google Fonts). Article metadata lives in `v2/articles.json`; article prose
lives in its own HTML file under `v2/articles/`.

## Run locally

```bash
git clone https://github.com/hrkfreelance-droid/paperback-phnom-penh-preview.git
cd paperback-phnom-penh-preview
python3 -m http.server 8000
# open http://localhost:8000
```
