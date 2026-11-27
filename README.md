# VIG Dashboard

Static dashboard served by GitHub Pages from the root of `main`. The research vault stays in a separate private repository.

- `index.html`: the page (no build step).
- `data/vig.json`: all content. Update this file to update the dashboard.

## Public-safe rule

A GitHub Pages site is publicly reachable. `data/vig.json` must never contain
account names, holdings, position sizes, capital amounts, ceilings or balances.
Suggestions and research only.

## Updating the data

Edit `data/vig.json` and push. Conventions:

- Unknown or missing values are `null` and render as **Pending**. Never estimate.
- Dual Decision Engine scores: `o` / `r` are points earned (0–6), `o_unknown` /
  `r_unknown` count UNKNOWN categories. `complete` is true only when all six
  categories of both forces are assessed.
- `prices.snapshot` holds dated quotes from VIG research. Live quotes come from
  TradingView widgets via `prices.symbols`.
- Bump `meta.as_of` on every update.
