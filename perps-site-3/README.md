# Perps DEX Market Microstructure — Top 20 by Open Interest

A single-page, primary-source reference on how the twenty largest perpetuals DEXs
actually match orders: matching engine type, order-priority rules, settlement
architecture, execution latency, speedbumps, and self-trade prevention.

**Live site:** https://hetalia-cmyk.github.io/perps-microstructure/

## What this is

Ranking snapshot taken **30 July 2026** from DefiLlama `/open-interest`,
perps-filtered, prediction markets excluded. Top-20 *membership* is robust;
exact ordering within ranks 7–20 reorders between intraday snapshots.

Every load-bearing claim is drawn from primary protocol documentation where one
exists. Inference, estimation, and vendor marketing are colour-coded as such
rather than presented as fact:

| Marker | Colour | Meaning |
| --- | --- | --- |
| `DOC` | `#5FAE8C` | Stated in the venue's own technical documentation |
| `EST` | `#C89B5A` | Estimated or inferred from indirect evidence |
| `MKTG` | `#E0705F` | Vendor marketing claim, not independently verified |
| `N/D` | `#8B9A95` | Not documented anywhere findable |

Every badge carries a text label as well as a colour, so the distinction
survives greyscale printing and red-green colour deficiency. Flagged spans
(`.flag`) mark places where the documentation is silent, self-contradictory or
has been quietly removed; they are underlined as well as coloured for the same
reason.

## Visual design

Styled to the **Fermi UI** language: three forest-green surface levels
(`#011712` canvas, `#08201A` panel, `#0D2922` raised, `#01120E` inset), square
geometry with radii of only `0` or `2px`, a display serif for the title, compact
sans for UI, and monospace tabular numerals for all market data. Hierarchy comes
from surface contrast rather than from borders; hovering a row lifts it exactly
one surface level.

Fermi yellow `#F3C544` is reserved for interaction — the active filter, the sort
direction — and deliberately never encodes data, so it keeps meaning "act here".

The eight meaning-carrying hues are split into two tiers by chroma: the evidence
badges above (higher chroma, with the rare `MKTG` warning loudest) and the four
engine categories (lower chroma, since they group rather than alert). All were
checked for WCAG contrast on every surface level and for CIE ΔE separation
within each axis — every pair is at least 20 ΔE76 apart, so no two categories
can visually merge.

### Two taxonomies, deliberately

The `cls` field drives the filter buttons and is coarse: it buckets by
*dominant* mechanism, so CLOB = 14. The `engT` field drives the coloured chip
and is precise: it separates out venues that are order books with a significant
non-book component, so CLOB = 12 with 3 hybrids. The two disagree on exactly two
rows — **Aster** (multi-mode) and **Antarctic** (CLOB plus house vault). The
social card counts by `engT`. If the filter returning 14 for CLOB is confusing,
reconcile by moving those two rows to `cls:"Other"`.

## Layout

```
index.html                     the whole site — self-contained, no external requests
og.png                         1200×630 social card
.nojekyll                      serve files verbatim, skip Jekyll processing
.github/workflows/pages.yml    build + deploy on push to main
```

`index.html` has **zero external dependencies** — no CDN, no fonts, no
analytics, no JavaScript fetches. It renders identically offline, which is the
point for a reference document: nothing upstream can change what it says or
watch who reads it.

## Deploying

Works under **either** Pages source:

- **Deploy from a branch** (simplest — no Actions run at all). The committed
  files are served verbatim. The workflow file is then inert and can be deleted.
- **GitHub Actions.** The workflow runs on every push to `main` and additionally
  reconciles the absolute URLs if the Pages origin ever changes.

The canonical link and the Open Graph / Twitter card tags are **baked into
`index.html` as literal absolute URLs**. This is deliberate: social scrapers read
raw HTML, never execute JavaScript, and don't retry. If those tags depend on a
build step and the build step doesn't run — which is exactly what happens under
branch-source deploys — the page still renders perfectly while every social
preview silently breaks, with no symptom visible from the page itself.

### Custom domain

Add a `CNAME` file containing the bare hostname (e.g. `perps.example.com`), point
a DNS `CNAME` record at `hetalia-cmyk.github.io`, then set the domain under
Settings → Pages. Under the Actions source the workflow rewrites the baked-in
URLs to match automatically; under branch source, update the four absolute URLs
in `index.html` by hand (`grep -n 'hetalia-cmyk.github.io' index.html`).

### After changing any social tag

Scrapers cache aggressively. Force a re-fetch:

- LinkedIn — https://www.linkedin.com/post-inspector/
- Facebook — https://developers.facebook.com/tools/debug/
- X — no validator since 2023; caches ~7 days. Post with a throwaway query
  string (`?v=2`) to present a URL it hasn't seen.
- Slack — unfurls follow the same cache; `?v=2` works there too.

## Updating

The page is a dated snapshot by design. To refresh it, re-pull the DefiLlama
ranking and check top-20 membership first — if a venue has entered or left, its
row needs re-verification against that venue's own docs, not just a number swap.

## Caveats worth keeping

- Ranks 7–20 are not stable ordering; treat the band as a set, not a sequence.
- Several venues publish no engineering spec at all. Those rows are marked grey
  rather than guessed at.
- One venue in the table (Ostium) suffered a $23.75 M oracle-forgery exploit on
  15 July 2026 that its own blog, media page and docs do not mention.
