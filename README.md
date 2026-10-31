# PJM Peak Alerts: public track record

By Connor Slattery

Large electricity users in PJM pay capacity charges based on their load during the
5 highest PJM-wide days of the summer, about $122,000 per MW per year at the 2027/28
price. Cutting load on exactly those days saves most of it. Cutting on the wrong days
costs money too. This service calls those days a day ahead.

This repo is the public, timestamped record of every call. The model itself is private.

## Live scorecard

No live season scored yet. The first live season starts Dec 1, 2026.

Updated 2026-10-31. Details: [`scorecard.json`](scorecard.json).

## How to check the record

1. **Every day's calls are sealed before the outcome.** The SHA-256 of that day's calls
   is added to [`commitments.csv`](commitments.csv) the same day, in a dated commit.
2. **A week later the calls are revealed** in [`calls/`](calls/), once every day in them
   has passed. Hash the file and compare it with the commitment:
   `sha256sum calls/calls_2026-12-01.json` (or `Get-FileHash` on Windows).
3. **Scored calls** are listed in [`track_record.csv`](track_record.csv).
4. **The model is frozen** for the first live season (Dec 1, 2026 to Feb 28, 2027).
   The fingerprints of the model files are in [`FROZEN.md`](FROZEN.md). They don't change
   until the season ends.

## Backtest (walk-forward, 7 seasons, 2023-2026)

Each season was predicted using only data available by 11:45 am ET the day before.

| Test | Peaks caught | Alert days |
|---|---|---|
| PJM-wide peak days, this service | **35 / 35** | **76** |
| Standard threshold rule, same seasons | 34 / 35 | 127 |
| Live code replayed day by day, 5 seasons (leak check) | 25 / 25 | 57 |
| ComEd transmission peak (1CP), 4 summers | 4 / 4 | 31 (rule: 53) |

- Our top-5 days match PJM's officially published peak days in every season PJM has published.
- The 3-hour cut window held the true peak hour on 97-100% of summer peak days.
- Tables: [`backtest/`](backtest/).

## What each call contains

- **Tomorrow:** ALERT or NO ALERT, with the chance it's one of the season's 5 peak days
- **Next 2-6 days:** WATCH flags for planning
- Summer calls also carry a 3-hour cut window and a dollar value per MW (shared with customers only)

## Data

Built on public data: PJM Data Miner 2 plus public weather and market data. PJM's terms
don't allow non-members to republish raw PJM data, so this repo only holds derived calls,
chances and outcomes.

Forecasts, not guarantees.
