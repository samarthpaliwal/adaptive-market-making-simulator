# Adaptive Market-Making Strategy Simulator

## 🎯 Project Overview

A professional-grade quantitative finance simulator that demonstrates adaptive market-making strategies under various stress scenarios.

## 📦 Contents

- `adaptive_market_making_simulator.py` - Main simulation code (600+ lines)
- `strategy_comparison_full.csv` - Performance metrics comparison
- `README.md` - This file

**Note**: Run the Python script to generate visualization PNG files:
- `market_making_comparison.png` - Visual comparison of strategies
- `performance_metrics_comparison.png` - Key metrics visualization

## 🚀 Quick Start

```bash
python adaptive_market_making_simulator.py
```

## 📊 What It Does

1. **Simulates limit order book** with regime-switching volatility
2. **Implements adaptive market-making** that adjusts to:
   - Volatility changes
   - Inventory position
   - Order flow imbalance
3. **Stress tests** across three scenarios:
   - Normal market
   - Volatility spike
   - Extreme stress
4. **Compares** adaptive vs static baseline strategy

## 🎓 Key Results

- **35.9% reduction** in PnL volatility during extreme stress
- **130% spread expansion** during extreme volatility for risk control
- Demonstrates inventory management and adaptive decision-making
- Built with Poisson order arrivals and regime-switching models

## 💼 Resume-Ready Statements

```
1. Developed adaptive market-making simulator that dynamically adjusted
   quote width under volatility shocks, expanding spread by 130% during
   extreme stress conditions while maintaining liquidity provision.

2. Implemented risk-aware decision policy that reduced PnL volatility by
   35.9% in extreme stress scenarios through adaptive spread widening
   and inventory management.

3. Evaluated robustness across three market regimes (normal, high-volatility,
   extreme stress), demonstrating adaptive strategy's ability to preserve
   capital during drawdown events vs static baseline.

4. Built end-to-end simulation framework using Poisson order arrival processes,
   regime-switching volatility models, and inventory-driven quote adjustments
   to model real market microstructure dynamics.
```

## 🛠️ Technical Stack

- Python 3.8+
- NumPy - Numerical computations
- Pandas - Data analysis
- Matplotlib - Visualizations

## 📈 Key Components

### Market Environment
- `LimitOrderBook` - Simulates bid/ask dynamics
- `OrderGenerator` - Poisson process order arrivals
- `MarketRegime` - Normal, high-volatility, extreme stress

### Strategy Engines
- `AdaptiveMarketMaker` - Dynamic quoting based on market conditions
- `StaticMarketMaker` - Baseline fixed-spread strategy

### Simulation & Analysis
- `MarketMakingSimulator` - Main simulation engine
- `StressTestSuite` - Scenario definitions
- `PerformanceMetrics` - Comprehensive evaluation

## 🎨 Customization

Modify these parameters in the strategies:

```python
# In AdaptiveMarketMaker
self.base_spread = 0.002  # Base spread width
self.volatility_spread_multiplier = 2.0  # Volatility response
self.max_inventory = 100  # Inventory limits

# In OrderGenerator
self.base_arrival_rate = 5.0  # Order flow intensity
```

## 📊 Output Files

When you run the simulator, it generates:
- `strategy_comparison.csv` - Detailed metrics
- `market_making_comparison.png` - 9-panel comparison
- `performance_metrics_comparison.png` - Bar chart metrics

## 🏆 Why This Project Stands Out

✅ **Real market microstructure** - Not just random price movements  
✅ **Adaptive decision-making** - Responds to changing conditions  
✅ **Risk management** - Inventory control and exposure limits  
✅ **Stress testing** - Multiple scenarios with regime changes  
✅ **Quantifiable results** - Concrete performance improvements  
✅ **Production-quality code** - Clean, documented, extensible  

## 📝 Project Background

This project was designed as a hire-tier quantitative research portfolio piece, demonstrating:
- Behavioral modeling under uncertainty
- Risk-aware decision policies
- Robustness evaluation across market conditions

Perfect for interviews at quantitative trading firms, market-making shops, or systematic trading teams.

## 🤝 Usage

Feel free to:
- Extend the adaptive strategy with ML-based policies
- Add more stress scenarios
- Implement additional risk metrics
- Experiment with different order flow models

---

**Author**: Quant Research Project  
**Purpose**: Portfolio demonstration of quantitative strategy development  
**Date**: February 2026
