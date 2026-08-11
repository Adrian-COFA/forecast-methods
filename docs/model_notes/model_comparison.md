# Model Comparisons

## Preprocessing and Assumptions

|Model|Stationary|Differencing|Moving Average|Scaling & Normalization|
|---|---|---|---|---|
|ETS|⛔|⛔|⛔|⛔|
|ARIMA|✅Yes, after differencing|✅|✅|⛔|
|SARIMA|✅Yes, after differencing|🟡Regular or seasonal|🟡Regular + seasonal MA terms|⛔|
|ARIMAX|✅Yes, for stochastic components|🟡As needed|✅|🟡Scaling usually required|
|BSTS|⛔|🟡Optional|🟡Optional|⛔|

## Formulas

### 1. Exponential Smoothing (ETS)
The general exponential smoothing framework decomposes a time series into
level, trend, and seasonal components.

$$
y_t = \text{Level}_t + \text{Trend}_t + \text{Seasonality}_t + \epsilon_t
$$

The specific ETS formulation depends on whether the model includes:

- Error: additive or multiplicative
- Trend: none, additive, or multiplicative
- Seasonality: none, additive, or multiplicative

A simple exponential smoothing model can be represented as:

$$
\hat{y}_{t+h|t} = \ell_t
$$

where:

$$
\ell_t = \alpha y_t + (1-\alpha)\ell_{t-1}
$$

and:

- $\ell_t$ = estimated level at time $t$
- $\alpha$ = smoothing parameter
- $y_t$ = observed value
- $\hat{y}_{t+h|t}$ = forecast $h$ periods ahead

---

### 2. ARIMA

An ARIMA model is represented as:

$$
ARIMA(p,d,q)
$$

The generalized ARIMA equation is:

$$
\phi(B)(1-B)^d y_t = c + \theta(B)\epsilon_t
$$

where:

- $p$ = autoregressive order
- $d$ = degree of differencing
- $q$ = moving-average order
- $B$ = backshift operator
- $\phi(B)$ = autoregressive polynomial
- $\theta(B)$ = moving-average polynomial
- $\epsilon_t$ = error term
- $c$ = constant/drift term when applicable

The autoregressive component can be written as:

$$
\phi(B) = 1-\phi_1B-\phi_2B^2-\cdots-\phi_pB^p
$$

The moving-average component is:

$$ \theta(B) = 1+\theta_1B+\theta_2B^2+\cdots+\theta_qB^q
$$

First-order differencing is:

$$
\Delta y_t = y_t-y_{t-1}
$$

---

### 3. SARIMA

SARIMA extends ARIMA to incorporate seasonal dynamics:

$$
SARIMA(p,d,q)(P,D,Q)_s
$$

The generalized formulation is:

$$
\Phi(B^s)\phi(B)(1-B)^d(1-B^s)^D y_t = c+\Theta(B^s)\theta(B)\epsilon_t
$$

where:

- $p$ = non-seasonal AR order
- $d$ = non-seasonal differencing
- $q$ = non-seasonal MA order
- $P$ = seasonal AR order
- $D$ = seasonal differencing
- $Q$ = seasonal MA order
- $s$ = seasonal period
- $\Phi(B^s)$ = seasonal autoregressive polynomial
- $\Theta(B^s)$ = seasonal moving-average polynomial

Seasonal differencing is:

$$
\Delta_s y_t = y_t-y_{t-s}
$$

For monthly data:

$$
s=12
$$

For quarterly data:

$$
s=4
$$

---

### 4. ARIMAX

ARIMAX combines a regression model with ARIMA errors.

The generalized formulation is:

$$
y_t = \beta_0 + \beta_1x_{1,t} + \beta_2x_{2,t} +\cdots+ \beta_kx_{k,t} + n_t
$$

where the error term follows an ARIMA process:

$$
\phi(B)(1-B)^d n_t = \theta(B)\epsilon_t
$$

Therefore, the complete model can be represented as:

$$
y_t = \beta_0+\mathbf{x}_t^\top\boldsymbol{\beta}+n_t
$$

with:

$$
\phi(B)(1-B)^d n_t = \theta(B)\epsilon_t
$$

where:

- $y_t$ = revenue being forecast
- $\mathbf{x}_t$ = vector of explanatory variables
- $\boldsymbol{\beta}$ = regression coefficients
- $n_t$ = ARIMA error process
- $x_{k,t}$ = external/economic predictors

For example:

$$ Revenue_t = \beta_0 + \beta_1Employment_t + \beta_2PersonalIncome_t + \beta_3RetailSales_t + n_t
$$

---

### 5. Bayesian Structural Time Series (BSTS)

BSTS represents a time series as a combination of latent structural components and
optional regression variables.

A generalized BSTS model can be written as:

$$
y_t =
\mu_t
+
\tau_t
+
\gamma_t
+
\mathbf{x}_t^\top\boldsymbol{\beta}
+
\epsilon_t
$$

where:

- $\mu_t$ = local level component
- $\tau_t$ = trend component
- $\gamma_t$ = seasonal component
- $\mathbf{x}_t^\top\boldsymbol{\beta}$ = regression component
- $\epsilon_t$ = observation error

A local level component can be represented as:

$$
y_t = \mu_t+\epsilon_t
$$

with:

$$
\mu_t=\mu_{t-1}+\eta_t
$$

where:

$$
\epsilon_t\sim N(0,\sigma_\epsilon^2)
$$

and:

$$
\eta_t\sim N(0,\sigma_\eta^2)
$$

A local linear trend can be represented as:

$$
y_t=\mu_t+\epsilon_t
$$

$$
\mu_t=\mu_{t-1}+\delta_{t-1}+\eta_t
$$

$$
\delta_t=\delta_{t-1}+\zeta_t
$$

where:

- $\mu_t$ = time-varying level
- $\delta_t$ = time-varying slope/trend
- $\eta_t$ = level disturbance
- $\zeta_t$ = slope disturbance

The Bayesian framework places prior distributions on the unknown parameters and
latent states and estimates their posterior distributions given the observed data.
