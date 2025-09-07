# NVDA-LGBM-Return-Prediction-And-Strategy_Development

Predicting NVIDIA returns using a LightGBM model and analyzing trading strategies based on these predictions.

## Project Overview

This project develops a machine learning model to predict NVIDIA (NVDA) stock returns using LightGBM, followed by comprehensive trading strategy analysis. The project demonstrates the complete pipeline from feature engineering to strategy implementation, achieving impressive risk-adjusted returns.

## Repository Structure

- **`NVDA_LGBM copy.ipynb`** - Main model development notebook
- **`NVDA_LGBM_2 copy.ipynb`** - Strategy testing and optimization
- **`nvda_with_features.pkl`** - Processed dataset with engineered features
- **`best_params.json`** - Optimized hyperparameters

## Part 1: Model Development (`NVDA_LGBM copy.ipynb`)

### Data & Feature Engineering

**Base Data**: NVIDIA stock data (2015-2025) from Yahoo Finance
- OHLCV data with calculated returns as target variable
- No missing values, clean dataset ready for analysis

**Feature Engineering Pipeline**:
1. **Basic Technical Features** (Cell 2): Moving averages, volatility, momentum, price ratios
2. **Advanced Technical Indicators** (Cells 4-6): 100+ indicators including RSI, MACD, Bollinger Bands, Stochastic oscillators
3. **Market Context Features** (Cell 7): Correlations with S&P 500, NASDAQ, sector ETFs, competitor stocks
4. **Macro-Economic Features** (Cell 8): VIX, Treasury yields, dollar strength, commodity prices
5. **Sector-Specific Features** (Cell 9): Crypto prices (GPU demand proxy), gaming sector, cloud computing, memory sector

**Final Feature Set**: 200+ engineered features covering technical, fundamental, and macro factors

### Model Training & Evaluation

**Training Framework** (Cell 3):
- Custom LightGBM training function with TimeSeriesSplit cross-validation
- Comprehensive evaluation metrics: RMSE, Sharpe ratio, directional accuracy
- Automated plotting of performance across folds

**Feature Selection Process**:
1. **Importance Analysis** (Cell 12): Removed zero-importance features
2. **Correlation Analysis** (Cell 13): Eliminated highly correlated features (>0.95)
3. **Backward Selection** (Cell 14): Optimized for Sharpe ratio using systematic feature removal

**Final Model Features** (7 features):
- `Volatility5`: 5-day rolling volatility
- `Close_to_High10`: Price position relative to 10-day high
- `Chaikin_Oscillator`: Volume-price momentum indicator
- `Customer_Index_Return`: Weighted return of NVIDIA's major customers
- `DataCenter_Proxy_Return`: Data center sector performance
- `Memory_Momentum_10d`: Memory sector 10-day momentum
- `ETH_Mining_Proxy`: Ethereum mining profitability proxy

### Hyperparameter Optimization

**Optimization Process** (Cells 15-17):
- Optuna-based hyperparameter tuning with 100 trials
- Objective: Maximize Sharpe ratio across time series folds
- Robust optimization considering consistency across folds

**Final Model Performance**:
<img width="1489" height="490" alt="image" src="https://github.com/user-attachments/assets/d2df8c56-793f-44d4-9e90-7bcc8a396bf7" />


--- Overall Metrics ---

Average RMSE Gap (Validation - Train): 0.0074

Return volatility: 0.0324

Average up/down accuracy across all folds: 54.73%

Average Sharpe ratio across all folds: 2.124

Average validation RMSE across all folds: 0.0322

Validation RMSE < Validation Std Dev: 5/5 folds

## Part 2: Strategy Development (`NVDA_LGBM_2 copy.ipynb`)

### Regime Analysis & Model Robustness

**COVID Period Analysis** (Cell 4):
- Identified Sharpe ratio drop to ~1.05 during COVID period (Fold 2)
- Root cause: Model optimized for fundamental-driven regimes, struggles in liquidity-dominated periods
- **Decision**: Maintained original features as additional regime indicators degraded overall performance
- **Rationale**: Model specializes in bull market conditions where NVIDIA historically performs well

### Signal Refinement & Threshold Optimization

**Signal Analysis** (Cells 5-6):
- **Key Finding**: Strong asymmetric edge favoring long positions
- **Long Performance**: Excellent Sharpe ratios (2.7+) with consistent profitability
- **Short Performance**: Poor performance due to model's bull market specialization
- **Strategy Decision**: Focus on long-only approach to maximize edge

**Threshold Optimization Results**:
- **Optimal Threshold**: 0.015 (top 20% of predictions)
- **Expected Performance**: ~100 trades/year with 3.3 Sharpe ratio
- **Trade-off Analysis**: Balanced frequency vs. quality of signals

### Position Sizing & Strategy Implementation

**Position Sizing Methods Tested** (Cells 7-8):

1. **Binary Approach**: 100% position when signal triggered
   - **Result**: 82.6% total return, 3.31 Sharpe ratio
   
2. **Progressive Scaling**: Position size increases with signal strength
   - **Result**: 46.6% total return, 2.97 Sharpe ratio
   
3. **Kelly Criterion**: Mathematically optimal sizing
   - **Result**: 6.0% total return, 3.68 Sharpe (impractical due to low position sizes)

**Aggressive Kelly Strategy** (Cells 9-10):
- **Implementation**: Enhanced Kelly with higher position limits
- **Performance**: **103.6% total return, 3.32 Sharpe, 107 trades**
- **Risk Profile**: Moderate leverage with excellent risk-adjusted returns

### Final Strategy Recommendation

**Optimal Strategy Configuration**:
- **Entry Rule**: Long position when prediction > 0.015
- **Position Sizing**: Aggressive Kelly-based approach
- **Expected Metrics**:
  - Annual Sharpe Ratio: 3.32
  - Total Return: 103.6%
  - Number of Trades: 107
  - Win Rate: ~55%

## Key Insights & Limitations

### Model Strengths
- **Specialized Excellence**: Optimized for NVIDIA's dominant bull market periods
- **Feature Engineering**: Comprehensive feature set capturing multiple market dimensions
- **Risk Management**: Consistent performance with controlled overfitting
- **Practical Implementation**: Clear entry/exit rules with quantified expectations

### Known Limitations
- **Regime Dependency**: Performance degrades during liquidity-dominated market periods
- **Long Bias**: Model not effective for short positions
- **Market Specificity**: Optimized for growth stock characteristics

### Future Enhancements
- **Risk Management**: Implementation of stop-losses and position limits
- **Regime Detection**: Dynamic model switching based on market conditions
- **Portfolio Integration**: Multi-asset framework for diversification
- **Live Trading**: Real-time data integration and execution systems

### Overview

We successfully developed an optimized LightGBM model to predict NVDA returns, achieving extremely impressive results. Building on this foundation, we tested various trading strategies using these predictions to further enhance performance. Our most successful approach was an Aggressive Kelly strategy that delivered exceptional results: 103.6% total return, 3.32 Sharpe ratio, and 107 trades. If implemented correctly, this strategy could generate substantial profits

  
