# Model Comparisons

## Preprocessing and Assumptions

|Model|Stationary|Differencing|Moving Average|Scaling & Normalization|
|---|---|---|---|---|
|ETS|⛔|⛔|⛔|⛔|
|ARIMA|✅Yes, after differencing|✅|✅|⛔|
|SARIMA|✅Yes, after differencing|🟡Regular or seasonal|🟡Regular + seasonal MA terms|⛔|
|ARIMAX|✅Yes, for stochastic components|🟡As needed|✅|🟡Scaling usually required|
|BSTS|⛔|🟡Optional|🟡Optional|⛔|


