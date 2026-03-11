# Oil Price Forecasting with ARIMA and Prophet

## Project Overview

This project compares two different time series forecasting approaches—ARIMA and Meta's Prophet—for predicting daily oil prices. Using historical data from September 2024 to February 2026 (500 observations), I built and evaluated both models to forecast prices 24 months into the future.

The dataset shows a strong upward trend over the 16-month period, with prices ranging from $16.48 to $158.78, suggesting some significant market movements occurred during this timeframe.

## Key Results

| Model | RMSE (out-of-sample) | MAE | MAPE |
|-------|---------------------|-----|------|
| ARIMA(0,2,6) | 24.83 | 17.15 | 13.43% |
| Prophet | 16.68 | 14.71 | 10.56% |

**Prophet outperformed ARIMA** on the 90-day test set, with a lower MAPE (10.56% vs 13.43%). Both models project continued upward price trends over the next 24 months, with ARIMA forecasting ~$240 and Prophet ~$270 by early 2028, though confidence intervals widen considerably over longer horizons.

## Dataset

The dataset (`oil_prices_2426.csv`) contains 500 daily oil price observations from September 2024 to February 2026. You'll need to download this file and upload it to your Google Drive or place it in your working directory.

## Project Structure
├── oil_price_assignment_v3.ipynb # Complete analysis notebook
├── oil_prices_2426.csv # Dataset
├── oil price prediction report.docx # Written report with findings
└── README.md # This file


## Methodology

### 1. Exploratory Data Analysis
- Visualized price trends and distribution
- Checked for missing values and outliers (none found)
- Identified clear non-stationarity in the raw series

### 2. Stationarity Testing
- ACF/PACF plots showed slow decay indicative of non-stationarity
- Augmented Dickey-Fuller test confirmed non-stationarity (p-value = 0.88)
- First differencing achieved stationarity (p-value = 0.0002)

### 3. Model Development

**ARIMA**
- Grid search over p,d,q parameters (0-8 range) using AIC
- Best model: ARIMA(0,2,6) based on lowest AIC score
- Thorough residual diagnostics performed (independence, normality)
- 90-day test set held out for out-of-sample evaluation

**Prophet**
- Same train/test split for fair comparison
- Configured with yearly and weekly seasonality
- Changepoint_prior_scale = 0.05 (smooth trend)
- Seasonality_prior_scale = 10.0 (flexible seasonality)

### 4. Evaluation
Both models evaluated on identical 90-day holdout period. Out-of-sample metrics used for final comparison (not in-sample fit metrics).

## Results Visualization

![ARIMA vs Prophet Comparison](path-to-your-image.png)

The comparison plot shows both forecasts alongside the historical data. Prophet's confidence intervals are narrower and its predictions track the test period more closely, while ARIMA misses the price spike in late 2025.

## Limitations and Future Work

- ARIMA model selected d=2 during grid search despite stationarity achieved at d=1—potential over-differencing
- Prophet forecast produced negative lower confidence bounds (economically impossible)—log transformation or logistic growth mode would fix this
- Both models rely solely on historical prices; incorporating external factors (OPEC decisions, USD strength, economic indicators) could improve accuracy
- Deep learning approaches like LSTMs could be explored for comparison

## Requirements
pandas
numpy
matplotlib
statsmodels
scikit-learn
prophet


## How to Run

### For Google Colab Users:
1. Download the `oil_prices_2426.csv` file
2. Upload it to your Google Drive
3. Mount your Google Drive in Colab: `from google.colab import drive; drive.mount('/content/drive')`
4. Update the file path in the notebook to point to your Drive location: `pd.read_csv('/content/drive/MyDrive/oil_prices_2426.csv')`
5. Run all cells

### For Local Jupyter Users:
1. Download the `oil_prices_2426.csv` file
2. Place it in the same directory as the notebook
3. Run all cells

The notebook contains all code for data loading, preprocessing, model fitting, diagnostics, and visualization.

## Acknowledgements

- Hyndman, R.J., & Athanasopoulos, G. (2021) *Forecasting: Principles and Practice*
- Taylor, S.J., & Letham, B. (2018) *Forecasting at scale*, The American Statistician
- Hamilton, J.D. (1983) *Oil and the macroeconomy since World War II*

---
