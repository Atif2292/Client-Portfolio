# Client Portfolio

A single-file UGC creator portfolio site (`index.html`) — no build step, no dependencies, just open it in a browser or deploy it as static hosting. All styling is inline `<style>`, all behavior is inline `<script>`.

> `ugc-portfolio-template.html` is kept alongside `index.html` as the original working copy — `index.html` is the deploy entry point (Vercel and most static hosts serve `index.html` at the root by default).

## Fonts

Loaded from Google Fonts (one `<link>` in `<head>`):

| Font | Weights | Used for |
|---|---|---|
| **Anton** | 400 | All headings (`h1`, `h2`, `h3`), the "BV" logo mark, section titles, tier/card titles — the site's bold display voice |
| **Space Grotesk** | 500, 600, 700 | Fallback heading font; hero tagline (`.hero-tagline`); nav links, buttons, tier list markers, brand-marquee text |
| **Inter** | 400, 500, 600, 700 | Default body copy (`body`), paragraph text, final-CTA email link |
| **Space Mono** | 400, 700 | All uppercase "eyebrow" labels, badges, dates, prices/durations, footer copyright — the monospace "meta" voice used throughout for small caps text |

Font stack fallback order: `Anton` → `Space Grotesk` → `sans-serif` for headings; `Inter` → `sans-serif` for body; `Space Mono` → `monospace` for labels.

## Color system

Defined once as CSS custom properties on `:root`, then reused everywhere — change a value here and it cascades through the whole page.

| Variable | Value | Role |
|---|---|---|
| `--ink` | `#14150E` | Primary text / dark surfaces (nav CTA, footer-strip backgrounds) |
| `--ink-soft` | `#55583F` | Secondary/muted text |
| `--paper` | `#FAF8EC` | Page background (warm cream) |
| `--paper-dim` | `#F1EDD6` | Slightly deeper cream, used for subtle fills |
| `--card` / `--card-hi` | `#FFFFFF` / `#FFFEF6` | Card backgrounds |
| `--signal` | `#C7D633` | Brand accent (lime) — buttons, active states, borders, glow effects |
| `--signal-soft` | `#E3EC8E` | Lighter accent, used in gradients and shine sweeps |
| `--signal-dim` | `rgba(199,214,51,0.22)` | Accent tint for badge backgrounds |
| `--signal-text` | `#6B7A1A` | Darker accent variant used specifically where accent color sits on text (contrast-safe) |
| `--indigo` | `#E0692C` | Secondary accent (terracotta/orange) — category tags |
| `--mid` | `#83816A` | Tertiary muted text (labels, dates) |
| `--line` | `rgba(20,21,14,0.10)` | Hairline borders/dividers |
| `--radius` | `18px` | Default card corner radius |
| `--maxw` | `1120px` | Default content max-width (`.wrap`); `.wrap-wide` overrides to `1440px` for the portfolio section |

## Layout structure

Sections in page order, each a direct child of `<body>`:

1. **`.scroll-progress`** — fixed top bar that fills as the page scrolls
2. **`.nav`** — sticky pill nav: "BV" logo mark, centered links (Work/Services/Reviews), "DM on Instagram" + "Book a call" CTAs
3. **`.hero`** (`<header>`) — eyebrow, big name (`.hero-name`), tagline, bio paragraph, CTA buttons, phone-mockup photo with animated halo
4. **`.stats`** — 4-stat strip (videos shipped, DTC brands, CTR lift, turnaround)
5. **`.trusted-by`** — animated dark gradient strip with a scrolling brand-name marquee
6. **`#niches`** — category-tabbed portfolio grid (5 tabs, each with a "featured" always-playing video card + 4 hover-to-play cards)
7. **`#reviews`** — two-row auto-scrolling review marquee (opposite directions)
8. **`#services`** — 4 pricing tier cards (grid, auto-wraps)
9. **`#process`** — numbered 3-step list
10. **`.final`** (`#contact`) — closing CTA, email link, Instagram link
11. **`footer`** — social links

## Key CSS patterns

- **Marquees** (`.marquee-track`, `.reel-track`, brand ticker): two identical content groups side by side, animated with `translateX(0 → -50%)` on an infinite loop — this is what makes the scroll seamless (any mismatch between the two groups' widths would cause a visible jump).
- **`.reveal` / `.hero-entrance`**: opacity+transform scroll-reveal classes toggled by an `IntersectionObserver` in the closing `<script>`; `--i` custom properties (set via JS on load) drive staggered `transition-delay` per item.
- **`.shine`**: animated gradient-text effect (`background-clip: text` + `background-position` keyframe) used on key accent words/headings.
- **`hover: hover` media guard**: all hover-triggered pop/play effects on portfolio cards are wrapped in `@media (hover: hover) and (pointer: fine)` so touch devices don't get stuck in a "hovered" state after a tap.

## Assets

- `assets/hero-creator.jpg` — hero portrait, referenced via relative path. Keep this folder alongside the HTML file wherever it's deployed.
- Portfolio videos are **not included in this repo** (see below).

## Video hosting (action needed)

The portfolio section (`#niches`) references local video files (`health.MP4`, `fintech.MP4`, etc.) that are **not committed to this repo** — several exceed GitHub's 100MB file limit, and the full set is ~1.1GB. Before this is fully live:

1. Upload the videos to an external host (Cloudinary, Bunny.net, Mux, Vercel Blob, etc.)
2. Replace each `<source src="...">` in the `.niche-panel` sections with the hosted URL

Until then, those video cards will 404 in any deployed environment.
