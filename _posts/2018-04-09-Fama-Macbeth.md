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
      <th>201211</th>
      <td>0.020997</td>
      <td>0.023927</td>
      <td>0.017467</td>
      <td>-0.024817</td>
      <td>0.011288</td>
      <td>-0.008034</td>
      <td>0.004184</td>
      <td>0.005071</td>
      <td>-0.045948</td>
      <td>-0.007955</td>
    </tr>
    <tr>
      <th>201212</th>
      <td>-0.036513</td>
      <td>0.061016</td>
      <td>0.014846</td>
      <td>0.000311</td>
      <td>-0.009918</td>
      <td>0.000136</td>
      <td>-0.024804</td>
      <td>-0.018405</td>
      <td>-0.009540</td>
      <td>0.022405</td>
    </tr>
    <tr>
      <th>201301</th>
      <td>0.006199</td>
      <td>-0.028027</td>
      <td>0.002353</td>
      <td>0.030782</td>
      <td>-0.045927</td>
      <td>-0.002901</td>
      <td>-0.000134</td>
      <td>0.033764</td>
      <td>0.020267</td>
      <td>0.010092</td>
    </tr>
    <tr>
      <th>201302</th>
      <td>0.017476</td>
      <td>-0.012884</td>
      <td>-0.001236</td>
      <td>-0.009001</td>
      <td>-0.008605</td>
      <td>0.008977</td>
      <td>-0.006511</td>
      <td>-0.000460</td>
      <td>0.009999</td>
      <td>0.004634</td>
    </tr>
    <tr>
      <th>201303</th>
      <td>0.015680</td>
      <td>-0.005545</td>
      <td>-0.016118</td>
      <td>-0.013205</td>
      <td>-0.024512</td>
      <td>0.018614</td>
      <td>0.006034</td>
      <td>0.029746</td>
      <td>0.032974</td>
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




    {'beta_p':            NoDur     Durbl     Manuf     Enrgy     HiTec     Telcm     Shops  \
     201407  0.715540  1.189876  1.025321  0.751282  1.330660  0.882397  0.961173   
     201408  0.716066  1.191065  1.025421  0.749003  1.330860  0.884793  0.961964   
     201409  0.716445  1.192776  1.025622  0.746964  1.331095  0.886943  0.962567   
     201410  0.716869  1.194620  1.025840  0.744621  1.331542  0.889133  0.963282   
     201411  0.717336  1.196189  1.026067  0.742730  1.331860  0.890813  0.963873   
     
     
                Hlth      Utils     Other  
     201407  0.777377  0.436765  1.066578  
     201408  0.778228  0.436408  1.066917  
     201409  0.778778  0.436004  1.067177  
     201410  0.779303  0.435552  1.067429  
     201411  0.779430  0.435199  1.067648  }




```python
def rolling_window_cal_y(real_data, times):
    start_period, end_period = how_many_betas(real_data, times)
    
    DF_real = pd.DataFrame()
    for i in range(len(start_period)):
        real = real_data.ix[real_data.index[start_period[i]]:real_data.index[end_period[i]]]
        real_p = np.mean(real)
        DF_real.loc[:,real_data.index[end_period[i]]] = real_p
    return DF_real.T
```


```python
rolling_window_cal_y(stock_data.ix[stock_data.index[-50]:stock_data.index[-1]], 30).tail()
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
      <td>0.008386</td>
      <td>0.000871</td>
      <td>0.003133</td>
      <td>-0.002714</td>
      <td>0.007038</td>
      <td>0.003700</td>
      <td>0.006595</td>
      <td>0.003648</td>
      <td>0.002881</td>
      <td>0.003400</td>
    </tr>
    <tr>
      <th>201609</th>
      <td>0.008343</td>
      <td>-0.000186</td>
      <td>0.003129</td>
      <td>-0.001514</td>
      <td>0.008819</td>
      <td>0.004795</td>
      <td>0.005514</td>
      <td>0.004243</td>
      <td>0.002838</td>
      <td>0.002319</td>
    </tr>
    <tr>
      <th>201610</th>
      <td>0.008576</td>
      <td>0.000105</td>
      <td>0.003710</td>
      <td>-0.000800</td>
      <td>0.009881</td>
      <td>0.005762</td>
      <td>0.003767</td>
      <td>-0.000038</td>
      <td>0.001986</td>
      <td>0.005495</td>
    </tr>
    <tr>
      <th>201611</th>
      <td>0.004410</td>
      <td>-0.000119</td>
      <td>0.003748</td>
      <td>0.001457</td>
      <td>0.006286</td>
      <td>0.004295</td>
      <td>0.003243</td>
      <td>-0.001438</td>
      <td>0.002714</td>
      <td>0.007400</td>
    </tr>
    <tr>
      <th>201612</th>
      <td>0.007210</td>
      <td>0.001895</td>
      <td>0.004952</td>
      <td>0.003729</td>
      <td>0.007857</td>
      <td>0.007610</td>
      <td>0.002710</td>
      <td>-0.001438</td>
      <td>0.004581</td>
      <td>0.009176</td>
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
         (beta_p)        0.0028        0.0020          1.38       -0.0012        0.0067
      (beta_p_sq)        0.0194        0.0022          8.88        0.0151        0.0236
    (error_term_p)        2.9906        0.3622          8.26        2.2806        3.7005
      (intercept)       -0.0169        0.0034         -4.96       -0.0236       -0.0102
    
    --------------------------------End of Summary---------------------------------

