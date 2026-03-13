# Forecasting Seasonal Influenza Dynamics Using Statistical Time Series Models

A statistical forecasting project modeling seasonal influenza activity using classical time series methodology applied to CDC surveillance data. The analysis explores variance stabilization, stationarity transformation, and seasonal autoregressive models to generate interpretable forecasts of influenza trends.

---

## Project Overview

Seasonal influenza exhibits strong temporal structure with pronounced yearly cycles, making it an ideal candidate for statistical time series modeling. Accurate forecasting of influenza activity is valuable for public health planning, allowing healthcare systems to anticipate demand and allocate resources more effectively.

This project analyzes weekly influenza surveillance data from the CDC FluView system (2015–2024) and develops forecasting models to predict the percentage of laboratory-confirmed influenza cases.

The analysis investigates how different modeling approaches perform under complex real-world conditions, including:

- strong seasonal patterns  
- structural breaks caused by the COVID-19 pandemic  
- highly skewed disease activity distributions  

The primary objective is to determine which models best capture influenza dynamics while maintaining statistical interpretability.

---

## Dataset

The dataset consists of weekly CDC influenza surveillance observations spanning nine influenza seasons.

Key dataset characteristics:

- 468 weekly observations  
- 2015–2024 time horizon  
- 9 complete flu seasons  
- 0 missing values  

### Target Variable

LAB_FLU_PCT_POSITIVE

Weekly percentage of laboratory tests that are positive for influenza.

This metric provides a direct measure of influenza activity and allows for detailed analysis of seasonal disease dynamics.

---

## Exploratory Data Analysis

Initial exploration reveals several important features of the data.

### Strong Seasonal Structure

Influenza activity follows a consistent annual cycle, with sharp spikes occurring each winter (typically January–February).

### Highly Skewed Distribution

Most weeks exhibit very low influenza activity, with occasional extreme peaks during seasonal outbreaks.

This right-skewed distribution introduces forecasting challenges, as models must predict both:

- when outbreaks occur  
- how large the peaks become  

### COVID-19 Structural Break

A major structural disruption occurs in March 2020, when influenza activity nearly disappears for approximately 18 months due to:

- social distancing  
- masking policies  
- reduced travel  
- displacement by COVID-19 infections  

Structural break analysis identifies three major change points:

- December 2017  
- March 2020  
- February 2022  

The March 2020 break corresponds directly to the onset of the COVID-19 pandemic.

---

## Methodology

The modeling pipeline follows a standard statistical time series workflow.

### 1. Variance Stabilization

A Box-Cox transformation is applied to stabilize variance and reduce heteroskedasticity.

Estimated parameter:

λ = 0.3617

---

### 2. Stationarity Testing

Stationarity is assessed using two complementary statistical tests.

ADF Test  
- p < 0.01  
- Rejects the unit root hypothesis  

KPSS Test  
- p = 0.095  
- Fails to reject stationarity  

Together these results indicate that the series is stationary after seasonal differencing.

---

### 3. Differencing

To remove seasonal persistence, a seasonal difference at lag 52 is applied.

This captures the annual influenza cycle and prepares the data for ARIMA-type modeling.

---

<img width="870" height="487" alt="Screenshot 2026-03-13 at 2 30 24 PM" src="https://github.com/user-attachments/assets/f61209be-b76e-4dad-b0b2-921d4af33a15" />


### 4. Model Construction

Several candidate models are evaluated, including:

- SARIMA  
- ARIMAX  
- TBATS  
- naive baselines  

Models are compared using:

- AICc  
- Ljung-Box residual tests  
- forecast accuracy metrics  

---

## SARIMA Model Development

The primary statistical model explored is a Seasonal Autoregressive Integrated Moving Average (SARIMA) specification.

Model selection is based on:

- lowest AICc  
- residual independence (Ljung-Box p > 0.05)

The selected model is:

SARIMA(2,0,2)(1,1,1)[52]

Additional model details:

- Box-Cox λ = 0.3617  
- σ² = 0.7747  
- log-likelihood = −569.38  

---
<img width="865" height="486" alt="Screenshot 2026-03-13 at 2 30 43 PM" src="https://github.com/user-attachments/assets/de562410-1623-4398-9599-5d0e43431967" />

<img width="852" height="490" alt="Screenshot 2026-03-13 at 2 30 49 PM" src="https://github.com/user-attachments/assets/1d1287a3-0c6c-4062-8b3e-687483e0951c" />

## COVID Structural Adjustment

To account for pandemic-related disruptions, a COVID dummy variable is incorporated.

Estimated coefficient:

COVID dummy = −1.579

This covariate captures the dramatic suppression of influenza during the 2020–2021 period and significantly improves model performance.

Including the COVID indicator reduces forecasting error:

RMSE improvement

4.456 → 2.587

(approximately 42% reduction)

---

## Model Diagnostics

Residual diagnostics confirm that the selected SARIMA model satisfies key statistical assumptions.

Residuals appear randomly distributed around zero.

ACF plots show nearly all lags within confidence bands.

Ljung-Box test:

p = 0.075

This indicates no remaining autocorrelation in the residuals.

Residuals are approximately normally distributed and centered at zero.

---

## Forecast Performance

Model performance is evaluated using a hold-out test set covering:

October 2023 – September 2024

Forecast accuracy metrics include:

- RMSE  
- MAE  
- MAPE  

Key results:

- SARIMA captures seasonal peaks most accurately  
- naive models perform poorly during outbreak periods  
- SARIMA achieves approximately 49% lower RMSE than the next best baseline  

---

## Extended Models

Several extended forecasting models were also explored.
<img width="984" height="547" alt="Screenshot 2026-03-13 at 2 33 38 PM" src="https://github.com/user-attachments/assets/4c2b9852-b04b-4e9c-bf63-0cd9389903e9" />


### ARIMAX Models

These models incorporate external epidemiological signals:

- ILINet outpatient influenza indicators  
- ESSENCE emergency department surveillance  
- ICU flu admissions  
- pediatric influenza deaths  

The best performing extended model is ARIMAX v2, which includes all covariates.

---

### TBATS Model

A TBATS model was also tested to automatically capture complex seasonal patterns.

Specification:

TBATS(0.259,{2,2},-,{<52,6>})

While TBATS achieved competitive performance, it cannot incorporate external predictors and therefore lacks the interpretability of ARIMAX models.

---
<img width="984" height="553" alt="Screenshot 2026-03-13 at 2 34 04 PM" src="https://github.com/user-attachments/assets/b8ced69d-971e-4dee-becd-4f2ef9acba8b" />

## Key Findings

Several important insights emerge from the analysis:

- Influenza activity exhibits strong predictable seasonal structure  
- Structural breaks caused by COVID-19 significantly alter disease dynamics  
- Classical statistical models remain highly effective for epidemiological forecasting  
- Incorporating domain-specific covariates can substantially improve predictive accuracy  

---

## Repository Structure

seasonal-influenza-forecasting-models/

README.md  
influenza_forecasting.rmd  

---

## Technologies Used

- R  
- RMarkdown  
- Statistical time series modeling  
- SARIMA / ARIMAX models  

