# SwingSense V2 — Swing Trading Research & Backtesting Platform

<video src="https://github.com/user-attachments/assets/021d0c25-4c9d-402a-9b28-47bd85acdacb" controls width="720" style="max-width: 100%; height: auto;"></video>

> **Note**  
> This is a **private project**. The repository intentionally contains **no source code**; this README documents the platform’s capabilities and artifacts for reviewers and collaborators.

---

## Tech Stack

| Layer        | Key Components |
| :--         | :-- |
| **Platform** | **Electron**, **Vite** (bundler) + **TypeScript** |
| **Frontend** | **React 18**, **lightweight-charts** (charting) |
| **State/Data** | **Zustand** (state), **SQLite** (embedded via `better-sqlite3`), **Zod** (validation) |
| **APIs/Utils** | `yahoo-finance2` (market data), `fflate` (ZIP export) |

---

## Overview

**SwingSense V2** is a modular **desktop research app** for multi-year swing-trading backtests with a plug-in strategy engine.  
It performs **next-bar** entries, supports single-ticker and experiment runs, and provides **one-click ZIP exports** (bars, indicators, trades, equity, config) for fully reproducible analysis.

---

## Key Capabilities

- **Modular Strategy Engine** — JSON-configurable rules, scoring, and guardrails to gate entries.  
- **Realistic Backtesting** — **5-year daily** backtests, next-bar entries, rich exits (target/stop, optional trailing & time-stop).  
- **Trade Analytics** — **R-multiple**, %PnL, **MAE/MFE (in R)**; global and per-strategy **Min R:R** overrides.  
- **Yearly Results View** — Trades grouped by year with per-year success rate and cumulative % profit.  
- **Artifacts** — One-click **ZIP** export of all run data for deeper analysis and comparison.

---

## What’s Inside

- **5Y daily backtests** (single position at a time)  
- **Exits**: target/stop, optional trailing (EMA20 / BB mid), optional time-stop  
- **Analytics**: R-multiple, %PnL, MAE/MFE (R)  
- **Min R:R control**: Global setting with **per-strategy overrides**  
- **Side filter**: BOTH / LONG / SHORT  
- **Export**: ZIP with bars, indicators, trades, equity, and runtime config

---

## Quick Start (UI)

1. Open **Backtest** page.  
2. Enter ticker, set **Min R:R** and **Side**.  
3. Use **Advanced overrides** to set per-strategy Min R:R.  
4. Click **Run**.  
5. Review:
   - **Chart (5Y daily)** with trade overlays  
   - **Per-strategy success bars** (filter by year)  
   - **Trades table** grouped by year  
   - **Per-year success rate** & **cumulative % profit**  
6. **Export**: Click **Export** to download a ZIP with all artifacts.

---

## Strategies & Scoring

**Enabled by default (Tier-1 + selected advanced):**  
`emaPullback`, `squeezeBreakout`, `rangeBreakoutVolume` (long),  
`emaPopFailShort` (short), `squeezeBreakdown` (short),  
`ema200Reclaim` (advanced long), `bbMeanRevertTrend` (advanced long)

**Available (disabled by default):**  
`macdZeroLineUp`, `emaBounceBullStack`, `rsiPositiveReversal`, `fibConfluencePullback`,  
`mtfAlignment`, `rangeBreakdownVolume` (short), `ema200RejectionShort`

**Pattern detection:** `patternFlagPennant`, `patternCupHandle`, `vcpContraction`

**Scoring model:**  
- **Base score**: `50 + sum(weights of hit factors)`  
- **Entry gate**: Per strategy via `strategies.<id>.minScore`  
- **Config**: Scoring weights live in `default-engine-config.json` (deep-mergeable)

---

## Exported Artifacts — File Shapes (JSON)

| File     | Description | Key Fields (excerpt) |
| :--      | :--         | :-- |
| `bars`   | Last ~1260 daily bars | `{ ts, iso, o, h, l, c, v, tf }[]` |
| `indicators` | Full indicator bundle | `ema20`, `rsi14`, `macd`, `bb:{upper,middle,lower,width,widthRank120}`, `atr14`, `donchian20`, `volMA20` |
| `trades` | Normalized trade log | `{ seq, symbol, strategy, side, entry, exit_price, exit_reason, R, pct, mae_R, mfe_R, confidence? }[]` |
| `equity` | Step equity & risk | `{ ts, ts_iso, equity, drawdown, exposure_pct }[]` |
| `config` | Runtime engine config | `{ generatedAt, symbol, engineConfig, minRR:{global,overrides}, side? }` |

---

## Contact

[offir.tura@gmail.com](mailto:offir.tura@gmail.com) · [LinkedIn](https://www.linkedin.com/in/offir-tura)
