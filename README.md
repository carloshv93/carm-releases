# carm

Gold scalping Expert Advisor for MetaTrader 5 targeting XAU/USD on M1 timeframe using EMA55 trend confirmation.

Developed by Carlos Herrera | Telegram: @Carlos_Herrera_CR
Strategy by Armando 9K | Telegram: @armando9k

## Download

Get the latest `.ex5` from the [Releases](https://github.com/carloshv93/carm-releases/releases) page.

## Strategy

1. Detect trend by measuring EMA55 slope over the last 5 bars
2. Enter on price retracement to EMA55, or on wick rejection at EMA55
4. Manage position with break-even stop and trailing stop

### EMA Touch Entry
- In a BULL trend: ASK price must be at or below EMA55
- In a BEAR trend: BID price must be at or above EMA55
- Evaluated every tick while a trend is confirmed

### Wick Entry
- Evaluated once per new bar using the previous closed candle
- Requires minimum wick size (`WickMinPips`)
- Wick must touch/cross EMA55 AND candle must close on the correct side (above EMA for bull, below EMA for bear)
- If the wick did NOT reach EMA55: wick tip must be within `WickEmaMaxDist` pips of EMA55

## Position Management

### SL / TP Calculation
- SL = 3x the entry candle size (capped by MaxSlPips if set)
- TP = 2.5x SL (1:2.5 risk/reward ratio)

### Trailing Stop
- When profit reaches 1:1 (1x SL distance), SL moves to entry + 2 pips
- When profit reaches 1:2 (2x SL distance), SL moves to lock in 1:1 profit

### Trading Hours
- No new trades within 15 minutes of market close or within 15 minutes of market open
- If a position is open in the last 15 minutes before close, it is closed immediately regardless of profit/loss

## Parameters

| Input | Default | Description |
|-------|---------|-------------|
| LotSize | 0.00 | Trade volume (0 = auto: 0.01 per $1000 balance) |
| MaxSlPips | 0 | Maximum SL in pips (0 = no cap) |

## Setup (MT5 Configuration)

### 1. Allow WebRequest (required for version check and auto-update)

1. Open MT5 → **Tools** → **Options** → **Expert Advisors**
2. Check **"Allow WebRequest for listed URL"**
3. Add these URLs:
   - `https://gist.githubusercontent.com` (version check)
   - `https://github.com` (auto-download of new versions)
4. Click **OK**

### 2. Version blocking

If a minimum version is enforced remotely, the EA will refuse to initialize on outdated versions. Update to the latest version to resume trading.

## Auto-Update

The EA checks for updates every hour. When a new version is detected:

1. Downloads the `.ex5` from GitHub Releases to `MQL5\Files\carm.ex5`
2. Shows an alert with instructions to install:
   - Close MT5
   - Copy `MQL5\Files\carm.ex5` to `MQL5\Experts\carm.ex5`
   - Restart MT5
   - (Use **File → Open Data Folder** to find the paths)

If a minimum version is enforced, outdated EAs will refuse to initialize until updated.
