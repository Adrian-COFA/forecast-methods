# ARIMA, SARIMA

## What model does

SARIMA is an extension of ARIMA. Both assumptions are described below. SARIMA models capture short and long-term dependencies within time series data. This model decomposes the series into trend, seasonal, and irregular components, models each separately, and recombines them for prediction.

## Assumptions

### 1. ARIMA

### 1.1 Stationarity after differencing

The differenced series should be approximately stationary.

For a first-order difference:

$$
\Delta y_t = y_t-y_{t-1}
$$

Stationarity means that the statistical properties of the series are reasonably stable over time, particularly:

- constant or stable mean
- constant or stable variance
- autocovariance that depends primarily on the lag rather than the point in time


### 1.2 Appropriate degree of differencing

The value of $d$ should be sufficient to remove non-stationary trend without excessively differencing the series.

Ideally:

$$
d = \min\{d : \Delta^d y_t \text{ is approximately stationary}\}
$$

Over-differencing can introduce unnecessary noise and induce artificial
negative autocorrelation.

### 1.3 Linear dependence structure

ARIMA assumes that the predictable component of the series can be reasonably
represented through a linear combination of past observations and past errors.

The autoregressive component is:

$$
y_t = c+\phi_1y_{t-1}+\phi_2y_{t-2}+\cdots+\phi_py_{t-p}+\epsilon_t
$$

with the moving-average component representing dependence on previous errors.

This means ARIMA is primarily a model of linear temporal dependence.

### 1.4 Residual independence

After fitting the model, the residuals should contain little remaining serial dependence.

Ideally:

$$
Corr(\epsilon_t,\epsilon_{t-k}) \approx 0
$$

for all relevant lags $k>0$.

In practice, residual autocorrelation can be examined using:

- ACF plots
- PACF plots

### 1.5 Residual mean approximately zero

The residuals should be centered approximately around zero:

$$
E[\epsilon_t]\approx0
$$

### 1.6 Constant error variance

ARIMA generally assumes that the variance of the errors is reasonably stable:

$$
Var(\epsilon_t)\approx\sigma^2
$$

Strong heteroskedasticity can reduce the reliability of standard inference and prediction intervals.

### 1.7 Normally distributed errors are not strictly required for forecasting

ARIMA does not require the raw observations to be normally distributed.

Normality of the residuals is primarily important for exact likelihood-based inference and conventional prediction intervals.

For forecasting, the more important assumptions are that residuals are approximately:

- zero mean
- uncorrelated
- reasonably stable in variance

### 1.8 No major unmodeled structural breaks

ARIMA assumes that the underlying relationship is reasonably stable over the forecasting period.

⚠️ Limitation - Large structural changes can violate this assumption.

Examples in municipal revenue include:

- tax-rate changes
- creation or elimination of a tax
- major legislative changes
- changes in revenue definitions
- major economic shocks
- changes in accounting methodology

A structural break may require an intervention, a different model period, or a structural model such as __BSTS__.

---

## 2. SARIMA

SARIMA extends ARIMA by explicitly modeling seasonal dependence:

### 2.1 All major ARIMA assumptions still apply

SARIMA retains the core ARIMA assumptions:

- stationarity after appropriate differencing
- appropriate degree of differencing
- linear temporal dependence
- approximately uncorrelated residuals
- approximately zero-mean residuals
- reasonably stable error variance
- no major unmodeled structural breaks

### 2.2 Seasonal stationarity

SARIMA additionally assumes that seasonal non-stationarity has been adequately addressed.

Seasonal differencing is:

$$
\Delta_s y_t=y_t-y_{t-s}
$$

For monthly data:

$$
\Delta_{12}y_t=y_t-y_{t-12}
$$

The resulting series should have reasonably stable seasonal behavior.


### 2.3 Stable seasonal structure

SARIMA assumes that the seasonal relationship is reasonably consistent over time.

If the seasonal pattern changes substantially over time, a standard SARIMA
model may perform poorly.

### 2.4 Seasonal dependence is adequately captured

SARIMA assumes that any important seasonal autocorrelation can be represented through the seasonal AR and MA terms.

For example:

$$
Y_t \leftarrow Y_{t-12}
$$

for monthly data.

If substantial seasonal autocorrelation remains in the residuals, the seasonal specification may be inadequate.

### 2.5 Residuals should not contain remaining seasonal structure

After fitting SARIMA, the residuals should ideally have:

$$
ACF(k)\approx0
$$

including at seasonal lags.

For monthly data, pay particular attention to:

$$
k=12,24,36,\ldots
$$

A significant spike at lag 12 may indicate that the model has not adequately
captured annual seasonality.

---

# 3. Model Diagnostic Summary

After fitting either ARIMA or SARIMA, the following diagnostic conditions should
ideally hold:

| Diagnostic | Desired result |
|:---|:---|
| Stationarity | Achieved after differencing |
| Residual mean | Approximately 0 |
| Residual ACF | No significant remaining autocorrelation |
| Residual PACF | No significant remaining structure |
| Ljung-Box test | Fail to reject residual independence |
| Residual variance | Approximately stable |
| Residual distribution | Approximately normal if relying on conventional inference |
| Structural breaks | None substantial or explicitly modeled |
| Seasonal residuals | None for a well-specified SARIMA |
| Forecast bias | Approximately 0 in backtesting |

## R implementation

See [forecast](https://www.rdocumentation.org/packages/forecast/versions/9.0.1) package documentation to see how methods are applied.

## Learn more

Learn more about [ARIMA](https://www.geeksforgeeks.org/machine-learning/model-selection-for-arima/) here. Learn more about [SARIMA](https://www.geeksforgeeks.org/machine-learning/sarima-seasonal-autoregressive-integrated-moving-average/) here.


