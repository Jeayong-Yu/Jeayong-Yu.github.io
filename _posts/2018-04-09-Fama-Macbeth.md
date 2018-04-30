---
layout: post
title:  "[산업응용확률 및 통계] Fama-MacBeth in Python"
subtitle: "Fama-MacBeth의 two-step regression을 활용한 two-parameter model의 증명"
date:   2018-04-09 06:07:13 -0400
background: '/img/posts/01.jpg'
---


```python
import pandas as pd
import numpy as np
from scipy import stats
import statsmodels.formula.api as sm
```


```python
market_data = pd.read_excel("D:\\제용\\금융공학 연구실\\학습용\\US_10_industry_FM.xlsx", index_col=0)['Market']
stock_data = pd.read_excel("D:\\제용\\금융공학 연구실\\학습용\\US_10_industry_FM.xlsx", index_col=0)
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
      <th>Telcm</th>
      <th>Shops</th>
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
      <td>-0.0350</td>
      <td>-0.0144</td>
      <td>0.0326</td>
    </tr>
    <tr>
      <th>201609</th>
      <td>-0.0248</td>
      <td>-0.0164</td>
      <td>-0.0018</td>
      <td>0.0293</td>
      <td>0.0048</td>
      <td>-0.0068</td>
      <td>-0.0121</td>
    </tr>
    <tr>
      <th>201610</th>
      <td>-0.0014</td>
      <td>-0.0358</td>
      <td>-0.0263</td>
      <td>-0.0289</td>
      <td>-0.0275</td>
      <td>-0.0397</td>
      <td>0.0059</td>
    </tr>
    <tr>
      <th>201611</th>
      <td>-0.0373</td>
      <td>0.0761</td>
      <td>0.0631</td>
      <td>0.0956</td>
      <td>0.0601</td>
      <td>0.0445</td>
      <td>0.1083</td>
    </tr>
    <tr>
      <th>201612</th>
      <td>0.0349</td>
      <td>0.0383</td>
      <td>0.0053</td>
      <td>0.0213</td>
      <td>0.0471</td>
      <td>-0.0042</td>
      <td>0.0293</td>
    </tr>
  </tbody>
</table>
</div>




```python
def how_many_betas(data,times):
        repet_times = len(data)-times
        start_period = list(map(lambda x: x, range(times)))
        end_period = list(map(lambda x: x+repet_times, range(times)))
        return start_period, end_period    
```


```python
def DF_liner_regression(data, market_data, times):
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
time_series_beta.head()
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
      <th>Telcm</th>
      <th>Shops</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201211</th>
      <td>0.712616</td>
      <td>1.176400</td>
      <td>1.023954</td>
      <td>0.777419</td>
      <td>0.856403</td>
      <td>0.951974</td>
      <td>1.063345</td>
    </tr>
    <tr>
      <th>201212</th>
      <td>0.713775</td>
      <td>1.175372</td>
      <td>1.023686</td>
      <td>0.771671</td>
      <td>0.861334</td>
      <td>0.955347</td>
      <td>1.064130</td>
    </tr>
    <tr>
      <th>201301</th>
      <td>0.713310</td>
      <td>1.172910</td>
      <td>1.023437</td>
      <td>0.775362</td>
      <td>0.860952</td>
      <td>0.954583</td>
      <td>1.064230</td>
    </tr>
    <tr>
      <th>201302</th>
      <td>0.712744</td>
      <td>1.180943</td>
      <td>1.023730</td>
      <td>0.761206</td>
      <td>0.872278</td>
      <td>0.958258</td>
      <td>1.064633</td>
    </tr>
    <tr>
      <th>201303</th>
      <td>0.712178</td>
      <td>1.184832</td>
      <td>1.024273</td>
      <td>0.759398</td>
      <td>0.871638</td>
      <td>0.959449</td>
      <td>1.064270</td>
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
error_term.head()
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
      <th>Telcm</th>
      <th>Shops</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201211</th>
      <td>0.020997</td>
      <td>0.023927</td>
      <td>0.017467</td>
      <td>-0.024817</td>
      <td>-0.008034</td>
      <td>0.004184</td>
      <td>-0.007955</td>
    </tr>
    <tr>
      <th>201212</th>
      <td>-0.036513</td>
      <td>0.061016</td>
      <td>0.014846</td>
      <td>0.000311</td>
      <td>0.000136</td>
      <td>-0.024804</td>
      <td>0.022405</td>
    </tr>
    <tr>
      <th>201301</th>
      <td>0.006199</td>
      <td>-0.028027</td>
      <td>0.002353</td>
      <td>0.030782</td>
      <td>-0.002901</td>
      <td>-0.000134</td>
      <td>0.010092</td>
    </tr>
    <tr>
      <th>201302</th>
      <td>0.017476</td>
      <td>-0.012884</td>
      <td>-0.001236</td>
      <td>-0.009001</td>
      <td>0.008977</td>
      <td>-0.006511</td>
      <td>0.004634</td>
    </tr>
    <tr>
      <th>201303</th>
      <td>0.015680</td>
      <td>-0.005545</td>
      <td>-0.016118</td>
      <td>-0.013205</td>
      <td>0.018614</td>
      <td>0.006034</td>
      <td>-0.002206</td>
    </tr>
  </tbody>
</table>
</div>

```python
def rolling_window_cal(beta_data, error_data, times):
    start_period, end_period = how_many_betas(beta_data, times)
    
    DF_beta_p = pd.DataFrame()
    DF_beta_p_sq = pd.DataFrame()
    DF_error_term_p = pd.DataFrame()
    for i in range(len(start_period)):
        beta = beta_data[start_period[i]:end_period[i]+1]
        error = error_data[start_period[i]:end_period[i]+1]
        beta_p = np.mean(beta)
        beta_p_sq = beta_p**2
        error_term_p = np.mean(error)
        DF_beta_p.loc[:,beta_data.index[end_period[i]]] = beta_p
        DF_beta_p_sq.loc[:,beta_data.index[end_period[i]]] = beta_p_sq
        DF_error_term_p.loc[:,beta_data.index[end_period[i]]] = error_term_p
    return {'beta_p' : DF_beta_p.T, 'beta_p_sq' : DF_beta_p_sq.T, 'error_term_p' : DF_error_term_p.T}
```

```python
rolling_window_cal(time_series_beta, time_series_intercept, 30)
```




    {'beta_p':            NoDur     Durbl     Manuf     Enrgy     Telcm     Shops     Other
     201608  0.720644  1.221555  1.026674  0.721073  0.917163  0.963859  1.067782
     201609  0.720278  1.222007  1.026632  0.721494  0.917535  0.962974  1.067616
     201610  0.719890  1.222302  1.026572  0.722199  0.917917  0.962060  1.067259
     201611  0.719280  1.222592  1.026557  0.723106  0.918187  0.961052  1.067038
     201612  0.718707  1.222847  1.026460  0.724143  0.918434  0.960007  1.066805,
     'beta_p_sq':            NoDur     Durbl     Manuf     Enrgy     Telcm     Shops     Other
     201608  0.519328  1.492198  1.054060  0.519947  0.841188  0.929023  1.140158
     201609  0.518801  1.493300  1.053973  0.520553  0.841871  0.927319  1.139804
     201610  0.518242  1.494022  1.053850  0.521571  0.842571  0.925560  1.139043
     201611  0.517364  1.494732  1.053819  0.522883  0.843067  0.923621  1.138569
     201612  0.516539  1.495356  1.053620  0.524384  0.843522  0.921613  1.138072,
     'error_term_p':            NoDur     Durbl     Manuf     Enrgy     Telcm     Shops     Other
     201608  0.005117 -0.002574  0.000968  0.003339  0.000964  0.001743 -0.000841
     201609  0.005102 -0.002618  0.000972  0.003335  0.000974  0.001711 -0.000843
     201610  0.005088 -0.002658  0.000970  0.003323  0.000982  0.001676 -0.000836
     201611  0.005070 -0.002693  0.000969  0.003309  0.000994  0.001652 -0.000825
     201612  0.005061 -0.002723  0.000970  0.003286  0.000999  0.001631 -0.000814}




```python
def rolling_window_cal_y(real_data, times):
    start_period, end_period = how_many_betas(real_data, times)
    
    DF_real = pd.DataFrame()
    for i in range(len(start_period)):
        real = real_data.ix[real_data.index[start_period[i]]:real_data.index[end_period[i]]]
        #print(real.ix[real_data.index[end_period[i]]])
        #real_p = np.mean(real)
        real_p = real.ix[real_data.index[end_period[i]]]
        DF_real.loc[:,real_data.index[end_period[i]]] = real_p
    return DF_real.T
```


```python
rolling_window_cal_y(stock_data.ix[stock_data.index[-50]:stock_data.index[-1]], 30).head()
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
      <th>Telcm</th>
      <th>Shops</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201407</th>
      <td>-0.0437</td>
      <td>-0.0463</td>
      <td>-0.0418</td>
      <td>-0.0358</td>
      <td>0.0050</td>
      <td>-0.0230</td>
      <td>-0.0212</td>
    </tr>
    <tr>
      <th>201408</th>
      <td>0.0565</td>
      <td>0.0520</td>
      <td>0.0477</td>
      <td>0.0227</td>
      <td>0.0055</td>
      <td>0.0515</td>
      <td>0.0442</td>
    </tr>
    <tr>
      <th>201409</th>
      <td>-0.0009</td>
      <td>-0.0942</td>
      <td>-0.0238</td>
      <td>-0.0802</td>
      <td>-0.0168</td>
      <td>-0.0144</td>
      <td>-0.0102</td>
    </tr>
    <tr>
      <th>201410</th>
      <td>0.0264</td>
      <td>0.0386</td>
      <td>0.0236</td>
      <td>-0.0444</td>
      <td>0.0141</td>
      <td>0.0303</td>
      <td>0.0329</td>
    </tr>
    <tr>
      <th>201411</th>
      <td>0.0481</td>
      <td>0.0474</td>
      <td>0.0223</td>
      <td>-0.0987</td>
      <td>0.0309</td>
      <td>0.0746</td>
      <td>0.0279</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.fama_macbeth(y = rolling_window_cal_y(stock_data.ix[stock_data.index[-50]:stock_data.index[-1]], 30), x= rolling_window_cal(time_series_beta, time_series_intercept, 30))
```

    
    ----------------------Summary of Fama-MacBeth Analysis-------------------------
    
    Formula: Y ~ beta_p + beta_p_sq + error_term_p + intercept
    # betas :  30
    
    ----------------------Summary of Estimated Coefficients------------------------
         Variable          Beta       Std Err        t-stat       CI 2.5%      CI 97.5%
         (beta_p)        0.2176        0.1276        **1.70**     -0.0325        0.4678
      (beta_p_sq)       -0.0889        0.0561       **-1.59**     -0.1989        0.0210
    (error_term_p)        3.2206        3.5492        **0.91**     -3.7359       10.1771
      (intercept)       -0.1247        0.0832       **-1.50**     -0.2878        0.0383
    
    --------------------------------End of Summary---------------------------------

