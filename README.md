# Nvda-Lgbm-Return-Predictor
Predicting NVDA Returns using an LGBM model and then analysing the results from different trading strategies using these predictions

(NVDA_LGBM copy.ipynb)

Steps:
- Creating LGBM training/ evaluating function for continuos use (Cell 3)
- Adding lots of features to data including technical features and market features (Cells 2, 4-8)
-  Feature selection via:
  Importance analysis then Correlation Analysis then Backwards Selection (Cells 12-14)
- Hyperparameter Optimization (Cells 15-17


LGBM with finalized features/ hyperparameters produced these results:

- <img width="1490" height="490" alt="image" src="https://github.com/user-attachments/assets/1a6c2249-3a46-4ed3-84bd-843dcccf8d37" />

Average RMSE Gap (Validation - Train): 0.0072

--- Overall Metrics ---
Return volatility: 0.0323
Average up/down accuracy across all folds: 55.35%
Average Sharpe ratio across all folds: 2.118
Average validation RMSE across all folds: 0.0319
Validation RMSE < Validation Std Dev: 4/5 folds

(NVDA_LGBM_2.ipynb)

Drop in Sharpe in fold 2 aligns with covid period and indicates lack of regim detection in features:
We tested the model with some additional regim indicators but ultimately decided to stick with original features as these produced best results (Cell 4)

# Strategy testing 

After finalizing the model - we looked to test some investment strategies using our predictions and analyse their effectiveness

(Cells 5-6)
- We started with signal refinement: from this we noticed strong performance when predicting/ investing in Longs however not for Shorts
  This is most likely as NVDA is long dominant over the data period so shorts are unpredictable
  Due to this we decided to test some long only strategies

(Cells 7-8)
Threshold Optimization: 
- Tested different trading strategies such as Binary, Progressive and Kelly based:
  Aggressive Kelly based gave impressive results: '103.6% total return, 3.32 Sharpe, N Trades: 107'

(Cells 9-10)

Position Sizing:
  This would most certainly be VERY profitable with some risk-management also added to it.

  We did not delve further into Risk Management, Backtesting as the main focus of this project was building/ perfecting the LGBM model
  
  
