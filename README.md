# Adaptive Market-Making Strategy Simulator

A single-file Python simulation that compares an adaptive market-making strategy against a fixed-spread baseline on a synthetic limit order book. Order arrivals follow Poisson processes, volatility switches between normal, high and extreme regimes on a schedule, and the adaptive quoter widens or skews its spread in response to realised volatility, inventory position and order-flow imbalance. The script runs both strategies through three stress scenarios, prints a metric-by-metric comparison, saves a CSV and two PNG figures.

## Requirements

- Python 3.8 or newer
- NumPy, Pandas, Matplotlib

```bash
pip install numpy pandas matplotlib
```

## Setup

```bash
git clone https://github.com/samarthpaliwal/adaptive-market-making-simulator.git
cd adaptive-market-making-simulator
```

No environment variables or configuration files are needed. `np.random.seed(42)` is set at import, so runs are reproducible.

## Usage

```bash
python adaptive_market_making_simulator.py
```

The script runs the adaptive strategy and then the static baseline through the three scenarios (1,000 steps each), prints a comparison table per scenario (metric, adaptive value, static value, percent change), and writes to the current directory:

- `strategy_comparison.csv` - the comparison table
- `market_making_comparison.png` - 9-panel figure: cumulative PnL, inventory and spread width (bps) for each of the three scenarios
- `performance_metrics_comparison.png` - bar charts of the key metrics

`strategy_comparison_full.csv` in the repository is the output of a previous run.

## How it works

Market environment (`adaptive_market_making_simulator.py`):

- `MarketRegime` / `MarketState` - regime enum (NORMAL, HIGH_VOLATILITY, EXTREME_VOLATILITY) and the per-step state (mid price, volatility, order-flow imbalance, regime).
- `LimitOrderBook` - evolves the mid price with regime-dependent volatility and exposes the current `MarketState`; each strategy's `process_fills` decides which of its quotes get hit by the generated orders.
- `OrderGenerator` - Poisson buy/sell arrivals; the base rate (5.0) is scaled by 1.0 / 1.5 / 2.0 across regimes and biased by order-flow imbalance.

Strategies:

- `AdaptiveMarketMaker` - base spread 0.2% of mid, scaled by a volatility multiplier (`volatility_spread_multiplier = 2.0`), shifted by an inventory penalty (`inventory_penalty = 0.001`) and an imbalance term (`imbalance_sensitivity = 0.0005`); `max_inventory = 100`, quote size 10.
- `StaticMarketMaker` - fixed 0.3% spread, same inventory limit and size; the baseline.

Simulation and analysis:

- `MarketMakingSimulator` - steps the book, applies the scenario's regime changes, records quotes, inventory, PnL, volatility and fills.
- `StressTestSuite` - defines the `normal_market`, `volatility_spike` and `extreme_stress` regime schedules and runs a strategy class through all three.
- `PerformanceMetrics` - computes Total PnL, Sharpe ratio, max drawdown, inventory drift, average absolute inventory, average spread (bps), fill count and PnL volatility, and builds the comparison DataFrame.
- `create_visualizations` - the two PNG figures.

Results from the committed `strategy_comparison_full.csv` (seed 42):

- In the extreme-stress scenario the adaptive strategy's PnL volatility was 74.63 versus 116.44 for the static baseline, a 35.9% reduction, and its average spread widened to 69.12 bps versus the fixed 30 bps, a 130.4% expansion.
- In the same run the adaptive strategy had lower total PnL and Sharpe ratio than the static baseline in all three scenarios (for example 374.07 vs 1974.99 total PnL under extreme stress, and a loss of -4894.26 vs a gain of 4671.17 in the volatility-spike scenario). The adaptive policy trades return for lower variance in the extreme regime; it is not a uniformly better strategy in this configuration.

Parameters to experiment with are the attributes set in the strategy constructors (`base_spread`, `volatility_spread_multiplier`, `inventory_penalty`, `imbalance_sensitivity`, `max_inventory`), `OrderGenerator.base_arrival_rate`, and the `duration` passed to `StressTestSuite.run_all_scenarios`.

## Project layout

```
adaptive-market-making-simulator/
├── adaptive_market_making_simulator.py   # all code: environment, strategies, simulator, metrics, plots, main()
├── strategy_comparison_full.csv          # saved output of a previous run
└── README.md
```

Running the script also produces `strategy_comparison.csv`, `market_making_comparison.png` and `performance_metrics_comparison.png`.
