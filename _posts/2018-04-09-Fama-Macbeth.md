---
layout: post
title:  "[산업응용확률 및 통계] Fama-MacBeth in Python"
subtitle: "Fama-MacBeth의 two-step regression을 활용한 two-parameter model의 증명"
date:   2018-04-09 06:07:13 -0400
background: '/img/posts/01.jpg'
---


# Fama-Macbeth Regression
----------------------------------------------------

Fama-Macbeth의 1973년 논문인 *"Risk, Return and Equilibrium empirical Tests"* 에서 사용하는 두 가지 회귀분석을 통한 Risk-Return의 two-parameter model의 증명에 사용된 회귀분석을 python 코드로 간단하게 구현해 보았습니다.


Fama-Macbeth가 제안한 2단계 회귀분석은

1. Time-Series Regression
2. Cross-Sectional Regression

을 활용함으로서 첫 번째, Time-Series Regression에서 사용하여 구한 베타들을 두 번째, Cross-Sectional Regression에 변수로 활용함으로서 첫 번째 회귀분석을 통하여 도출된 베타들에 대한 통계적 분석을 실시할 수 있습니다. 이 때, 귀무가설을 설정하여 논문에서 이야기하는 모델의 3가지 함의를 검증할 수 있습니다.


```python
import pandas as pd
import numpy as np
from scipy import stats
import statsmodels.formula.api as sm
```

### 1. Data Set


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



테스트에 활용할 데이터의 형태입니다.

### 2. Time-Series Regression

여기서 Time-Series Regression을 활용하기 위해서 각 기간에 대한 rolling window방법을 통하여 기간별 time-series를 통해 얻은 베타를 연속적으로 추정합니다.


```python
def how_many_betas(data,times):
        repet_times = len(data)-times
        start_period = list(map(lambda x: x, range(times)))
        end_period = list(map(lambda x: x+repet_times, range(times)))
        return start_period, end_period    
```

첫 번째, 위 함수를 통하여 Time-Series 분석을 위해 필요한 데이터의 양을 DataFrame에서 추출하기 위하여 슬라이싱에 필요한 인덱스를 자르는 함수를 정의합니다.


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

위 함수는 마켓 데이터와 베타를 구하고자하는 개별 포트폴리오(또는 개별 주식)의 데이터를 받아 베타를 Time-Series로 구하기 위해서 원하는 베타의 수를 인풋으로 받아 각 자산들에 대한 베타를 계산한 DataFrame과 intercept를 받은 DataFrame을 반환합니다.


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
time_series_intercept.head()
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
      <td>0.005674</td>
      <td>-0.002121</td>
      <td>0.000844</td>
      <td>0.003475</td>
      <td>0.001868</td>
      <td>0.002496</td>
      <td>-0.000746</td>
    </tr>
    <tr>
      <th>201212</th>
      <td>0.005619</td>
      <td>-0.002003</td>
      <td>0.000872</td>
      <td>0.003306</td>
      <td>0.002015</td>
      <td>0.002535</td>
      <td>-0.000668</td>
    </tr>
    <tr>
      <th>201301</th>
      <td>0.005769</td>
      <td>-0.001904</td>
      <td>0.000941</td>
      <td>0.003030</td>
      <td>0.002046</td>
      <td>0.002664</td>
      <td>-0.000570</td>
    </tr>
    <tr>
      <th>201302</th>
      <td>0.005829</td>
      <td>-0.002150</td>
      <td>0.000930</td>
      <td>0.003381</td>
      <td>0.001770</td>
      <td>0.002549</td>
      <td>-0.000568</td>
    </tr>
    <tr>
      <th>201303</th>
      <td>0.005819</td>
      <td>-0.002004</td>
      <td>0.000940</td>
      <td>0.003302</td>
      <td>0.001759</td>
      <td>0.002600</td>
      <td>-0.000584</td>
    </tr>
  </tbody>
</table>
</div>



위 함수를 통해 반환된 각각의 데이터프레임입니다.

### 3. Cross-Sectional Regression

논문에서 제시된 3가지 함의에 대한 가설검정을 하기 위해서 error-term을 계산해 주는 함수를 구현합니다.


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



Time-Series 회귀분석을 통해 위에서 구한 각각의 베타들을 활용하여 각각의 error-term에 대한 계산을 해주고 그 또한 데이터프레임의 형태로 반환 받습니다.


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

아래에서 활용할 pandas fama-macbeth 회귀분석(Cross-Sectional Regression)을 사용하기 위하여 각 자산들에 대한 베타, 베타 제곱, 에러텀을 딕셔너리의 형태로 반환하는 함수를 정의합니다.


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



각각의 포트폴리오(또는 개별자산)에 형태로 베타, 베타 제곱, 에러텀을 딕션너리로 표현한 모습입니다.


```python
def rolling_window_cal_y(real_data, times):
    start_period, end_period = how_many_betas(real_data, times)
    
    DF_real = pd.DataFrame()
    for i in range(len(start_period)):
        real = real_data.ix[real_data.index[start_period[i]]:real_data.index[end_period[i]]]
        real_p = real.ix[real_data.index[end_period[i]]]
        DF_real.loc[:,real_data.index[end_period[i]]] = real_p
    return DF_real.T
```

위에서 Cross-Sectional에 활용될 변수들을 구하였다면 이번에는 비교적 간단한 종속변수에 대한 데이터를 반환하는 함수를 구성합니다.


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



함수를 통해서 얻을 수 있는 데이터프레임의 형태입니다.


```python
pd.fama_macbeth(y = rolling_window_cal_y(stock_data.ix[stock_data.index[-50]:stock_data.index[-1]], 30), x= rolling_window_cal(time_series_beta, time_series_intercept, 30))
```



    
    ----------------------Summary of Fama-MacBeth Analysis-------------------------
    
    Formula: Y ~ beta_p + beta_p_sq + error_term_p + intercept
    # betas :  30
    
    ----------------------Summary of Estimated Coefficients------------------------
         Variable          Beta       Std Err        t-stat       CI 2.5%      CI 97.5%
         (beta_p)        0.2176        0.1276          1.70       -0.0325        0.4678
      (beta_p_sq)       -0.0889        0.0561         -1.59       -0.1989        0.0210
    (error_term_p)        3.2206        3.5492          0.91       -3.7359       10.1771
      (intercept)       -0.1247        0.0832         -1.50       -0.2878        0.0383
    
    --------------------------------End of Summary---------------------------------



pandas의 *fama_macbeth* 함수를 통하여 구한 종속변수와 독립변수들을 활용해 회귀계수의 추정을 얻습니다.

### 4. 결론 및 한계

결과의 t-statistic 값을 통하여 각 함의들 중 risk & return의 선형성을 나타내는 함의를 제외하고는 기각되는 것을 확인 할 수 있습니다.

* 첫 번째로, 베타의 t-statistic 값을 통하여 베타의 회귀계수가 0이라는 귀무가설을 기각합니다.
* 두 번째, 베타 제곱의 t-statistic 값을 통해 베타 제곱의 회귀계수가 0이라는 귀무가설을 기각할 수 없음으로 베타 제곱의 회귀계수는 0입니다.
* 세 번째, error-term의 t-statistic 값을 통하여 체계적 위험을 제외한 위험을 의미하는 회귀계수가 0이라는 귀무가설을 기각할 수 없으므로 다음 항의 회귀계수는 0입니다.



위 분석을 통해서 확인할 수 있는 t-statistc을 활용해서 도출되는 **p-value 값들은 두 번째와, 세 번째의 결론을 통해서 알 수 있듯이 가설을 기각할 수 있지는 않지만 낮게 나오고 있다.**


이러한 이유는 논문에서 말하는 바와 같이 *충분히 잘 분산된 포트폴리오*를 사용하지 않아서 발생하는 문제로 고려된다.
따라서, 차후 연구를 통해서 활용할 데이터 및 포트폴리오 구성의 기준을 확정하여 충분히 잘 분산된 포트폴리오를 사용한다면 보다 정확한 결과를 얻을 수 있다고 기대한다.
