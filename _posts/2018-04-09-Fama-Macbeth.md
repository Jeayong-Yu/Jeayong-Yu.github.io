

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
```


```python
real.tail()
```




    201608   -0.00441
    201609    0.00174
    201610   -0.02450
    201611    0.04017
    201612    0.02254
    dtype: float64




```python
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
     201412  0.717855  1.197660  1.026255  0.740774  1.332213  0.892593  0.964448   
     201501  0.718469  1.199205  1.026431  0.738583  1.332514  0.894355  0.965208   
     201502  0.719163  1.200740  1.026565  0.736392  1.332764  0.896241  0.965992   
     201503  0.719908  1.202415  1.026854  0.734047  1.333201  0.898078  0.966878   
     201504  0.720650  1.204063  1.027095  0.731789  1.333647  0.900018  0.967758   
     201505  0.721360  1.205840  1.027341  0.729463  1.334175  0.901868  0.968668   
     201506  0.722105  1.207597  1.027557  0.727113  1.334824  0.903723  0.969573   
     201507  0.722427  1.209020  1.027526  0.725563  1.335471  0.905016  0.970004   
     201508  0.722696  1.210340  1.027464  0.724239  1.335932  0.906385  0.970368   
     201509  0.722710  1.211427  1.027327  0.723452  1.336296  0.907668  0.970263   
     201510  0.722628  1.212393  1.027223  0.723029  1.336645  0.908966  0.969788   
     201511  0.722626  1.213331  1.027129  0.722415  1.336981  0.910235  0.969482   
     201512  0.722674  1.214246  1.027033  0.722048  1.337218  0.911268  0.969143   
     201601  0.722563  1.215385  1.026924  0.721786  1.337385  0.912160  0.968683   
     201602  0.722374  1.216619  1.026828  0.721440  1.337666  0.913086  0.968163   
     201603  0.722165  1.217861  1.026801  0.721071  1.338029  0.914068  0.967581   
     201604  0.721958  1.219104  1.026775  0.720759  1.338425  0.914923  0.966988   
     201605  0.721643  1.220038  1.026813  0.720744  1.338655  0.915558  0.966259   
     201606  0.721378  1.220551  1.026780  0.720742  1.338823  0.916127  0.965546   
     201607  0.721023  1.221113  1.026742  0.720783  1.338971  0.916655  0.964726   
     201608  0.720644  1.221555  1.026674  0.721073  1.339014  0.917163  0.963859   
     201609  0.720278  1.222007  1.026632  0.721494  1.338840  0.917535  0.962974   
     201610  0.719890  1.222302  1.026572  0.722199  1.338615  0.917917  0.962060   
     201611  0.719280  1.222592  1.026557  0.723106  1.338296  0.918187  0.961052   
     201612  0.718707  1.222847  1.026460  0.724143  1.337941  0.918434  0.960007   
     
                Hlth      Utils     Other  
     201407  0.777377  0.436765  1.066578  
     201408  0.778228  0.436408  1.066917  
     201409  0.778778  0.436004  1.067177  
     201410  0.779303  0.435552  1.067429  
     201411  0.779430  0.435199  1.067648  
     201412  0.779569  0.434999  1.067864  
     201501  0.779728  0.434820  1.068253  
     201502  0.779796  0.434553  1.068638  
     201503  0.779873  0.434140  1.069010  
     201504  0.779825  0.433668  1.069348  
     201505  0.779757  0.433053  1.069675  
     201506  0.779686  0.432501  1.069968  
     201507  0.779446  0.431645  1.070147  
     201508  0.779198  0.430955  1.070212  
     201509  0.779159  0.430352  1.070010  
     201510  0.779255  0.429822  1.069668  
     201511  0.779338  0.429296  1.069436  
     201512  0.779383  0.428821  1.069225  
     201601  0.779650  0.428107  1.069069  
     201602  0.779963  0.427383  1.068891  
     201603  0.780238  0.426726  1.068709  
     201604  0.780684  0.425910  1.068499  
     201605  0.780918  0.425235  1.068283  
     201606  0.781169  0.424562  1.068123  
     201607  0.781431  0.423872  1.067949  
     201608  0.781679  0.423230  1.067782  
     201609  0.781849  0.422678  1.067616  
     201610  0.782231  0.422180  1.067259  
     201611  0.782558  0.421720  1.067038  
     201612  0.782967  0.421315  1.066805  ,
     'beta_p_sq':            NoDur     Durbl     Manuf     Enrgy     HiTec     Telcm     Shops  \
     201407  0.511998  1.415806  1.051284  0.564425  1.770655  0.778625  0.923853   
     201408  0.512751  1.418637  1.051488  0.561005  1.771187  0.782859  0.925374   
     201409  0.513293  1.422714  1.051901  0.557955  1.771815  0.786668  0.926535   
     201410  0.513902  1.427116  1.052347  0.554460  1.773005  0.790558  0.927912   
     201411  0.514571  1.430869  1.052814  0.551647  1.773850  0.793547  0.929052   
     201412  0.515316  1.434390  1.053199  0.548746  1.774790  0.796722  0.930159   
     201501  0.516197  1.438092  1.053561  0.545505  1.775593  0.799872  0.931626   
     201502  0.517195  1.441776  1.053836  0.542274  1.776260  0.803247  0.933141   
     201503  0.518267  1.445801  1.054430  0.538825  1.777425  0.806544  0.934852   
     201504  0.519336  1.449768  1.054923  0.535515  1.778615  0.810032  0.936556   
     201505  0.520361  1.454051  1.055430  0.532117  1.780024  0.813367  0.938317   
     201506  0.521435  1.458290  1.055874  0.528693  1.781755  0.816716  0.940071   
     201507  0.521900  1.461729  1.055810  0.526442  1.783483  0.819055  0.940908   
     201508  0.522289  1.464922  1.055683  0.524522  1.784715  0.821533  0.941613   
     201509  0.522310  1.467554  1.055401  0.523383  1.785687  0.823862  0.941411   
     201510  0.522191  1.469896  1.055187  0.522772  1.786619  0.826220  0.940490   
     201511  0.522188  1.472173  1.054994  0.521883  1.787517  0.828527  0.939895   
     201512  0.522258  1.474392  1.054797  0.521354  1.788152  0.830410  0.939238   
     201601  0.522097  1.477161  1.054573  0.520975  1.788599  0.832036  0.938348   
     201602  0.521824  1.480162  1.054375  0.520476  1.789351  0.833726  0.937340   
     201603  0.521522  1.483186  1.054320  0.519944  1.790323  0.835520  0.936214   
     201604  0.521223  1.486214  1.054268  0.519494  1.791381  0.837084  0.935067   
     201605  0.520769  1.488492  1.054345  0.519472  1.791997  0.838246  0.933657   
     201606  0.520386  1.489745  1.054276  0.519469  1.792447  0.839288  0.932278   
     201607  0.519874  1.491117  1.054199  0.519528  1.792844  0.840257  0.930697   
     201608  0.519328  1.492198  1.054060  0.519947  1.792958  0.841188  0.929023   
     201609  0.518801  1.493300  1.053973  0.520553  1.792493  0.841871  0.927319   
     201610  0.518242  1.494022  1.053850  0.521571  1.791890  0.842571  0.925560   
     201611  0.517364  1.494732  1.053819  0.522883  1.791036  0.843067  0.923621   
     201612  0.516539  1.495356  1.053620  0.524384  1.790087  0.843522  0.921613   
     
                Hlth      Utils     Other  
     201407  0.604315  0.190763  1.137589  
     201408  0.605639  0.190452  1.138313  
     201409  0.606495  0.190100  1.138867  
     201410  0.607314  0.189705  1.139405  
     201411  0.607511  0.189398  1.139872  
     201412  0.607727  0.189224  1.140333  
     201501  0.607976  0.189068  1.141164  
     201502  0.608082  0.188836  1.141987  
     201503  0.608202  0.188478  1.142783  
     201504  0.608127  0.188068  1.143505  
     201505  0.608022  0.187535  1.144206  
     201506  0.607910  0.187057  1.144831  
     201507  0.607536  0.186317  1.145215  
     201508  0.607150  0.185723  1.145355  
     201509  0.607089  0.185202  1.144922  
     201510  0.607238  0.184747  1.144189  
     201511  0.607367  0.184295  1.143694  
     201512  0.607438  0.183887  1.143242  
     201601  0.607853  0.183276  1.142908  
     201602  0.608343  0.182656  1.142527  
     201603  0.608771  0.182095  1.142140  
     201604  0.609467  0.181400  1.141689  
     201605  0.609833  0.180825  1.141229  
     201606  0.610225  0.180253  1.140887  
     201607  0.610634  0.179667  1.140514  
     201608  0.611023  0.179123  1.140158  
     201609  0.611288  0.178657  1.139804  
     201610  0.611885  0.178236  1.139043  
     201611  0.612397  0.177848  1.138569  
     201612  0.613038  0.177507  1.138072  ,
     'error_term_p':            NoDur     Durbl     Manuf     Enrgy     HiTec     Telcm     Shops  \
     201407  0.005580 -0.001774  0.000910  0.003263 -0.002911  0.001826  0.002427   
     201408  0.005562 -0.001764  0.000914  0.003275 -0.002908  0.001788  0.002405   
     201409  0.005545 -0.001758  0.000919  0.003282 -0.002900  0.001744  0.002380   
     201410  0.005523 -0.001756  0.000920  0.003291 -0.002889  0.001699  0.002353   
     201411  0.005501 -0.001746  0.000919  0.003276 -0.002875  0.001665  0.002337   
     201412  0.005475 -0.001748  0.000919  0.003276 -0.002875  0.001627  0.002316   
     201501  0.005450 -0.001762  0.000919  0.003287 -0.002877  0.001583  0.002297   
     201502  0.005426 -0.001791  0.000919  0.003305 -0.002878  0.001540  0.002271   
     201503  0.005398 -0.001815  0.000923  0.003318 -0.002876  0.001490  0.002248   
     201504  0.005373 -0.001849  0.000927  0.003333 -0.002868  0.001448  0.002220   
     201505  0.005345 -0.001896  0.000929  0.003354 -0.002869  0.001403  0.002185   
     201506  0.005314 -0.001952  0.000928  0.003388 -0.002877  0.001348  0.002144   
     201507  0.005281 -0.002014  0.000920  0.003418 -0.002884  0.001285  0.002100   
     201508  0.005250 -0.002064  0.000918  0.003438 -0.002881  0.001226  0.002055   
     201509  0.005226 -0.002108  0.000917  0.003442 -0.002878  0.001180  0.002018   
     201510  0.005208 -0.002151  0.000922  0.003463 -0.002886  0.001138  0.001979   
     201511  0.005191 -0.002195  0.000927  0.003472 -0.002895  0.001093  0.001951   
     201512  0.005181 -0.002236  0.000931  0.003457 -0.002902  0.001048  0.001936   
     201601  0.005176 -0.002283  0.000931  0.003431 -0.002902  0.001021  0.001921   
     201602  0.005165 -0.002322  0.000938  0.003406 -0.002906  0.000995  0.001900   
     201603  0.005154 -0.002365  0.000944  0.003390 -0.002913  0.000971  0.001875   
     201604  0.005146 -0.002401  0.000952  0.003383 -0.002931  0.000953  0.001850   
     201605  0.005138 -0.002441  0.000957  0.003357 -0.002940  0.000953  0.001823   
     201606  0.005137 -0.002489  0.000960  0.003348 -0.002953  0.000961  0.001798   
     201607  0.005131 -0.002532  0.000963  0.003336 -0.002965  0.000965  0.001774   
     201608  0.005117 -0.002574  0.000968  0.003339 -0.002971  0.000964  0.001743   
     201609  0.005102 -0.002618  0.000972  0.003335 -0.002959  0.000974  0.001711   
     201610  0.005088 -0.002658  0.000970  0.003323 -0.002942  0.000982  0.001676   
     201611  0.005070 -0.002693  0.000969  0.003309 -0.002937  0.000994  0.001652   
     201612  0.005061 -0.002723  0.000970  0.003286 -0.002924  0.000999  0.001631   
     
                Hlth      Utils     Other  
     201407  0.004072  0.005785 -0.000692  
     201408  0.004080  0.005797 -0.000699  
     201409  0.004083  0.005803 -0.000710  
     201410  0.004080  0.005804 -0.000724  
     201411  0.004088  0.005801 -0.000738  
     201412  0.004086  0.005805 -0.000749  
     201501  0.004088  0.005807 -0.000763  
     201502  0.004090  0.005812 -0.000779  
     201503  0.004093  0.005812 -0.000794  
     201504  0.004091  0.005806 -0.000805  
     201505  0.004096  0.005804 -0.000814  
     201506  0.004094  0.005800 -0.000821  
     201507  0.004094  0.005795 -0.000828  
     201508  0.004095  0.005789 -0.000834  
     201509  0.004094  0.005782 -0.000839  
     201510  0.004087  0.005768 -0.000847  
     201511  0.004080  0.005744 -0.000845  
     201512  0.004087  0.005711 -0.000842  
     201601  0.004088  0.005685 -0.000840  
     201602  0.004080  0.005668 -0.000839  
     201603  0.004064  0.005653 -0.000841  
     201604  0.004048  0.005645 -0.000842  
     201605  0.004033  0.005631 -0.000839  
     201606  0.004023  0.005629 -0.000841  
     201607  0.004014  0.005621 -0.000842  
     201608  0.004002  0.005603 -0.000841  
     201609  0.003996  0.005579 -0.000843  
     201610  0.003985  0.005557 -0.000836  
     201611  0.003973  0.005543 -0.000825  
     201612  0.003962  0.005532 -0.000814  }




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

    C:\Users\user\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:2881: FutureWarning: The pandas.stats.fama_macbeth module is deprecated and will be removed in a future version. We refer to external packages like statsmodels, see here: http://www.statsmodels.org/stable/index.html
      exec(code_obj, self.user_global_ns, self.user_ns)
    C:\Users\user\Anaconda3\lib\site-packages\pandas\stats\fama_macbeth.py:28: FutureWarning: The pandas.stats.plm module is deprecated and will be removed in a future version. We refer to external packages like statsmodels, see some examples here: http://www.statsmodels.org/stable/mixed_linear.html
      return klass(**kwargs)
    




    
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




```python

```
