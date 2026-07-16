# 30-Min Opening Range Breakout (ORB) — TradingView Indicator

Pine Script v5 indicator that signals when price breaks out of the 30-minute
opening range with volume confirmation, in either direction. See
[`ORB_Breakout_Alert.pine`](ORB_Breakout_Alert.pine).

## Signal conditions (all must be true)

**Long / breakout:**

1. **Breakout** — price wicks above the high of the 09:30–10:00 opening range
   (triggers intrabar, on the bar's high — not on close — so the signal
   doesn't lag behind fast moves).
2. **Volume** — current bar volume is at least **1.5x** the average bar volume
   of the opening range (multiplier is adjustable in settings).
3. **VWAP** — price is above the session VWAP.
4. **Time window** — the bar is between 10:00 and 11:30 (first 2 hours of the
   regular session).

**Short / breakdown (mirror image):**

1. Price wicks **below** the opening range **low** (intrabar, on the bar's low).
2. Same volume condition.
3. Price is **below** the session VWAP.
4. Same time window.

## Visuals

- Orange horizontal line at the opening range high and red line at the opening
  range low, drawn through the trading day.
- Green triangle above the first breakout bar / red triangle below the first
  breakdown bar.
- Light-green (long) or light-red (short) background on every bar where all
  conditions are met.

## Setup

1. In TradingView, open **Pine Editor**, paste the contents of
   `ORB_Breakout_Alert.pine`, and click **Add to chart**.
2. Use an intraday timeframe of 30 minutes or less — 5m or 15m recommended.
   The script raises an error on any higher timeframe (or daily+), since the
   09:30–10:00 opening range can't be built from whole bars there.
3. To create alerts: **Alerts → Create Alert**, set *Condition* to
   **ORB 30m → ORB Breakout Alert (Long)** or
   **ORB 30m → ORB Breakdown Alert (Short)**, and set the frequency to
   **Once Per Bar**. Create one alert per direction if you want both.

The alert messages automatically include the symbol, price, and time:

```
ORB Breakout: {{ticker}} broke ABOVE the 30-min opening range high — price {{close}} at {{timenow}}
ORB Breakdown: {{ticker}} broke BELOW the 30-min opening range low — price {{close}} at {{timenow}}
```

Session times default to `America/New_York` and can be changed in the
indicator settings.

## Tuning

The volume condition compares each bar to the opening range's own average
volume. Default is **1.0x** (current bar volume must be at least the OR's
average bar volume) — loose enough that most genuine breakouts qualify.
Raise it (e.g. to 1.5x) if you want fewer, higher-conviction signals; on
high-volatility gap days, heavy volume during the 09:30–10:00 range itself
can raise that average enough that a high multiplier delays the signal well
past the actual price break.

If a move happens but no alert fires, also check the **time window**: the
signal only evaluates between 10:00 and 11:30 in the session timezone
(`America/New_York` by default). A breakout that occurs, or only completes
all conditions, after 11:30 will not fire — this is by design, not a bug.
