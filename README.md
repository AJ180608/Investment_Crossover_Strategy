# 50/200-Day Moving Average Crossover Backtester

A simple Python backtesting program that evaluates the classic **50-day / 200-day moving average crossover strategy** against a traditional **Buy and Hold** strategy over the past five years.

The program accepts any valid stock ticker, downloads historical market data, generates trading signals, calculates performance metrics, and produces comparison charts.

## Features

* Accepts any valid stock ticker such as `AAPL`, `TSLA`, or `SPY`
* Downloads approximately 5 years of daily historical data
* Downloads an additional 200 trading days of historical data to initialize the 200-day moving average
* Calculates:

  * 50-day Simple Moving Average (SMA)
  * 200-day Simple Moving Average (SMA)
* Generates:

  * **Buy signal:** 50-day SMA crosses above 200-day SMA
  * **Sell signal:** 50-day SMA crosses below 200-day SMA
* Executes trades at the **next day's opening price**
* Starts with **$10,000 initial capital**
* Invests 100% of available capital when a Buy signal occurs
* Moves 100% of the portfolio to cash when a Sell signal occurs
* Compares the strategy against a 5-year Buy and Hold strategy
* Calculates:

  * Total Return
  * CAGR
  * Maximum Drawdown
  * Win Rate
  * Sharpe Ratio
* Produces two charts:

  1. Stock price with 50-day and 200-day moving averages and Buy/Sell markers
  2. Crossover strategy portfolio value vs. Buy and Hold
* Provides clear error messages for invalid stock symbols

## Strategy

The strategy uses two simple moving averages:

* **50-day SMA:** Shorter-term trend
* **200-day SMA:** Longer-term trend

### Golden Cross

A Buy signal occurs when:

```text
50-day SMA moves from below the 200-day SMA
to above the 200-day SMA
```

The trade is executed at the **next day's opening price**.

### Death Cross

A Sell signal occurs when:

```text
50-day SMA moves from above the 200-day SMA
to below the 200-day SMA
```

The position is closed at the **next day's opening price**.

### Position Rules

The backtester uses a simple all-in/all-out approach:

```text
Buy signal  → 100% invested in stock
Sell signal → 100% in cash
```

No leverage, short selling, transaction costs, or partial positions are used.

## Benchmark

The strategy is compared with a standard **Buy and Hold** approach.

For Buy and Hold:

1. Start with $10,000.
2. Buy the stock at the beginning of the 5-year testing period.
3. Hold the stock throughout the entire testing period.
4. Compare the final portfolio value with the crossover strategy.

## Performance Metrics

### Total Return

Measures the overall percentage gain or loss during the backtest.

```text
Total Return = (Final Portfolio Value / Initial Capital - 1) × 100
```

### CAGR

Compound Annual Growth Rate measures the annualized growth of the portfolio.

```text
CAGR = (Final Value / Initial Value)^(1 / Years) - 1
```

### Maximum Drawdown

Measures the largest percentage decline from a previous portfolio peak.

```text
Drawdown = (Portfolio Value - Previous Peak) / Previous Peak
```

The most negative drawdown during the backtest is reported as Maximum Drawdown.

### Win Rate

The percentage of completed trades that were profitable.

```text
Win Rate = Profitable Trades / Total Completed Trades × 100
```

### Sharpe Ratio

Measures risk-adjusted return using the variability of daily portfolio returns.

For this project, the Sharpe ratio is calculated using daily returns and annualized using approximately 252 trading days.

```text
Sharpe Ratio = Mean Daily Return / Standard Deviation of Daily Return × √252
```

## Project Structure

```text
moving-average-backtester/
│
├── Investment_Crossover_Strategy.py
├── requirements.txt
└── README.md
```

## Requirements

Python 3.9 or newer is recommended.

The project uses the following libraries:

* `pandas` — data manipulation
* `numpy` — numerical calculations
* `yfinance` — historical stock data
* `matplotlib` — visualization

## Installation

Clone or download the project and navigate into its directory.

Install the required packages:

```bash
pip install -r requirements.txt
```

A typical `requirements.txt` file contains:

```text
pandas
numpy
yfinance
matplotlib
```

## Running the Program

Run the Python script:

```bash
python Investment_Crossover_Strategy.py
```

The program will ask for a stock ticker:

```text
Enter stock ticker: AAPL
```

It will then download the required historical data, run the backtest, calculate the performance metrics, and display the charts.

## Example

For example:

```text
Enter stock ticker: AAPL
```

The program may produce output similar to:

```text
==================================================
50/200 Moving Average Crossover Backtest
==================================================

Ticker: AAPL
Initial Capital: $10,000.00
Testing Period: 5 Years

Crossover Strategy
-------------------
Final Portfolio Value: $XX,XXX.XX
Total Return: XX.XX%
CAGR: XX.XX%
Maximum Drawdown: -XX.XX%
Win Rate: XX.XX%
Sharpe Ratio: X.XX

Buy and Hold
------------
Final Portfolio Value: $XX,XXX.XX
Total Return: XX.XX%
CAGR: XX.XX%
Maximum Drawdown: -XX.XX%
```

The exact results depend on the stock and the date on which the program is run.

## Charts

### 1. Moving Average Crossover Chart

The first chart displays:

* Stock price
* 50-day moving average
* 200-day moving average
* Buy signals
* Sell signals

This allows the user to visually identify the Golden Cross and Death Cross events.

### 2. Portfolio Comparison

The second chart compares:

* 50/200-day crossover strategy portfolio value
* Buy and Hold portfolio value

Both portfolios start with the same **$10,000 initial investment**.

## Data Handling

The program downloads approximately **5 years + 200 trading days** of historical data.

The additional 200 days are necessary because the 200-day moving average cannot be calculated correctly on the first day of the actual five-year testing period without historical prices preceding that date.

Only the requested five-year period is used when evaluating final strategy performance.

## Trade Execution

Signals are generated using the moving averages calculated from the current day's closing price.

To avoid assuming that a trade can be executed at the same closing price that generated the signal, the program executes the trade at the **following day's opening price**.

This helps avoid look-ahead bias.

For example:

```text
Day 1: 50-day SMA crosses above 200-day SMA
       ↓
       Buy signal generated
       ↓
Day 2: Position purchased at opening price
```


Other possible errors, such as network/data-download failures, should also produce a clear message rather than causing an unexplained program crash.


## Performance Requirement

The program is designed to complete a normal backtest in **under 5 seconds**, excluding unusual delays caused by the external market-data provider or network connection.

The calculation itself uses vectorized Pandas/NumPy operations rather than unnecessary Python loops wherever possible.

## Important Limitations

This project is intended for **educational purposes** and demonstrates how a basic moving-average crossover backtest works.

It does not account for:

* Brokerage fees
* Bid/ask spreads
* Slippage
* Taxes
* Dividends unless reflected by the selected data series
* Stock splits beyond the adjustments provided by the data source
* Fractional-share restrictions
* Market impact
* Short selling
* Leverage
* Intraday price movements

Historical backtest performance also does not guarantee future performance.
