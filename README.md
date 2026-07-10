# carm

Gold scalping Expert Advisor for MetaTrader 5 targeting XAU/USD on M1 timeframe.

Developed by Carlos Herrera | Telegram: @Carlos_Herrera_CR
Strategy by Armando 9K | Telegram: @armando9k

## Download

Get the latest `.ex5` from the [Releases](https://github.com/carloshv93/carm-releases/releases) page.

## Strategy

1. Detect EMA crossover (EMA10 crossing EMA55)
2. Confirm trend by waiting for minimum pip separation between EMAs
3. Enter on price retracement to EMA55, or on wick rejection at EMA55
4. Manage position with break-even stop and trailing stop

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
   - `https://gist.githubusercontent.com`
   - `https://github.com`
4. Click **OK**

### 2. Allow DLL imports (required for auto-update install)

1. When attaching the EA to a chart, go to the **Dependencies** tab
2. Check **"Allow DLL imports"**

Without DLL imports, the EA will still download the update but cannot install it automatically. It will show an alert to copy the file manually from `MQL5\Files\` to `MQL5\Experts\`.

### 3. Version blocking

If a minimum version is enforced remotely, the EA will refuse to initialize on outdated versions. Update to the latest version to resume trading.

## Position Management

### Break-Even
- When profit reaches 1x SL distance, SL moves to entry price

### Trailing Stop
- Activates after break-even is set
- Each time price makes a new extreme that exceeds the previous by 10+ pips, SL moves 10 pips toward more profit
- SL never moves below entry price (break-even floor)
