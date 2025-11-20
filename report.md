
# HAR Project — Submission Report

## Overview
This project implements a Hierarchical AutoRegressive (HAR) forecasting pipeline and compares it to a baseline hourly SARIMA model. The HAR model is implemented as a linear regression using hierarchical lag inputs:
- lag_1: previous hour
- lag_24_mean: previous 24-hour mean (up to t-1)
- lag_168_mean: previous 7-day (168h) mean (up to t-1)

Reconciliation uses **Bottom-up** proportional scaling: hourly HAR forecasts are scaled so each 24-hour block sums to the daily HAR forecast.

## Models
- Baseline: SARIMA applied to hourly series (auto_arima used if available).
- HAR: LinearRegression (lag features as above). Daily HAR model uses daily lag_1 and lag_7 means.

## Backtest setup
- Rolling-origin evaluation on last 90 days
- Retrain frequency: every 7 days
- Horizon per block: 24 hours
- Metrics: MAE, RMSE, MAPE

## Metrics (aggregated)
| model                     |      MAE |     RMSE |     MAPE |
|:--------------------------|---------:|---------:|---------:|
| baseline_fast_persistence |  7.27782 |  8.28386 |  7.94812 |
| HAR_reconciled            | 88.1511  | 88.5609  | 95.8483  |

## Key artifacts (in this folder)
- results_metrics.csv
- har_coefficients.csv
- feature_importances.csv (if generated)
- series_last_120_days.png
- decomp_trend.png
- decomp_seasonal.png
- forecast_overlay.png
- error_comparison.png
- shap_summary.png (if generated)

## Notes
- HAR coefficients show direct feature importance (linear). SHAP for surrogate RF also provided.
- Reconciliation implemented is bottom-up; for production consider MinT/optimal combination for better efficiency.

