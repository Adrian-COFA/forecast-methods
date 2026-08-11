# ETS

## What model does

ETS models are a family of time series models with an underlying state space model consisting of a level component, a trend component (T), a seasonal component (S) and an error term (E).

The simplest of these models is known as _simple exponential smoothing_ (ANN model). This has additive errors, no trend, and no seasonality.

In the additive Holt-Winters' method, the seasonal component is added to the rest (AAA model).

## Assumptions

### 1. The series has a systematic level, trend, and/or seasonal pattern

The observed value can be represented approximately as:

$$
y_t = f(\ell_t, b_t, s_t) + \epsilon_t
$$

where:

- $\ell_t$ = level
- $b_t$ = trend
- $s_t$ = seasonal component
- $\epsilon_t$ = random error

The model does not require the series to be stationary.

### 2. The underlying pattern evolves gradually over time

Holt-Winters assumes that the level, trend, and seasonal components can change
over time, but generally not in an entirely unpredictable manner.

For example:

$$
\ell_t \approx \ell_{t-1}
$$

and:

$$
b_t \approx b_{t-1}
$$

unless the data provide evidence for a change.

⚠️Limitation - Large structural breaks can therefore reduce forecast accuracy.


### 3. Seasonality is reasonably stable

For a seasonal Holt-Winters model, the seasonal pattern is assumed to repeat
over a known seasonal period.

For example, with monthly data:

$$
s = 12
$$

The model assumes that the relationship between January, February, March, etc. is reasonably persistent over time.

This assumption may be violated if seasonal patterns change substantially between years.

### 4. Errors are approximately unpredictable

After accounting for level, trend, and seasonality, the remaining errors should
ideally resemble random noise.

$$
\epsilon_t = y_t-\hat{y}_t
$$

A good model should therefore leave residuals with:

- approximately zero mean
- little or no autocorrelation
- approximately constant variance
- no obvious remaining trend or seasonality

### 5. Future behavior is reasonably related to historical behavior

Holt-Winters is primarily a univariate forecasting method.

It assumes that the patterns contained in the historical series provide useful
information about its future behavior.


⚠️Limitation - It does not directly incorporate external economic variables such as:

- employment
- personal income
- retail sales
- inflation
- population
- interest rates

unless these are incorporated through a separate modeling framework.

## R implementation

## Model Evaluation

