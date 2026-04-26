# SMB 9 EMA Continuation Scanner

A trader's toolkit for the SMB Capital 9 EMA pullback continuation strategy. Includes a TradingView Pine Script indicator, a Pine Screener version, and a standalone HTML dashboard powered by the Finnhub API.

## What's included

| File | Purpose |
|------|---------|
| `index.html` | Standalone dashboard — live scanning, signal alerts, sound + browser notifications |
| `pine/SMB_9EMA_Continuation_Indicator.pine` | TradingView chart indicator with buy/sell triangles, EMA + VWAP plot, status table |
| `pine/SMB_9EMA_Screener.pine` | Pine Screener version for scanning watchlists inside TradingView |
| `docs/setup-guide.docx` | Detailed setup instructions and daily workflow guide |

## Live dashboard

Open `index.html` directly in a browser, or visit the GitHub Pages URL once deployed:

```
https://<your-username>.github.io/<repo-name>/
```

## Quick start

1. **Get a Finnhub API key.** Free tier at [finnhub.io/register](https://finnhub.io/register).
2. **Open the dashboard.** Either open `index.html` locally or visit the deployed Pages URL.
3. **Paste your key** into the modal. It stores in browser localStorage — never transmitted anywhere except the Finnhub API.
4. **Build a watchlist** of 10–15 active tickers (gappers, high-RVOL movers from your morning scan).
5. **Click Run Scan.** Auto-refreshes every 60 seconds by default.

## Strategy logic

The 9 EMA continuation strategy enters trending stocks **after** a pullback rather than chasing. Core mechanics:

- Price pulls back to or wicks the 9 EMA on a 1-minute chart
- Within 5 bars, price reclaims the 9 EMA and closes above the prior bar's high (long) or below its low (short)
- Volume contracts during the pullback and expands on the reclaim
- Filters: RelVol > 2x, price respects VWAP bias, 9 EMA > 20 EMA, active hours only (9:30–11:00 AM and 2:00–3:45 PM ET)

Stop goes immediately below the pullback low (long) or above the pullback high (short). Exit on a close on the opposite side of the 9 EMA.

## Filters, tunable

All filters can be toggled in the dashboard sidebar or in the Pine Script settings:

- **RelVol minimum** — default 2.0x. Raise to 3.0x on choppy days.
- **Min price** — default $5. Lower for small-cap focus, raise for liquid-only.
- **Pullback window** — default 5 bars. The reclaim must come within this many bars of the pullback.
- **VWAP bias** — longs above VWAP only, shorts below.
- **Trend filter** — 9 EMA must be above 20 EMA for longs, inverse for shorts.
- **Active hours** — suppresses signals during midday chop.

## Rate limits

Finnhub's free tier allows 60 API calls per minute. Each ticker costs 2 calls per scan (quote + 1-minute candles). With 15 tickers refreshing every 60 seconds, you'll use ~30 calls/min, well under the limit.

If you need more tickers or faster refresh:
- Finnhub paid tiers raise the cap considerably
- Polygon.io Starter ($29/mo) gives unlimited calls
- Alpaca free tier with brokerage account gives real-time IEX data

## Disclaimer

This is a personal trading tool. Signals are mechanical pattern matches, not trading advice. Test in a paper account before risking capital. Past performance of any strategy doesn't guarantee future results. You're responsible for your own trades.

## License

MIT — use, modify, share freely.
