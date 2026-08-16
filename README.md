# botcommits.dev

Static site for [botcommits.dev](https://botcommits.dev): monthly counts of
AI-attributed commits on public GitHub, by tool, with the measuring instrument
stated. Published via **GitHub Pages** at the apex domain (see `CNAME`).
Fully pre-rendered — no build step here, no runtime API, no env vars.

The page is **generated** in the analysis repo and copied here:

```
github_explosion/analysis/search_collector.py   # GitHub Search API collector (Nov 2025+)
github_explosion/analysis/fit_models.py         # exponential / logistic / Gompertz fits + hindcast
github_explosion/analysis/build_dashboard.py    # renders dashboard/index.html from index.template.html
```

Monthly update = run those three, then `cp` the outputs listed below into this
repo and push. Do not hand-edit `index.html`.

## Contents

| File | Purpose |
|------|---------|
| `index.html` | The single-page dashboard. Data is embedded (`const DATA = …`). Loads Chart.js from `cdn.jsdelivr.net`. |
| `data.json` | The same data blob, machine-readable (linked from the page and its Dataset schema). |
| `og-image.png` / `og-card.html` | 1200×630 social preview and its source (render with headless Chrome at 1200×630). |
| `favicon.png`, `robots.txt`, `sitemap.xml`, `CNAME` | As usual. |
| `Dockerfile.cloudrun-archive` | The nginx Dockerfile from the old Cloud Run deployment, kept for provenance. Not used. |

## History

- **2026-03-08 / 04-06** — original dashboard (Cloud Run). Data: BigQuery Jan–Oct 2025, hand-collected Search API counts after; exponential best fit, "no inflection observed".
- **2026-04-17** — this repo scaffolded for the Pages migration; migration left half-done (Cloud Run deleted before DNS/Pages were switched, site 404 for ~4 months).
- **2026-08-16** — rebuilt: repeatable windowed Search-API collector, Oct 2025 filled, Copilot coding agent added, developer count dropped (unmeasurable since GH Archive removed commit payloads), commit/push-event denominator explained, models refit with hindcast — the exponential bent around April 2026.

## What's NOT here

No secrets, no CI, no tests, no package manifest. The point of Pages is zero
operational surface. Everything with logic lives in `github_explosion`.
