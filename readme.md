# Hexes on Automatic — public site

Static site for [Hexes on Automatic](https://hexesonautomatic.com), the
overworld sibling of [Dungeons on Automatic](https://dungeonsonautomatic.com).
Same design system as the DOA site (Cinzel / Spectral / IBM Plex Mono, dark
workshop chrome, brass), with a canopy-green primary in place of DOA's ember red.

## Layout

- `index.html` — product homepage (hero screenshot is the real app at seed 443127)
- `quickstart.html` — install + first region walkthrough
- `vault.html` — Campaign Vault interop (what Hexes writes, commissions to DOA)
- `releases.html` — downloads, known issues, changelog
- `privacy.html`, `404.html`
- `styles.css` — design-system entry point (`tokens/` + `components/`)
- `site.css` — page layout
- `data/` — committed snapshots of the hexes release metadata

No build step. Every page is plain HTML + CSS with a little inline JS.

## Release data

`data/downloads.json`, `data/releases.json`, `data/changelog.json` are snapshots
of the same files the DOA site repo mirrors into `data/hexes/` from the
`hexes-continuous` release. The releases page renders the snapshot first, then
refreshes URL / size / date live from the public GitHub release API, so the
download cards stay current even when the snapshots lag. To refresh the
snapshots, copy them from `DungeonsOnAutomaticSite/data/hexes/` (or extend that
repo's mirror workflow to push here).

## Deploy

`.github/workflows/deploy.yml` deploys to Cloudflare Pages on push to `main`
(`wrangler pages deploy . --project-name hexesonautomatic`). It needs:

- repo secret `CLOUDFLARE_API_TOKEN` (same token the DOA site uses)
- repo variable `CLOUDFLARE_ACCOUNT_ID`
- a Pages project named `hexesonautomatic` (direct upload) in the account

The public home is `https://hexesonautomatic.com/` — the zone is already in
the same Cloudflare account. Wire it like the DOA cutover: add apex + `www`
as custom domains on the Pages project (proxied CNAME records to
`hexesonautomatic.pages.dev`). Canonical URLs, `robots.txt` and `sitemap.xml`
all assume that origin; if the host ever changes, swap it in the
`<link rel="canonical">` / `og:` tags of the five pages plus `robots.txt`
and `sitemap.xml`.

## Bugs

User-facing issues go to the family's public tracker:
<https://github.com/Zuljita/DungeonsOnAutomaticSite/issues> (say it's Hexes).

Hexes on Automatic is an original, unofficial, non-resale fan game aid by Kyle
Norton for GURPS and the Dungeon Fantasy Roleplaying Game, used per the
SJ Games Online Policy. Not affiliated with or endorsed by Steve Jackson Games.
