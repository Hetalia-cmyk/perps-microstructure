# Perps DEX Market Microstructure — Top 20 by Open Interest

A single-page, primary-source reference on how the twenty largest perpetuals DEXs
actually match orders: matching engine type, order-priority rules, settlement
architecture, execution latency, speedbumps, and self-trade prevention.

**Live site:** https://USERNAME.github.io/REPO/ *(fill in after the first deploy)*

## What this is

Ranking snapshot taken **30 July 2026** from DefiLlama `/open-interest`,
perps-filtered, prediction markets excluded. Top-20 *membership* is robust;
exact ordering within ranks 7–20 reorders between intraday snapshots.

Every load-bearing claim is drawn from primary protocol documentation where one
exists. Inference, estimation, and vendor marketing are colour-coded as such
rather than presented as fact:

| Marker | Meaning |
| --- | --- |
| green | Stated in the venue's own technical documentation |
| amber | Estimated or inferred from indirect evidence |
| red | Vendor marketing claim, not independently verified |
| grey | Not documented anywhere findable |

Flagged spans (`.flag`) mark places where the documentation is silent, self-
contradictory, or has been quietly removed.

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

Requires Pages source set to **GitHub Actions** (Settings → Pages → Build and
deployment → Source). The workflow then runs on every push to `main`.

The `__SITE_URL__` placeholder in `index.html` is rewritten at build time with
the real Pages origin, so the canonical link and social-card tags stay correct
without hand-editing — including if the repo is renamed or moved to a custom
domain.

### Custom domain

Add a `CNAME` file containing the bare hostname (e.g. `perps.example.com`),
point a DNS `CNAME` record at `USERNAME.github.io`, then set the domain under
Settings → Pages. `base_url` picks it up automatically on the next build.

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
