Advanced Time Series Forecasting with Prophet and Hyperparameter Optimization
1. Project Overview
This project focuses on engineering-grade time series forecasting using Meta/Facebook Prophet, combined with systematic hyperparameter optimization and robust time-series cross-validation.
Rather than simply applying Prophet, this project treats forecasting as a full ML engineering workflow, including:


Complex dataset generation or acquisition


Automated daily time-series generation with seasonality, trends, changepoints, holidays, and noise


Rolling-origin (expanding window) cross-validation


Grid search for Prophet hyperparameters


Baseline comparison using Holt–Winters Exponential Smoothing


Final evaluation on a holdout set using RMSE, MAE, and MASE


Modular, industry-grade, well-documented Python code


This project is suitable for academic submission, interviews, portfolio work, or real-world forecasting pipelines.

2. Project Structure
prophet_time_series_project.py   # Main end-to-end pipeline
output/
    simulated_series.csv         # Auto-generated dataset
    simulated_holidays.csv       # Holiday metadata
    prophet_tuning_results.csv   # Grid search CV results
    prophet_holdout_forecast.csv
    hw_holdout_forecast.csv
    prophet_holdout_metrics.csv
    hw_holdout_metrics.csv


3. Objectives
Primary Goal
Implement and rigorously evaluate an optimized Prophet forecasting pipeline that outperforms a classical baseline model.
Secondary Goals


Build reproducible and modular Python code suitable for production.


Perform thorough hyperparameter tuning using cross-validation.


Create a quantitative performance comparison with classical forecasting.



4. Dataset Description
The dataset is synthetic daily data spanning 3 years (≈1095 rows).
It contains:


Trend with changepoints (slope changes at 1/3 and 2/3 of dataset)


Weekly seasonality (weekend effects)


Yearly seasonality (sinusoidal)


Holiday effects (random spikes)


Noise and outliers


Two files are generated:
FileDescriptionsimulated_series.csvMain dataset (ds, y)simulated_holidays.csvHoliday events for Prophet
This dataset mimics real-world retail, energy, or traffic forecasting signals.

5. Key Features of the Pipeline
✔ 1. Dataset Generation
A sophisticated synthetic dataset is automatically generated with:


Seasonality


Trend shifts


Holidays


Outliers


Gaussian noise


✔ 2. Time Series Cross-Validation
Implements rolling-origin CV:


Initial training window: 365 days


Forecast horizon per fold: 90 days


Step size: 90 days


This mimics real forecasting workflows.
✔ 3. Prophet Hyperparameter Optimization
Grid search over:


changepoint_prior_scale


seasonality_prior_scale


seasonality_mode


weekly_seasonality


yearly_seasonality


Each combination is evaluated using CV.
✔ 4. Baseline Model
Holt-Winters Exponential Smoothing with:


Additive trend


Additive seasonality


Weekly seasonality (7-day period)


✔ 5. Evaluation Metrics
Uses on holdout set:


RMSE


MAE


MASE (primary metric for overall accuracy)


✔ 6. Modular Code
All steps are encapsulated into clear functions:


Data generation


Cross-validation splitting


Hyperparameter tuning


Model fitting


Evaluation


Visualization



6. Installation
Required Packages
Install dependencies:
pip install prophet numpy pandas scikit-learn statsmodels matplotlib

(For optional Bayesian optimization)
pip install scikit-optimize optuna


7. Running the Project
Generate only the dataset
python prophet_time_series_project.py --generate-data

Saved to output/.
Run the full forecasting pipeline
(with tuning, baseline model, evaluation, plots, and all CSV outputs)
python prophet_time_series_project.py --run-all

This may take several minutes depending on your machine.

8. Hyperparameter Tuning Methodology
Why Rolling-Origin CV?
Prophet is sensitive to:


changepoint flexibility


seasonality prior strength


additive vs multiplicative seasonality


Using random CV is invalid for time series.
Hence, rolling origin validation is used:
Train  → Test
Train+90 → Test+90
Train+180 → Test+180
...

Each fold produces RMSE/MAE/MASE, averaged for final model selection.
Search Space
The following grid is used (modifiable):
HyperparameterValueschangepoint_prior_scale[0.001, 0.01, 0.05, 0.1]seasonality_prior_scale[1.0, 5.0]seasonality_mode['additive', 'multiplicative']weekly_seasonality[True, False]yearly_seasonality[True, False]
Model Selection Criterion
Sort by:


MASE


RMSE


Lower = better.

9. Baseline Model Details
A Holt-Winters Exponential Smoothing model is added for comparison:
trend='add'
seasonal='add'
seasonal_periods=7

This provides a strong classical baseline.

10. Evaluation & Outputs
Evaluation Metrics on Holdout Data (180 days)
For both models:


RMSE


MAE


MASE


Output files:
FileDescriptionprophet_holdout_metrics.csvProphet final performancehw_holdout_metrics.csvHolt-Winters performance

11. Visualizations
A combined plot is saved/shown:


Actual values


Prophet predictions


Holt-Winters predictions


Used to inspect accuracy visually.

12. How to Modify the Project
To use a real dataset
Replace:
df, holidays = generate_synthetic_daily_series(...)

with:
df = pd.read_csv("your_dataset.csv")

To add Bayesian optimization
I can add a full Bayesian tuner using Optuna/Skopt.
Just ask: “Add Bayesian optimization tuner”.
To export results as PDF/Report
I can generate a complete report automatically.

13. Conclusion
This project demonstrates:


Advanced time series forecasting


Strong engineering workflow


Hyperparameter optimization


Proper time series validation methods


Clean, modular, reusable code


It is fully suitable for academic submission, production deployment, or a strong project portfolio.
