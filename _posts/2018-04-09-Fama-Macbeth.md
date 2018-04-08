---
layout: post
title:  "Fama-Macbeth"
subtitle: "Fama-Macbeth-cross-sectional-regression"
date:   2018-04-09 05:04:13 -0400
background: '/img/posts/01.jpg'
---


```python
import pandas as pd
import numpy as np
from scipy import stats
import statsmodels.formula.api as sm
```


```python
market_data = pd.read_excel("D:\\제용\\금융공학 연구실\\학습용\\US_10_industry.xlsx", index_col=0)['Market']
stock_data = pd.read_excel("D:\\제용\\금융공학 연구실\\학습용\\US_10_industry.xlsx", index_col=0)
stock_data = stock_data.ix[:,'NoDur':'Other']
```


```python
market_data.tail()
```




    201608    0.0052
    201609    0.0027
    201610   -0.0200
    201611    0.0487
    201612    0.0184
    Name: Market, dtype: float64




```python
stock_data.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NoDur</th>
      <th>Durbl</th>
      <th>Manuf</th>
      <th>Enrgy</th>
      <th>HiTec</th>
      <th>Telcm</th>
      <th>Shops</th>
      <th>Hlth</th>
      <th>Utils</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201608</th>
      <td>-0.0062</td>
      <td>0.0029</td>
      <td>0.0160</td>
      <td>0.0139</td>
      <td>0.0176</td>
      <td>-0.0350</td>
      <td>-0.0144</td>
      <td>-0.0323</td>
      <td>-0.0392</td>
      <td>0.0326</td>
    </tr>
    <tr>
      <th>201609</th>
      <td>-0.0248</td>
      <td>-0.0164</td>
      <td>-0.0018</td>
      <td>0.0293</td>
      <td>0.0241</td>
      <td>0.0048</td>
      <td>-0.0068</td>
      <td>0.0036</td>
      <td>0.0175</td>
      <td>-0.0121</td>
    </tr>
    <tr>
      <th>201610</th>
      <td>-0.0014</td>
      <td>-0.0358</td>
      <td>-0.0263</td>
      <td>-0.0289</td>
      <td>-0.0109</td>
      <td>-0.0275</td>
      <td>-0.0397</td>
      <td>-0.0743</td>
      <td>-0.0061</td>
      <td>0.0059</td>
    </tr>
    <tr>
      <th>201611</th>
      <td>-0.0373</td>
      <td>0.0761</td>
      <td>0.0631</td>
      <td>0.0956</td>
      <td>0.0060</td>
      <td>0.0601</td>
      <td>0.0445</td>
      <td>0.0137</td>
      <td>-0.0284</td>
      <td>0.1083</td>
    </tr>
    <tr>
      <th>201612</th>
      <td>0.0349</td>
      <td>0.0383</td>
      <td>0.0053</td>
      <td>0.0213</td>
      <td>0.0084</td>
      <td>0.0471</td>
      <td>-0.0042</td>
      <td>0.0086</td>
      <td>0.0364</td>
      <td>0.0293</td>
    </tr>
  </tbody>
</table>
</div>




```python
def DF_liner_regression(data, market_data, times):
    def how_many_betas(data,times):
        repet_times = len(data)-times
        start_period = list(map(lambda x: x, range(times)))
        end_period = list(map(lambda x: x+repet_times, range(times)))
        return start_period, end_period    

    start_period, end_period = how_many_betas(data,times)
    
    DF_beta = pd.DataFrame()
    DF_intercept = pd.DataFrame()
    for j in range(times):
        beta = list()
        intercept_list = list()
        market = market_data[start_period[j]:end_period[j]+1]
        individual_stock = data.ix[data.index[start_period[j]]:data.index[end_period[j]],]
        for i in range(len(data.columns)):
            individual = individual_stock.ix[:,data.columns[i]]
            slope, intercept, r_value, p_value, std_err = stats.linregress(market,individual)
            beta.append(slope)
            intercept_list.append(intercept)
        DF_beta.loc[:,data.index[end_period[j]]] = beta
        DF_intercept.loc[:,data.index[end_period[j]]] = intercept_list 
    DF_beta.index = data.columns
    DF_intercept.index = data.columns
    return DF_beta.T, DF_intercept.T
```


```python
time_series_beta, time_series_intercept = DF_liner_regression(stock_data, market_data,50)
time_series_beta.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NoDur</th>
      <th>Durbl</th>
      <th>Manuf</th>
      <th>Enrgy</th>
      <th>HiTec</th>
      <th>Telcm</th>
      <th>Shops</th>
      <th>Hlth</th>
      <th>Utils</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201608</th>
      <td>0.714581</td>
      <td>1.223192</td>
      <td>1.027097</td>
      <td>0.727595</td>
      <td>1.337977</td>
      <td>0.918208</td>
      <td>0.952457</td>
      <td>0.784403</td>
      <td>0.419853</td>
      <td>1.065718</td>
    </tr>
    <tr>
      <th>201609</th>
      <td>0.715412</td>
      <td>1.225190</td>
      <td>1.027316</td>
      <td>0.727149</td>
      <td>1.336352</td>
      <td>0.916845</td>
      <td>0.952930</td>
      <td>0.784216</td>
      <td>0.421479</td>
      <td>1.065329</td>
    </tr>
    <tr>
      <th>201610</th>
      <td>0.715177</td>
      <td>1.225279</td>
      <td>1.026753</td>
      <td>0.728560</td>
      <td>1.335966</td>
      <td>0.917130</td>
      <td>0.953386</td>
      <td>0.788021</td>
      <td>0.422421</td>
      <td>1.063829</td>
    </tr>
    <tr>
      <th>201611</th>
      <td>0.711201</td>
      <td>1.226933</td>
      <td>1.027365</td>
      <td>0.732058</td>
      <td>1.333629</td>
      <td>0.917686</td>
      <td>0.952207</td>
      <td>0.785751</td>
      <td>0.418682</td>
      <td>1.066830</td>
    </tr>
    <tr>
      <th>201612</th>
      <td>0.711281</td>
      <td>1.226141</td>
      <td>1.026538</td>
      <td>0.734993</td>
      <td>1.332286</td>
      <td>0.919397</td>
      <td>0.950578</td>
      <td>0.785614</td>
      <td>0.419342</td>
      <td>1.066849</td>
    </tr>
  </tbody>
</table>
</div>




```python
def compute_error(real, beta, intercept, market):
    DF_error = pd.DataFrame()
    for i in range(len(beta.columns)):
        real_ind = real.ix[:, real.columns[i]]
        beta_ind = beta.ix[:, beta.columns[i]]
        intercept_ind = intercept.ix[:, intercept.columns[i]]
        error = real_ind-(beta_ind*market+intercept_ind)
        DF_error.loc[:, beta.columns.values[i]] = error
    return DF_error
```


```python
error_term = compute_error(stock_data.ix[stock_data.index[-50]:stock_data.index[-1],], time_series_beta, time_series_intercept, market_data.ix[market_data.index[-50]:market_data.index[-1],])
error_term.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NoDur</th>
      <th>Durbl</th>
      <th>Manuf</th>
      <th>Enrgy</th>
      <th>HiTec</th>
      <th>Telcm</th>
      <th>Shops</th>
      <th>Hlth</th>
      <th>Utils</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201608</th>
      <td>-0.014990</td>
      <td>-0.000642</td>
      <td>0.009631</td>
      <td>0.006980</td>
      <td>0.013531</td>
      <td>-0.040827</td>
      <td>-0.020900</td>
      <td>-0.040325</td>
      <td>-0.046847</td>
      <td>0.027893</td>
    </tr>
    <tr>
      <th>201609</th>
      <td>-0.031688</td>
      <td>-0.016728</td>
      <td>-0.005577</td>
      <td>0.024123</td>
      <td>0.023230</td>
      <td>0.001184</td>
      <td>-0.010868</td>
      <td>-0.002470</td>
      <td>0.010978</td>
      <td>-0.014138</td>
    </tr>
    <tr>
      <th>201610</th>
      <td>0.007918</td>
      <td>-0.008270</td>
      <td>-0.006673</td>
      <td>-0.017560</td>
      <td>0.018474</td>
      <td>-0.010258</td>
      <td>-0.022027</td>
      <td>-0.062442</td>
      <td>-0.003103</td>
      <td>0.027974</td>
    </tr>
    <tr>
      <th>201611</th>
      <td>-0.076769</td>
      <td>0.019404</td>
      <td>0.012133</td>
      <td>0.056666</td>
      <td>-0.056121</td>
      <td>0.014260</td>
      <td>-0.003369</td>
      <td>-0.028497</td>
      <td>-0.054233</td>
      <td>0.057035</td>
    </tr>
    <tr>
      <th>201612</th>
      <td>0.016926</td>
      <td>0.018647</td>
      <td>-0.014554</td>
      <td>0.004775</td>
      <td>-0.013376</td>
      <td>0.029104</td>
      <td>-0.023269</td>
      <td>-0.009765</td>
      <td>0.023224</td>
      <td>0.010324</td>
    </tr>
  </tbody>
</table>
</div>




```python
def make_DF(beta, error):
    one = np.ones((len(beta.index),1))
    beta_p = np.mean(beta, axis=1)
    beta_p_sq = beta_p**2
    error_term_p = np.mean(error, axis=1)
    DF = pd.DataFrame()
    DF.loc[:, 'beta_p'] = beta_p
    DF.loc[:, 'beta_p_sq'] = beta_p_sq
    DF.loc[:, 'error_term_p'] = error_term_p
    DF.loc[:, 'intercept'] = one
    return DF
```


```python
real = np.mean(stock_data.ix[stock_data.index[-50]:stock_data.index[-1],], axis=1)
parameter_DF = make_DF(time_series_beta, error_term)

parameter_DF.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>beta_p</th>
      <th>beta_p_sq</th>
      <th>error_term_p</th>
      <th>intercept</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201608</th>
      <td>0.917108</td>
      <td>0.841087</td>
      <td>-0.010650</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>201609</th>
      <td>0.917222</td>
      <td>0.841296</td>
      <td>-0.002195</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>201610</th>
      <td>0.917652</td>
      <td>0.842086</td>
      <td>-0.007597</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>201611</th>
      <td>0.917234</td>
      <td>0.841319</td>
      <td>-0.005949</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>201612</th>
      <td>0.917302</td>
      <td>0.841442</td>
      <td>0.004204</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
model = sm.OLS(endog=real, exog=parameter_DF).fit()
model.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>            <td>y</td>        <th>  R-squared:         </th> <td>   0.196</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.143</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   3.736</td>
</tr>
<tr>
  <th>Date:</th>             <td>Mon, 09 Apr 2018</td> <th>  Prob (F-statistic):</th>  <td>0.0174</td> 
</tr>
<tr>
  <th>Time:</th>                 <td>04:50:00</td>     <th>  Log-Likelihood:    </th> <td>  110.54</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>    50</td>      <th>  AIC:               </th> <td>  -213.1</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>    46</td>      <th>  BIC:               </th> <td>  -205.4</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     3</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th> <th>[95.0% Conf. Int.]</th> 
</tr>
<tr>
  <th>beta_p</th>       <td> 1120.8399</td> <td> 1155.955</td> <td>    0.970</td> <td> 0.337</td> <td>-1205.977  3447.657</td>
</tr>
<tr>
  <th>beta_p_sq</th>    <td> -613.6551</td> <td>  631.815</td> <td>   -0.971</td> <td> 0.336</td> <td>-1885.433   658.123</td>
</tr>
<tr>
  <th>error_term_p</th> <td>    1.9918</td> <td>    0.778</td> <td>    2.560</td> <td> 0.014</td> <td>    0.426     3.558</td>
</tr>
<tr>
  <th>intercept</th>    <td> -511.7790</td> <td>  528.722</td> <td>   -0.968</td> <td> 0.338</td> <td>-1576.042   552.484</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 0.435</td> <th>  Durbin-Watson:     </th> <td>   2.526</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.804</td> <th>  Jarque-Bera (JB):  </th> <td>   0.070</td>
</tr>
<tr>
  <th>Skew:</th>          <td>-0.065</td> <th>  Prob(JB):          </th> <td>   0.966</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 3.130</td> <th>  Cond. No.          </th> <td>5.79e+05</td>
</tr>
</table>




```python

```


```python
def rolling_window_cal(bata_data, error_data, times):
    start_period, end_period = how_many_betas(bata_data, times)
    
    for i in range(len(start_period)):
        beta = beta_data[start_period[i]:end_period[i]+1]
        error = error_data[start_period[i]:end_period[i]+1]
        beta_p = np.mean(beta)
        beta_p_sq = beta_p**2
        error_term_p = np.mean(error)
```




    NoDur    0.515613
    Durbl    1.458385
    Manuf    1.053101
    Enrgy    0.540175
    HiTec    1.781652
    Telcm    0.813226
    Shops    0.925892
    Hlth     0.608395
    Utils    0.184544
    Other    1.139052
    dtype: float64




```python

        
```


```python

```
