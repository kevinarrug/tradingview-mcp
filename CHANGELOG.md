# Changelog

All notable changes to this project will be documented in this file.

## [0.7.0] - 2026-03-29

### Added
- **Walk-Forward Backtesting** (`walk_forward_backtest_strategy`):
  - Splits data into N folds (train/test) to validate strategy on unseen forward data
  - Per-fold in-sample vs out-of-sample return comparison
  - **Robustness score** (test/train ratio): ROBUST ≥ 0.8 | MODERATE ≥ 0.5 | WEAK ≥ 0.2 | OVERFITTED < 0.2
  - Aggregate out-of-sample metrics: Sharpe, win rate, max drawdown, total return
  - Supports 2–10 splits, configurable train ratio, both 1d and 1h intervals
- **Full Trade Log** (`include_trade_log=True`):
  - Per-trade breakdown: entry/exit date & price, holding days, gross/net return %, cost %
  - Running capital and cumulative return at each trade
- **Equity Curve** (`include_equity_curve=True`):
  - Capital value + drawdown % at each trade exit — ready for charting
- **1h (Hourly) Timeframe** (`interval="1h"`):
  - All strategies and compare now support intraday hourly data
  - Sharpe ratio annualization corrected for 1h bars (252 × 6 trading hours)
  - Works on `backtest_strategy`, `compare_strategies`, and `walk_forward_backtest_strategy`

### Changed
- `backtest_strategy` tool: added `interval`, `include_trade_log`, `include_equity_curve` params
- `interval` param; now documents all 6 strategies (was 4)
- `run_backtest()` now returns last 5 trades always (`recent_trades`) for quick inspection
- Sharpe ratio calculation now uses interval-aware annualization factor

### Notes (personal)
- I changed the default `interval` in `backtest_strategy` from `"1d"` to `"1h"` in my fork
  since I mostly test intraday setups. Change it back if you prefer daily.
- I also bumped the default `n_splits` in `walk_forward_backtest_strategy` from 5 to 3 —
  less granular but runs noticeably faster on longer date ranges, which suits my workflow.
- Bumped default `train_ratio` in `walk_forward_backtest_strategy` from 0.7 to 0.8 —
  I prefer giving the model more training data per fold, especially on shorter histories
  where 70% train leaves too few bars in the test window to be meaningful.
- Bumped default `initial_capital` from 10000 to 5000 — better reflects my actual
  testing budget and makes the equity curve numbers easier to reason about at a glance.
- Bumped default `recent_trades` count from 5 to 10 — 5 trades is often not enough
  context when reviewing intraday 1h results; 10 gives a better feel for recent behavior.
- Bumped default `commission` from 0.001 (0.1%) to 0.002 (0.2%) — more realistic for
  the crypto exchanges I actually use; the upstream default felt optimistically low and
  was making strategies look better than they perform in practice.
- Bumped default `slippage` from 0.0 to 0.001 (0.1%) — real fills on thinly traded
  crypto pairs rarely happen at the exact signal price; even a small slippage assumption
  keeps backtest results more honest. Combined with the 0.2% commission this gives a
  round-trip cost of ~0.6% per trade, which aligns with what I actually see on Binance
  and Kraken for mid-cap pairs with moderate liquidity.
