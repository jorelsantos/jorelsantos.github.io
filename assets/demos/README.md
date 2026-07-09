# assets/demos

Screenshots and short demo clips for the portfolio's project entries. Each project has its own folder;
media renders **inside the expanded `<details>` detail** of that project in `index.html`, after the
Results line.

## Folder model

```
assets/demos/<slug>/
  raw/   # source recordings + screenshots — gitignored, kept local
  web/   # optimized, reviewed copies — committed and referenced by index.html
```

Slugs: `sosbio` · `vinland` · `rabbit-hole` · `cai`. Drop full-quality source captures in `raw/`
(never committed), optimize them into `web/` (committed). The `.gitkeep` files hold the empty structure
in git; the `.gitignore` rule `assets/demos/*/raw/*` (with a `!…/.gitkeep` negation) keeps raw source out.

## Before you export anything — scrub checklist

The repo is **public**, and some apps show sensitive material. Review every capture for:

- **API tokens, keys, internal URLs, session cookies** visible in the UI or address bar.
- **SOS:BIO:** real researcher/dataset names or lab-internal data if flagged sensitive (live CASL/ISR
  project under Prof. Hemphill, tied to the NSF RDE grant). Confirm what's clear to show first.
- **Vinland:** personal health data — RPE notes, injuries, body metrics, anything you'd rather not publish.
- Anything else you wouldn't put on a billboard.

When in doubt, crop it out or blur it before it leaves `raw/`.

## Screenshot specs

- **Format:** WebP, quality ~90 for UI text. Use PNG only if a specific screenshot shows
  text fringing/artifacts in WebP.
- **Phone apps (Vinland):** capture **viewport only** at ~390×844 @ deviceScaleFactor 2–3
  (≈1170×2532). Keep **portrait** — do **not** letterbox into landscape 16:10 (that shrinks
  the UI to a blurry stamp). Portfolio CSS uses `max-width: ~17.5rem` phone frames.
- **Wide desktop UIs (future):** ~1600px wide, optional **16:10** crop.
- **File size:** phone stills often ~70–200KB WebP; keep under ~400KB each.
- **Treatment:** prefer dark app views that sit inside Flexoki Dark; CSS adds border + shadow.
  Capture from a **local synthetic demo pack** when live data undercuts the story (empty week,
  private notes). Never fudge production vault/Render for screenshots.

Optimize with any tool you like. Example with `cwebp`:

```bash
cwebp -q 82 raw/01-hero.png -o web/01-hero.webp
```

## Video specs

Short, **muted, auto-looping "motion teasers"** — not narrated trailers. Motion should tell a story a
still can't (a graph traversal, a natural-language log parsing into structured sets).

- **Length:** 10–20s (muted loops read fine short).
- **Resolution:** ≤1280px wide.
- **Aspect ratio:** 16:9.
- **Format:** ship **both** `demo.webm` (VP9, modern browsers) and `demo.mp4` (H.264, fallback), plus a
  `demo-poster.webp` still frame.
- **File size target:** **< 3MB** per clip. GitHub Pages caps files at 100MB and has bandwidth limits —
  but the real budget is perceived speed, so keep clips tiny.

Install ffmpeg once: `brew install ffmpeg`. Then, from a source recording `raw/demo.mov`:

```bash
# MP4 (H.264) fallback — muted, capped at 1280px wide
ffmpeg -i raw/demo.mov -an -vf "scale='min(1280,iw)':-2" -c:v libx264 -crf 28 -preset slow -movflags +faststart web/demo.mp4

# WebM (VP9) — muted
ffmpeg -i raw/demo.mov -an -vf "scale='min(1280,iw)':-2" -c:v libvpx-vp9 -crf 33 -b:v 0 web/demo.webm

# Poster frame (grab ~1s in), then compress to WebP
ffmpeg -i raw/demo.mov -ss 00:00:01 -vframes 1 web/demo-poster.png
cwebp -q 82 web/demo-poster.png -o web/demo-poster.webp && rm web/demo-poster.png
```

Tune `-crf` up to shrink further; check the result is still under ~3MB and looks clean.

## Naming convention

- Screenshots: `NN-short-slug.webp` (e.g., `01-citation-ranking.webp`, `02-graph-view.webp`).
- Video set: `demo.mp4`, `demo.webm`, `demo-poster.webp`. (Add a `-2` suffix if a project needs a second
  clip: `demo-2.mp4`, etc.)

## Publish workflow

1. Capture into `raw/` (gitignored). Scrub per the checklist above.
2. Optimize into `web/` using the specs above.
3. In `index.html`, find that project's commented `DEMO MEDIA` block, **uncomment it**, and fill in the
   real `alt` text and `<figcaption>` (remove the `TODO`s). Delete any media lines you're not using.
4. Push to `main` — GitHub Pages redeploys in ~1 minute.

The markup is already wired and pointed at these paths for all four projects — it just ships commented out
so the live site never shows a broken image before an asset exists. Adding an asset is: drop the file,
uncomment, push.

## Status

- **Vinland (2026-07-09 v2):** three **portrait** phone stills from local synthetic
  hybrid demo pack (`sample_data/portfolio_demo/` in the Vinland repo). Viewport
  @3×, no landscape letterbox. Video / HyperFrames teaser deferred.

## Future slot

A narrated 30-second trailer hosted on YouTube/Vimeo is a possible later addition. It's not scaffolded
here (the self-hosted muted-loop path is), but the `<figure class="demo demo--video">` block could be
swapped for an embed if you decide a narrated trailer is worth it for a flagship project.

**HyperFrames later:** free HTML→MP4 agent path for a muted 12–18s Vinland teaser;
do not block stills on that work.
