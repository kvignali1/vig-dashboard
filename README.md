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

## CEO decisions from the dashboard

Each new opportunity has **Start tracking / Keep watching / Pass** buttons. A
button opens a prefilled issue in the private vault repo
(`meta.decisions.repo`, label `ceo-decision`). Nothing is written until the
CEO presses "Create" on GitHub, and only people with vault access can do so.
No token is stored in this public site.

Each issue carries a machine-readable block:

    ```vig-decision
    decision: TRACK | WATCH | PASS
    ticker: CAH
    exchange: NYSE
    record: VIG-OD-20261127-001
    date: 2026-11-28
    source: dashboard
    ```

The daily **VIG New Opportunity Discovery** run (8 AM Pacific) applies them
first: it reads open `ceo-decision` issues authored by `kvignali1` only.

- **TRACK**: classified as `AWAITING ENTRY` (concrete entry conditions exist)
  or `MONITOR / MORE EVIDENCE REQUIRED`, recorded in a new Drive Case State,
  moved from `opportunities` to `cases` here.
- **WATCH**: status becomes `WATCHLIST (CEO <date>)`.
- **PASS**: removed from `opportunities`.

The run then comments on and closes each issue, and adds the day's new
candidates to `opportunities`.
