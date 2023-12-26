# Multi-variate Forecast for multi-seasonal Zurich city's Energy Consumption 

Business Problem:

Predicting energy consumption has always been very important for a city to plan its resources for the smooth workflow of the city. The recent events in Europe have made this even more important for European cities. 

Project Objective:

Use time series forecasting methods, to forecast the energy consumption for the city of Zurich for 2023 up to the end of October using previous years' data. Further, we could leverage even weather data due to its correlation with energy consumption.

## Table of Contents

- [Project Overview](#project-overview)
- [Data](#data)
- [Workflow](#workflow)
- [Methodology](#methology)
- [Results](#results)
- [Dependencies](#dependencies)
- [Contact](#contact)
- [References](#references)

## Project Overview

The goal of this project is to develop a reliable forecasting solution for forecasting energy consumption for Zurich city in Switzerland.

## Data

* The energy consumption data for the city of Zurich [1]
* The weather data for the city of Zurich [2]

## Workflow

* Swiss_energy_forecast.ipynb contains the end-to-end code for Document parsing with database integration.
* images_app contains images of README.md

## Methogology
1. Holt-Winters forecasting
2. SARIMA
* For 1 and 2, yearly seasonality was removed by subtracting previous years since these methods could handle only one seasonality.
3. Hybrid model (Linear regression for trend forecast and XGBoost for seasonality forecast)
4. Prophet
5. Bidirectional LSTM

* The optimal parameters for 1,2 and 4, 5 methods were found by Grid search CV.
* The optimal parameters for 3 method were found by Bayesian search CV.
* Optimized on RMSE, AIC metrics

For multivariate forecasting, along with weather data, we also have 1-8 previous lags, target encoding in the form of average energy per month, isWeekend (same or isWeekday), isChristmus, day of the week, the month of the year. For the Prophet, we even used holidays in Switzerland.


## Results


Following are the images of energy consumption for Zurich 2020 onwards. As seen, 2020 has covid impact, thus for training, we use data from 2021 to the end of 2022. And test on 2023 data up to November 6th.

| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/energy_2020.png) | 2020 energy consumption  |
|-----------------------------|------------------|
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/energy_2021.png) | 2021 energy consumption  |
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/energy_2022.png) | 2022 energy consumption  |
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/energy_2023.png) | 2023 energy consumption  |

Following is the weather data for city of Zurich.

| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/weather_temp.png) | Temperature  |
|-----------------------------|------------------|
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/weather_cloud.png) | Cloud cover  |
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/weather_preci.png) | Precipitation |
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/weather_humidity.png) | Relative Humidity  |


| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/Holt_Winters.png) | Holt Winters |
|-----------------------------|------------------|
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/sarima.png) | SARIMA |
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/LR.png)| Linear Regression (for trend) | 
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/XGBoost.png)| Linear + XGBoost Regression (trend+seasonality)  |


| ![Image 1](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/LR_imp.png) | ![Image 2](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/xgboost_imp.png)
--- | --- 
Feature importance for Linear Regression forecast (trend) | Feature importance for Linear XGBoost forecast (seasonality)

| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/prophet.png)| Prophet |
|-----------------------------|------------------|
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/lstm.png)| BiLSTM |
| ![Image](https://github.com/krunalgedia/ZurichEnergyConsumption_MultivariateForecast/blob/main/images_app/lstm_ci%20(1).png)| BiSTLM (with 90% CI)|

Following is the table with the results:  

| method                                          |       r2 |         mse |    mae |forecasting |
|:------------------------------------------------|---------:|------------:|-------:|-----------:|
| Holt Winters ES                                 | 0.561794 | 2.01073e+11 | 343770 |  Univariate|
| SARIMA                                          | 0.656569 | 1.57585e+11 | 285656 |  Univariate|
| Linear Regression (trend)                       | 0.730851 | 1.235e+11   | 257814 |Multivariate|
| Linear + XGboost (Hybrid) Regression (trend+seasonality) | 0.839059 | 7.38485e+10 | 195714 |Multivariate|
| Prophet                                         | 0.706717 | 1.34575e+11 | 283883 |Univariate (with holidays)|
| BiLSTM                                          | 0.845019 | 7.13089e+10 | 199810 |Multivariate|

**As seen, based on MAE or r2, both Hybrid model (LR+XGBoost) and BiLSTM are comparable and better than others. **

## Dependencies

- statsmodels version: 0.14.1
- plotly version: 5.15.0
- ppscore version: 1.3.0
- category_encoders version: 2.6.3
- xgboost version: 2.0.2
- scikit-optimize version: 0.9.0
- prophet version: 1.1.5
- python version: 3.10.12
  
## Contact

Feel free to reach out if you have any questions, suggestions, or feedback related to this project. I'd love to hear from you!

- **LinkedIn:** [Krunal Gedia](https://www.linkedin.com/in/krunal-gedia-00188899/)

## References

1 : [swissgrid](https://www.swissgrid.ch/en/home/operation/grid-data/generation.html)

[2]: [swiss open data](https://opendata.swiss/de/dataset/klimamessnetz-tageswerte)
