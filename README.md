# carm

Gold scalping Expert Advisor for MetaTrader 5 targeting XAU/USD on M1 timeframe using EMA crossover with trend confirmation.

Developed by Carlos Herrera | Telegram: @Carlos_Herrera_CR
Strategy by Armando 9K | Telegram: @armando9k

## Download

Get the latest `.ex5` from the [Releases](https://github.com/carloshv93/carm-releases/releases) page.

## Strategy

1. Detect EMA crossover (EMA10 crossing EMA55)
2. Confirm trend by waiting for minimum pip separation between EMAs
3. Enter on price retracement to EMA55, or on wick rejection at EMA55
4. Manage position with break-even stop and trailing stop

### EMA Touch Entry
- Current bid price must be within 25% of the current candle size from EMA55
- Evaluated every tick while a trend is confirmed

### Wick Entry
- Evaluated once per new bar using the previous closed candle
- Requires minimum wick size (`WickMinPips`)
- Wick must touch/cross EMA55 AND candle must close on the correct side (above EMA for bull, below EMA for bear)
- If the wick did NOT reach EMA55: wick tip must be within `WickEmaMaxDist` pips of EMA55

## Position Management

### SL / TP Calculation
- SL = 3x the entry candle size (capped by MaxSlPips if set)
- TP = 3x SL (1:3 risk/reward ratio)

### Break-Even
- When profit reaches 1x SL distance, SL moves to entry price

### Trailing Stop
- Activates after break-even is set
- Initial reference: price at the moment break-even triggered
- Each time price makes a new extreme (high for BUY, low for SELL) that exceeds the previous by 10+ pips, SL moves 10 pips from its current position toward more profit
- SL never moves below entry price (break-even floor)

## Parameters

| Input | Default | Description |
|-------|---------|-------------|
| LotSize | 0.01 | Trade volume |
| EmaSlow | 55 | Slow EMA period |
| EmaFast | 10 | Fast EMA period |
| CrossMinPips | 5.0 | Minimum EMA separation (pips) to confirm trend |
| MaxSlPips | 0 | Maximum SL in pips (0 = no cap) |
| PipMultiplier | 100 | Point-to-pip multiplier (100 for gold) |
| WickMinPips | 5.0 | Minimum wick size for wick entry |
| WickEmaMaxDist | 3.0 | Max wick-to-EMA distance when wick doesn't touch EMA |

## Setup (MT5 Configuration)

### 1. Allow WebRequest (required for version check and auto-update)

1. Open MT5 → **Tools** → **Options** → **Expert Advisors**
2. Check **"Allow WebRequest for listed URL"**
3. Add these URLs:
   - `https://gist.githubusercontent.com` (version check)
   - `https://github.com` (auto-download of new versions)
4. Click **OK**

### 2. Allow DLL imports (required for auto-update install)

1. When attaching the EA to a chart, go to the **Dependencies** tab
2. Check **"Allow DLL imports"**

Without DLL imports, the EA will still download the update but cannot install it automatically. It will alert you to copy the file manually from `MQL5\Files\` to `MQL5\Experts\`.

### 3. Version blocking

If a minimum version is enforced remotely, the EA will refuse to initialize on outdated versions. Update to the latest version to resume trading.

## Auto-Update

The EA checks for updates every hour via the Gist URL. When a new version is detected:

1. Downloads the `.ex5` from GitHub Releases automatically
2. Copies it to `MQL5\Experts\carm.ex5` (requires DLL imports enabled)
3. Alerts you to restart the EA to apply the update

If DLL imports are disabled, the file is saved to `MQL5\Files\carm_update.ex5` and an alert will instruct you to copy it manually.

If a minimum version is enforced, outdated EAs will refuse to initialize until updated.
