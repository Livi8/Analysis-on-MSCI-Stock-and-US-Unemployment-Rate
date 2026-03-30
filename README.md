# ANALYSIS OF MICROSOFT STOCK AND THE UNEMPLOYMENT RATE IN THE UNITED STATES  


---

## Table of Contents

1. [INTRODUCTION](#1-introduction)  
2. [METHODOLOGY](#2-methodology)  
   - [2.1 Box-Jenkins framework](#21-box-jenkins-framework)  
   - [2.2 Model building method](#22-model-building-method)  
   - [2.3 Statistical tests](#23-statistical-tests)  
   - [2.4 Model Selection](#24-model-selection)  
3. [DATA AND DESCRIPTIVE ANALYSIS](#3-data-and-descriptive-analysis)  
   - [3.1 Microsoft stock returns](#31-microsoft-stock-returns)  
   - [3.2 US unemployment rate](#32-us-unemployment-rate)  
4. [MODELS AND PREDICTIONS](#4-models-and-predictions)  
   - [4.1 Models and prediction for Microsoft log-return](#41-models-and-prediction-for-microsoft-log-return)  
   - [4.2 Models and prediction for the unemployment rate in the US](#42-models-and-prediction-for-the-unemployment-rate-in-the-us)  
5. [CONCLUSION](#5-conclusion)

---

# INTRODUCTION

In this paper, I examine two different indicators: the stock price of Microsoft (MSFT), one of the most influential technology companies in global markets, and the unemployment rate of the United States, which is commonly used to assess the overall condition of the economy.

To analyze these two series, I apply the Box–Jenkins methodology, a systematic approach for building and validating ARIMA models.

The study of Microsoft’s stock price connects directly to the ongoing debate surrounding the Efficient Market Hypothesis (EMH). EMH suggests that asset prices immediately incorporate all available information, meaning that future price changes cannot be predicted from past data.

A typical way to evaluate this claim is to analyze log-returns instead of raw prices, since log-returns help reveal whether a series behaves like white noise.

Fama and French (2007) confirm that expected stock returns are systematically related to book-to-market ratios, profitability, and investment decisions, providing theoretical foundation for analyzing technology companies where innovation cycles may create temporary efficiency deviations.

Forecasting the unemployment rate is another important task, not only for policymakers but also for businesses that depend on labor market expectations.

While advanced macroeconomic models (e.g. the Diamond–Mortensen–Pissarides model) capture crisis impacts effectively (Malak Kandoussi & François Langot.,2024), in this analysis, the unemployment rate is modeled strictly as a univariate process without any exogenous variables using ARIMA model.

However, model performance may be affected by structural breaks and economic shocks.

My two main research questions are:

1. Does Microsoft’s stock price align with the Efficient Market Hypothesis when examined through its log-returns, so is the transformed time series white noise?
2. Do past values of the US unemployment rate provide meaningful information for forecasting its short-term movements?

---

# METHODOLOGY

## Box-Jenkins framework

Step 1: test for stationarity, if it is unit root, make it stationary  
Step 2: Based on correlogram decide on the possible ARIMA models  
Step 3: Estimate the models; the error of these should be white noise  
Step 4: Choose the best model based on AIC and BIC  
Step 5: Forecast future out-of-world sample values  

---

## Model building method

The ARIMA(p,d,q) model specification is:

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/formula.png" width="420">
</p>

Where:

- c is a constant  
- p is the order of the autoregressive part (number of AR lags)  
- φᵢ are autoregressive parameters  
- q is the order of the moving-average part (number of MA lags)  
- θⱼ are moving average parameters  
- d is the degree of differencing  
- εₜ represents white noise error term with 0 mean and with σ^2 variance  

---

## Statistical tests

I used 5% significant level to make decisions.

Tests that I use:

- Augmented Dickey-Fuller (ADF) test: tests null hypothesis of unit root  
- Kwiatkowski-Phillips-Schmidt-Shin (KPSS) test: its null hypothesis is that the process is stationary  
- Breusch-Godfrey test: tests for autocorrelation up to a given lag (H₀: no autocorrelation up to a given lag)  
- ARCH test: it is for conditional heteroskedasticity (H₀: no ARCH effects)  
- Jarque-Bera test: it is for normality (H₀: it is normally distributed)  

---

## Model selection

To compare the models, I use:

- Akaike Information Criterion (AIC): penalizes model complexity more heavily  
- Bayesian Information Criterion (BIC): applies stronger penalty for additional parameters  

---

# DATA AND DESCRIPTIVE ANALYSIS

## Microsoft stock returns

I used the daily closing prices for Microsoft (MSFT) from January 1, 2020, to December 19, 2025, providing 1496 observations. The data source is Yahoo Finance website.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture11.png" width="750">
</p>

The stock price series exhibits the typical characteristics of financial time series, including an upward trend over the 5 years and increased volatility during the COVID-19 pandemic period. I can also see some structural breaks.

One potential reason for the structural break in early 2023 could be the market's shift toward a faster growth phase. This change suggests that the sudden popularity of AI tools might have been the key factor that drove Microsoft's stock price to new highs during that time (Goldmansachs.com, 2025).

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/t1.png" width="500">
</p>

The statistic reveals positive skewness: the average price ($321.24) is higher than the median ($303.04), and prices ranged from a low of $135.42 to a high of $542.07 during the period.

Both tests confirm non-stationarity of the raw stock price series. The ADF test fails to reject the unit root hypothesis (p-value = 41%), while the KPSS test strongly rejects stationarity (p-value < 1%), supporting the random walk hypothesis typical of financial time series.

To analyze the behavior of the log-return, I first compute the natural logarithm of the closing price and then apply first differencing.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture12.png" width="750">
</p>

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/t2.png" width="500">
</p>

The mean return is approximately zero, which is consistent with the Efficient Market Hypothesis.

Both the ADF and KPSS tests confirm stationarity of the log returns series, with p-values of less than 1% and greater than 10% respectively.

The negative skewness suggests an asymmetric risk profile where larger negative outliers are more likely to occur compared to positive outliers.

To investigate the Efficient Market Hypothesis, I examine whether the log-returns follow a white noise process. If they do, it will mean there's no predictable information in the data.

The Breusch-Godfrey test shows a p-value of less than 1%, so I reject the null hypothesis of no autocorrelation up to 10 lags. This means the data is not white noise.

Therefore, I build an ARIMA model to extract the information from the data. The correlogram of the series provides the starting point for this search.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture13.png" width="700">
</p>

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture14.png" width="700">
</p>

Based on the correlogram the possible models are:

- ARIMA(1,1,0)
- ARIMA(0,1,1)
- ARIMA(1,1,1)

---

## US unemployment rate

Monthly unemployment rate data from the FRED (Federal Reserve Bank of St. Louis) spans from January 2019 to September 2025, yielding 81 observations.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture15.png" width="750">
</p>

This series captures the dramatic impact of the COVID-19 pandemic on labor markets, with unemployment rates peaking at 14.8% in April 2020 before gradually declining to pre-pandemic levels.

After the pandemic shock faded, unemployment rate exhibits a modest upward trend.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/t3.png" width="500">
</p>

The ADF test suggests a unit root, while KPSS shows borderline stationarity. Here the p-value is 6.8% so at my alpha level 5% I fail to reject the H0, stating it is stationary.

But only by looking at Figure 5 I can see that it is not a stationary process for sure, so I decided to take the first difference.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture16.png" width="750">
</p>

Based on the plot I would not say it is stationary, so I take the second difference.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture17.png" width="750">
</p>

With stationarity now established (both tests suggest that), I turn to the correlogram to identify the appropriate ARIMA pattern.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture18.png" width="700">
</p>

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture19.png" width="700">
</p>

The possible models are:

- ARIMA(0,2,1)
- ARIMA(1,1,0)
- ARIMA(2,2,0)
- ARIMA(1,2,1)
- ARIMA(2,2,1)

---

# MODELS AND PREDICTIONS

## Models and prediction for Microsoft log-return

### Model selection

I used the Breusch-Godfrey test to check the autocorrelation of the residuals.

In case of ARIMA(1,0,0) the p-value is almost 0 so I reject H0 which states there is no autocorrelation up to 10 lags. So, I drop this model.

For the other two models I got the same result, meaning: I could not build an ARIMA model with white noise error term.

But why?

The reason can be that the variance of the log-return is clustered. So the differencing removes the unit root in the mean, yet it does nothing to smooth the variance.

Let’s run an ARCH test to identify if I have an ARCH effect in the time series or not.

The H0 is that I do not have ARCH effect up to 12 lags.

The p is almost 0 so I reject the null hypothesis, I have indeed ARCH effect, meaning that shocks affect the variability of the series rather than its expected level, highlighting the limitations of ARIMA models in capturing volatility.

To make prediction I use ARIMA(3,1,3).

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/t4.png" width="500">
</p>

### Prediction

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture24.png" width="750">
</p>

My model predicts that Microsoft stock prices will go up, which matches the other tech companies stock prices’ trend. However, the wide confidence intervals show there is a lot of uncertainty in my forecast.

---

## Models and prediction for the unemployment rate in the US

### Model selection

Based on the Breusch-Godfrey tests I drop the ARIMA(1,2,0), ARIMA(2,2,0) models.

I compare the other models with non-autocorrelated residuals based on AIC and BIC.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/t5.png" width="500">
</p>

According to the AIC and BIC the best model is MA1 (ARIMA(0,2,1)) so I use it for the forecast.

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/t6.png" width="500">
</p>

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture21.png" width="750">
</p>

Despite residual plot shows the huge outlier and the Jarque-Bera test rejects normality (p < 0.01), I will use it for the forecast.

Moreover, the ARCH test confirms the presence of conditional heteroskedasticity (p is almost 0), indicating that the residuals exhibit time-varying volatility.

### Prediction

<p align="center">
<img src="https://github.com/Livi8/Analysis-on-MSCI-Stock-and-US-Unemployment-Rate/blob/main/data/Picture22.png" width="750">
</p>

The forecast indicates that the unemployment rate is expected to remain stable over the forecast horizon, with the point estimate around the expected value.

This suggests that, under the model’s assumptions, no significant upward or downward trend is anticipated in the near term, but the confidence interval allow uncertainty in my forecast.

---

# CONCLUSION

The Microsoft log returns are not white noise, which means the Efficient Market Hypothesis is not supported by this data.

This indicates that the stock returns cannot be reliably forecasted using only the current data.

In contrast, the unemployment rate model produces stable forecasts, though the confidence intervals suggest potential increases up to 13% and decreases down to 0%.

---

# References

Fama, E. F., & French, K. R. (2006). Profitability, investment and average returns. Journal of Financial Economics, 82(3), 491–518.  
https://doi.org/10.1016/j.jfineco.2005.09.009  

Malak Kandoussi, & François Langot. (2024). Modeling and evaluating the heterogeneous impacts of the COVID-19 on US unemployment. Economic Modelling, 106978–106978.  
https://doi.org/10.1016/j.econmod.2024.106978  

Gans, J. S. (2025). The efficient market hypothesis when time travel is possible. Economics Letters, 248, 112209.  
https://doi.org/10.1016/j.econlet.2025.112209  

(2025). Goldmansachs.com.  
https://www.goldmansachs.com/pdfs/insights/goldman-sachs-research/ai-in-a-bubble/report.pdf
