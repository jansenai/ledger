# jansen.ai — Verifiable Prediction Ledger
 
This repository is the public, tamper-evident track record for [jansen.ai](https://jansen.ai), a quantitative signal service covering crypto, US equities/indices, and metals.
 
Every prediction we publish is committed here **before** the period it covers closes, together with a cryptographic proof that anchors its existence to the Bitcoin blockchain. Nobody — including us — can quietly edit a prediction after the fact without it being detectable.
 
## Why this exists
 
Retail trading-signal services are notorious for unverifiable claims: cherry-picked screenshots, backtests with no out-of-sample discipline, and win rates nobody can audit. This repo is our answer to that. Every prediction is:
 
- **Public** — anyone can clone this repo and read every prediction we've ever made.
- **Timestamped** — each file has a matching [OpenTimestamps](https://opentimestamps.org) proof (`.ots`) showing it existed at or before a specific point in time.
- **Immutable** — files are never edited or deleted after being committed. Corrections, if needed, are posted as new files, not silent rewrites.
You don't have to trust jansen.ai to trust this ledger. You only have to trust the Bitcoin blockchain, which is independently verifiable by anyone.
 
## How it works
 
1. jansen.ai generates a forecast for a given period (daily / weekly / monthly).
2. The prediction is written to a plain-text file and committed to this repo.
3. `ots stamp` hashes that file and submits the hash to OpenTimestamps calendar servers, producing a `.ots` proof file, which is committed alongside it.
4. Once the Bitcoin transaction anchoring that hash confirms (usually within a few hours), the `.ots` proof is upgraded and re-committed with the completed on-chain path.
5. When the period closes, the actual outcome is committed the same way, in its own file.
At no point does the original prediction file change — only new files are ever added.
 
## Repository structure
 
Files are named `YYYY-MM-DD-{frequency}.txt` for predictions and `YYYY-MM-DD-{frequency}-actuals.txt` once outcomes are known, each with a matching `.ots` proof:
 
```
2026-07-13-weekly.txt
2026-07-13-weekly.txt.ots
2026-07-13-weekly-actuals.txt
2026-07-13-weekly-actuals.txt.ots
2026-07-16-daily.txt
2026-07-16-daily.txt.ots
...
```
 
`frequency` is one of `daily`, `weekly`, or `monthly`. The date in the filename is the date the *prediction period begins* (e.g. `2026-07-13-weekly.txt` covers the week of July 13–19, 2026).
 
Each prediction file lists, per instrument, the model/method used and the predicted value (and forecast bands, where published).
 
## Verifying a proof yourself
 
You don't need to take our word for anything in this repo. Anyone can independently check that a given prediction existed before a given date:
 
1. Install the OpenTimestamps client:
```bash
   pipx install opentimestamps-client
```
2. Clone this repo (or download a single prediction file plus its `.ots` file):
```bash
   git clone https://github.com/jansenai/ledger.git
   cd ledger
```
3. Verify:
```bash
   ots verify 2026-07-13-weekly.txt.ots
```
   A confirmed proof returns something like:
```
   Success! Bitcoin block 912345 attests existence as of 2026-07-13 UTC
```
   If the file has been altered in any way since it was stamped — even by a single byte — verification will fail. That's the entire point.
 
## Rules we hold ourselves to
 
- **Nothing is ever edited or deleted.** If a mistake is found in a published prediction, we add a new file noting the correction; the original stays exactly as committed.
- **Predictions are always committed before the period they cover ends.** A "prediction" posted after the fact isn't one.
- **Actuals are posted separately**, after the period closes, sourced from public market data (exchange data for crypto, Yahoo Finance for equities/indices/metals).
## Methodology (summary)
 
Forecasts are produced by an ensemble of optimization methods (PSO, GA, DE, SA, ACO, CMA-ES, Nelder-Mead, Bayesian Optimization) feeding prediction models (ARIMA, XGBoost, LightGBM, quantile regression, Theta, Kalman filtering, Random Forest, Elastic Net, and an ensemble blend), with GARCH-based volatility bands and HMM-based market regime detection layered on top. A full breakdown of the methodology is available at [jansen.ai](https://jansen.ai).
 
This repo is the proof layer. [jansen.ai/ledger](https://jansen.ai/ledger) is the human-readable presentation of the same data — a running scorecard of predictions vs. actuals.
 
## Questions
 
Reach out via [@jansen_ai](https://x.com/jansen_ai) on X, or through [jansen.ai](https://jansen.ai).
 
---
 
*Jansen Applied Intelligence Pte Ltd, Singapore.*
