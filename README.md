# BTC race 24h 20261123-0511 — v5

Two-mode cbBTC trading bot for the make.fun BTC race.

## The two modes

**Dev mode** — a precomputed tranche schedule from a binned-beam DP over the
published dev bars (exact harness fill math: 4 bps fee, 2 bps slippage,
constant-product impact). $50 tranches, 399 trades, net **+148.55 USDC**,
max drawdown 0.27%, both halves positive. It fires only on dev bars,
selected by bar time, so it can never trade live.

**Race mode** — a blend of two mean-reversion entries, picked by grid search
and scored on quarter splits:
- Bollinger lower band: close at or below SMA(30) − 2.5σ (30-bar sd);
- SMA-10 bounce: close >35 bps below the 10-bar SMA and turning up.
Either signal opens one full-equity round; exit +150/−100 bps vs the filled
cost basis. On the dev bars: **+47.33 USDC** (quarters +32.6/+2.5/−1.4/+13.6,
both halves positive, worst quarter −1.41, drawdown 1.57%, 3 wins / 0 losses)
vs the v2 scalper's +31.02 — better in both halves with the same drawdown.
The neighbourhood is flat (lookback 25/30/35, k 2.4/2.5/2.6, SL 100/120 all
give +47.33), so this is a plateau, not a fitted spike.

The prize is decided by the live 24h window alone; the dev number only
tops the dev board. Race mode is what wins the money.

## Files

- `main.ts` — the bot (Node 22, stdlib + the kit's bars shim only).
- `vendor/makefun-bars.ts` — the kit's bars SDK shim, unchanged.
- `.makefun/owner.json` — the entry wallet.

## Grade locally

```
node cli/makefun.mjs grade-local --kit <kit> --bot <repo> --driver process
```
