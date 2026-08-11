# ARIMAX

## What model does

This model adds onto traditional ARIMA models by allowing exogenous variables to impact predictions. This framework allows for additional factors to enhance forecasting accuracy, such as economic indicators.

## Assumptions

# ARIMAX Model Assumptions

ARIMAX (AutoRegressive Integrated Moving Average with Exogenous Variables) combines a regression model with an ARIMA error process.

The general form is:

$$
y_t = \beta_0 + \mathbf{x}_t^\top\boldsymbol{\beta} + n_t
$$

where the error term follows an ARIMA process:

$$
\phi(B)(1-B)^d n_t = \theta(B)\epsilon_t
$$

where:

- $y_t$ = dependent variable being forecast
- $\mathbf{x}_t$ = vector of exogenous variables
- $\boldsymbol{\beta}$ = regression coefficients
- $n_t$ = ARIMA error process
- $\epsilon_t$ = random error
- $p$ = autoregressive order
- $d$ = degree of differencing
- $q$ = moving-average order

---

## 1. The relationship between predictors and the target is reasonably specified

ARIMAX assumes that the relationship between the dependent variable and the exogenous variables can be reasonably represented by the regression component:

$$
y_t = \beta_0+ \beta_1x_{1,t}+ \beta_2x_{2,t}+ \cdots+ \beta_kx_{k,t} +n_t
$$

The relationship does not necessarily have to be perfectly linear, but the
standard ARIMAX formulation assumes a linear relationship in the parameters.

⚠️ Limitation - If the true relationship is strongly nonlinear, a standard ARIMAX model may not capture it adequately.


## 2. Exogenous variables must be available for the forecast horizon

This is one of the most important practical assumptions for forecasting.

If:

$$
Revenue_t=f(Employment_t,Income_t,RetailSales_t)
$$

then future values of:

- $Employment_t$
- $Income_t$
- $RetailSales_t$

must be known or forecast separately when producing future revenue forecasts.

For municipal forecasting, this means that an ARIMAX model using economic
indicators may require a separate forecast or scenario for those indicators.


## 3. The regression errors follow an ARIMA process

After accounting for the exogenous variables, the remaining error should be adequately modeled by ARIMA:

$$
n_t \sim ARIMA(p,d,q)
$$

This means the residual process should be approximately stationary after
appropriate differencing and should have a structure that can be represented
through autoregressive and moving-average components.


## 4. The regression errors should be approximately uncorrelated after modeling

$$
Corr(\epsilon_t,\epsilon_{t-k})\approx0
$$

for relevant lags $k$.

This can be evaluated using:

- ACF
- PACF

## 5. The regression errors should have approximately constant variance

Ideally:

$$
Var(\epsilon_t)\approx\sigma^2
$$

Strong changes in error variance over time may indicate:

- heteroskedasticity
- structural changes
- outliers
- an inappropriate transformation
- model misspecification


## 6. The errors should have approximately zero mean

After accounting for the regression and ARIMA components:

$$
E[\epsilon_t]\approx0
$$


## 7. The dependent variable and predictors must be appropriately treated for non-stationarity

ARIMAX does not automatically eliminate the problems associated with unrelated
trending variables.

For example, suppose:

$$
Revenue_t \uparrow
$$

and:

$$
Employment_t \uparrow
$$

simply because both variables have long-term upward trends.

A strong regression relationship could appear even if employment has little
short-run relationship with revenue.

This creates a potential risk of spurious regression.

Depending on the data-generating process, variables may therefore need to be:

- differenced
- transformed
- expressed as growth rates
- expressed as percentage changes
- detrended
- modeled in levels when a long-run relationship is theoretically justified

The appropriate transformation should be determined based on the economic
relationship and time-series properties rather than applied automatically.


## 8. Predictors should have a meaningful relationship with the target

Exogenous variables should have an economic or substantive rationale for inclusion.

## 9. Multicollinearity among predictors should be limited

If predictors are highly correlated:

$$
Corr(X_1,X_2)\approx1
$$

then individual coefficient estimates may become unstable.

This does not necessarily prevent ARIMAX from forecasting accurately, but it can
make coefficient interpretation unstable and increase sensitivity to model
specification.


## 10. Predictors should not contain future information

ARIMAX must respect the information available at the forecast origin.

For example, when backtesting a 2022 forecast, the model should only use
economic information that would actually have been available when the 2022
forecast was made.

This is known as information leakage or look-ahead bias.

## 11. The relationship between predictors and revenue should be reasonably stable

ARIMAX assumes that the estimated relationship:

$$
\beta_k
$$

is reasonably representative of the forecasting period.

Structural changes can violate this assumption.

Examples in municipal finance include:

- tax-rate changes
- changes to the tax base
- new legislation
- elimination of a revenue source
- changes in collection practices
- changes in accounting definitions
- major economic shocks

In such cases, consider:

- intervention variables
- structural breaks
- separate modeling periods
- interaction terms
- BSTS
- other structural models

## 12. Residual normality is useful but not strictly required for forecasting

ARIMAX does not require the raw dependent variable or predictors to be
normally distributed.

Normality of residuals is primarily relevant for conventional statistical
inference and prediction intervals.

For forecasting, more important conditions are:

- approximately zero-mean errors
- little remaining autocorrelation
- reasonably stable variance
- good out-of-sample performance

Residual normality can still be assessed using Q-Q plots and other diagnostics.

# ARIMAX Diagnostic Checklist

After fitting an ARIMAX model, evaluate:

| Diagnostic | Desired result |
|:---|:---|
| Target stationarity | Appropriate transformation/differencing |
| Predictor treatment | Appropriate levels/differences/growth rates |
| Residual mean | Approximately 0 |
| Residual ACF | Little/no remaining autocorrelation |
| Residual PACF | Little/no remaining structure |
| Ljung-Box test | Fail to reject independence |
| Residual variance | Approximately stable |
| Residual normality | Approximately normal if required for inference |
| Multicollinearity | Limited/manageable |
| Structural breaks | None substantial or explicitly modeled |
| Predictor availability | Known or forecastable |
| Information leakage | None |
| Economic interpretation | Plausible |
| Out-of-sample performance | Better than appropriate baseline |


# ARIMAX vs. ARIMA

ARIMAX gives you a way to answer an interesting municipal-finance question:

> **How much of the movement in this revenue stream can be explained by
economic conditions, while accounting for the revenue series' own historical
dynamics?**

## R implementation

To see how this method is applied in R, read [TSA documentation](https://cran.r-project.org/web/packages/TSA/index.html) here.

## Learn More
Read about [ARIMAX](https://www.geeksforgeeks.org/artificial-intelligence/what-is-an-arimax-model/) here.

