# 30-Min Opening Range Breakout (ORB) — TradingView Indicator

Pine Script v5 indicator that signals when price breaks out of the 30-minute
opening range with volume confirmation. See [`ORB_Breakout_Alert.pine`](ORB_Breakout_Alert.pine).

## Signal conditions (all must be true)

1. **Breakout** — price closes above the high of the 09:30–10:00 opening range.
2. **Volume** — current bar volume is at least **1.5x** the average bar volume
   of the opening range (multiplier is adjustable in settings).
3. **VWAP** — price is above the session VWAP.
4. **Time window** — the bar is between 10:00 and 11:30 (first 2 hours of the
   regular session).

## Visuals

- Orange horizontal line at the opening range high, drawn through the trading day.
- Green triangle above the first bar where the full signal appears.
- Light-green background on every bar where all conditions are met.

## Setup

1. In TradingView, open **Pine Editor**, paste the contents of
   `ORB_Breakout_Alert.pine`, and click **Add to chart**.
2. Use an intraday timeframe of 30 minutes or less (1m, 5m, or 15m recommended).
3. To create the alert: **Alerts → Create Alert**, set *Condition* to
   **ORB 30m → ORB Breakout Alert**, and set the frequency to **Once Per Bar**.

The alert message automatically includes the symbol, price, and time:

```
ORB Breakout: {{ticker}} broke the 30-min opening range high — price {{close}} at {{timenow}}
```

Session times default to `America/New_York` and can be changed in the
indicator settings.
