---
layout: post
title:  "[창의적 종합설계] Human capital을 고려한 TDF Glide paths"
subtitle: "Human capital을 고려한 TDF glide paths 실험 결과"
date:   2018-12-06 03:18:13 -0400
background: '/img/posts/01.jpg'
---


[창의적 종합설계] Human capital을 고려한 TDF Glide paths
====================

```python
import pandas as pd
from utils import total_change_plot
from gen_glide_path import *
```


```python
initial_wealth = 5000
initial_hc = 5000

init_period = 35
```


```python
mu_rf = 0.05
sigma_rf = 0.05

rho_rf = 0.4

mu_market = 0.1
sigma_market = 0.2

rho = 0.2

mu_h = 0.04
sigma_h = 0.01

gamma = 4
size = 20000
```

_______________________________________

# 1. Risk aversion coefficient

* risk aversion coefficient
    * 2부터 6까지 1씩 변화한 경우의 glide path 변화
    
    
* initial wealth & inital hc
    * 5,000으로 고정


```python
gamma_list = []
df_gammaglide_path = pd.DataFrame()
for i in range(5):
    glide_path = gen_glide_path(initial_wealth, initial_hc, init_period, size, mu_rf, sigma_rf, mu_market, sigma_market, mu_h, sigma_h, rho, i+2)
    gamma_list.append(i+2)
    df_gammaglide_path = pd.concat([df_gammaglide_path, glide_path], axis=1, sort=False)
df_gammaglide_path.columns = gamma_list
df_gammaglide_path
```

    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 10065.6875
    max: 1.0 , hc : 7401.221424591721 , wealth 18660.1799579
    max: 1.0 , hc : 9004.717527534582 , wealth 33032.3417032
    max: 0.918313146385 , hc : 10955.615715167101 , wealth 56824.4635427
    max: 0.843178079039 , hc : 13329.181657437108 , wealth 94159.6643419
    max: 0.801612906131 , hc : 16216.987550137701 , wealth 151494.879613
    max: 0.624194191067 , hc : 19730.4449710597 , wealth 239420.161865
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_6_1.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 0.969412659869 , hc : 6083.264512000001 , wealth 10065.6875
    max: 0.779736966909 , hc : 7401.221424591721 , wealth 18530.8207834
    max: 0.695907800718 , hc : 9004.717527534582 , wealth 31213.4196291
    max: 0.631923199642 , hc : 10955.615715167101 , wealth 50271.8398866
    max: 0.586073401581 , hc : 13329.181657437108 , wealth 78467.4576372
    max: 0.556176595416 , hc : 16216.987550137701 , wealth 119803.708507
    max: 0.414303077538 , hc : 19730.4449710597 , wealth 180149.305562
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_6_3.png)


    max: 0.970448460818 , hc : 5000 , wealth 5000
    max: 0.724282117595 , hc : 6083.264512000001 , wealth 9998.26511065
    max: 0.596988810088 , hc : 7401.221424591721 , wealth 17417.8695247
    max: 0.538253924435 , hc : 9004.717527534582 , wealth 28291.5419045
    max: 0.49164288173 , hc : 10955.615715167101 , wealth 44239.5004746
    max: 0.45687283926 , hc : 13329.181657437108 , wealth 67312.5992603
    max: 0.431566426688 , hc : 16216.987550137701 , wealth 100406.757145
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 147595.729099
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_6_5.png)


    max: 0.770547893317 , hc : 5000 , wealth 5000
    max: 0.588937460258 , hc : 6083.264512000001 , wealth 9551.61554692
    max: 0.489916546815 , hc : 7401.221424591721 , wealth 16227.4160843
    max: 0.443619558957 , hc : 9004.717527534582 , wealth 25892.1048247
    max: 0.405844036454 , hc : 10955.615715167101 , wealth 39876.337784
    max: 0.377399008899 , hc : 13329.181657437108 , wealth 59851.7374113
    max: 0.354846445783 , hc : 16216.987550137701 , wealth 88151.900689
    max: 0.247193857765 , hc : 19730.4449710597 , wealth 127965.283655
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_6_7.png)


    max: 0.637863449031 , hc : 5000 , wealth 5000
    max: 0.495440818543 , hc : 6083.264512000001 , wealth 9264.07191546
    max: 0.414993611504 , hc : 7401.221424591721 , wealth 15466.6921738
    max: 0.376770899204 , hc : 9004.717527534582 , wealth 24373.1893563
    max: 0.345563735785 , hc : 10955.615715167101 , wealth 37139.8251851
    max: 0.321395046975 , hc : 13329.181657437108 , wealth 55224.2054514
    max: 0.30150269691 , hc : 16216.987550137701 , wealth 80631.2185564
    max: 0.205620398449 , hc : 19730.4449710597 , wealth 116067.360394
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_6_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>97.0</td>
      <td>77.0</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>97.0</td>
      <td>77.0</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>100.0</td>
      <td>97.0</td>
      <td>72.0</td>
      <td>59.0</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>100.0</td>
      <td>78.0</td>
      <td>60.0</td>
      <td>49.0</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>100.0</td>
      <td>70.0</td>
      <td>54.0</td>
      <td>44.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>92.0</td>
      <td>63.0</td>
      <td>49.0</td>
      <td>41.0</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>84.0</td>
      <td>59.0</td>
      <td>46.0</td>
      <td>38.0</td>
      <td>32.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>80.0</td>
      <td>56.0</td>
      <td>43.0</td>
      <td>35.0</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>62.0</td>
      <td>41.0</td>
      <td>31.0</td>
      <td>25.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>62.0</td>
      <td>41.0</td>
      <td>31.0</td>
      <td>25.0</td>
      <td>21.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________

# 2. initial wealth amount

* inital wealth amount
    * 0부터 20,000까지 5,000씩 변화한 경우의 glide path 변화
    
    
* inital hc
    * 5,000으로 고정
    

* risk aversion coefficient
    * gamma = 4로 고정


```python
wealth_list = [0, 5000, 10000, 15000, 20000]
df_weathglide_path = pd.DataFrame()
for i in wealth_list:
    glide_path = gen_glide_path(i, initial_hc, init_period, size, mu_rf, sigma_rf, mu_market, sigma_market, mu_h, sigma_h, rho, 4)
    df_weathglide_path = pd.concat([df_weathglide_path, glide_path], axis=1, sort=False)
df_weathglide_path.columns = wealth_list
df_weathglide_path
```

    max: 1.0 , hc : 5000 , wealth 0
    max: 1.0 , hc : 6083.264512000001 , wealth 2013.1375
    max: 1.0 , hc : 7401.221424591721 , wealth 5691.46765743
    max: 0.792237288975 , hc : 9004.717527534582 , wealth 12146.1008561
    max: 0.651328861368 , hc : 10955.615715167101 , wealth 22112.5880681
    max: 0.565914781618 , hc : 13329.181657437108 , wealth 36950.9213563
    max: 0.502040389763 , hc : 16216.987550137701 , wealth 58723.7239944
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 90168.6697246
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_10_1.png)


    max: 0.970448460818 , hc : 5000 , wealth 5000
    max: 0.724282117595 , hc : 6083.264512000001 , wealth 9998.26511065
    max: 0.596988810088 , hc : 7401.221424591721 , wealth 17417.8695247
    max: 0.538253924435 , hc : 9004.717527534582 , wealth 28291.5419045
    max: 0.49164288173 , hc : 10955.615715167101 , wealth 44239.5004746
    max: 0.45687283926 , hc : 13329.181657437108 , wealth 67312.5992603
    max: 0.431566426688 , hc : 16216.987550137701 , wealth 100406.757145
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 147595.729099
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_10_3.png)


    max: 0.684129224753 , hc : 5000 , wealth 10000
    max: 0.571328844125 , hc : 6083.264512000001 , wealth 16854.368225
    max: 0.501584913559 , hc : 7401.221424591721 , wealth 26820.4601114
    max: 0.471099496241 , hc : 9004.717527534582 , wealth 41175.765104
    max: 0.442544259466 , hc : 10955.615715167101 , wealth 61927.0963819
    max: 0.419929272079 , hc : 13329.181657437108 , wealth 91602.5571707
    max: 0.406056168488 , hc : 16216.987550137701 , wealth 133772.198435
    max: 0.309828603044 , hc : 19730.4449710597 , wealth 193582.404699
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_10_5.png)


    max: 0.57420118921 , hc : 5000 , wealth 15000
    max: 0.501310357254 , hc : 6083.264512000001 , wealth 23734.3317802
    max: 0.453247535939 , hc : 7401.221424591721 , wealth 36268.0810797
    max: 0.434642617399 , hc : 9004.717527534582 , wealth 54131.4610816
    max: 0.414617132385 , hc : 10955.615715167101 , wealth 79721.7211814
    max: 0.39787168371 , hc : 13329.181657437108 , wealth 116050.820439
    max: 0.390275810084 , hc : 16216.987550137701 , wealth 167357.748905
    max: 0.309828603044 , hc : 19730.4449710597 , wealth 239868.464822
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_10_7.png)


    max: 0.515740859862 , hc : 5000 , wealth 20000
    max: 0.461259387226 , hc : 6083.264512000001 , wealth 30618.9631855
    max: 0.424182685096 , hc : 7401.221424591721 , wealth 45726.6020441
    max: 0.41168236303 , hc : 9004.717527534582 , wealth 67106.9568391
    max: 0.396278483743 , hc : 10955.615715167101 , wealth 97544.1877181
    max: 0.383330971881 , hc : 13329.181657437108 , wealth 140529.967321
    max: 0.379603745746 , hc : 16216.987550137701 , wealth 200989.652186
    max: 0.309828603044 , hc : 19730.4449710597 , wealth 286216.806413
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_10_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>5000</th>
      <th>10000</th>
      <th>15000</th>
      <th>20000</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>100.0</td>
      <td>97.0</td>
      <td>68.0</td>
      <td>57.0</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>100.0</td>
      <td>97.0</td>
      <td>68.0</td>
      <td>57.0</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>100.0</td>
      <td>72.0</td>
      <td>57.0</td>
      <td>50.0</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>100.0</td>
      <td>60.0</td>
      <td>50.0</td>
      <td>45.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>79.0</td>
      <td>54.0</td>
      <td>47.0</td>
      <td>43.0</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>65.0</td>
      <td>49.0</td>
      <td>44.0</td>
      <td>41.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>57.0</td>
      <td>46.0</td>
      <td>42.0</td>
      <td>40.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>50.0</td>
      <td>43.0</td>
      <td>41.0</td>
      <td>39.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________

# 3. initial labor income

* inital hc
    * 2,000부터 10,000까지 2,000씩 변화한 경우의 glide path 변화
    
    
* inital wealth amount
    * 5,000으로 고정
    

* risk aversion coefficient
    * gamma = 4로 고정


```python
hc_list = [2000, 4000, 6000, 8000, 10000]
df_hcglide_path = pd.DataFrame()
for i in hc_list:
    glide_path = gen_glide_path(5000, i, init_period, size, mu_rf, sigma_rf, mu_market, sigma_market, mu_h, sigma_h, rho, 4)
    df_hcglide_path = pd.concat([df_hcglide_path, glide_path], axis=1, sort=False)
df_hcglide_path.columns = hc_list
df_hcglide_path
```

    max: 0.619046398457 , hc : 2000 , wealth 5000
    max: 0.5308306843 , hc : 2433.3058048000003 , wealth 8116.99688294
    max: 0.474034265597 , hc : 2960.4885698366884 , wealth 12616.400876
    max: 0.450595609296 , hc : 3601.8870110138323 , wealth 19059.5063252
    max: 0.42694482096 , hc : 4382.24628606684 , wealth 28327.4094131
    max: 0.407782503403 , hc : 5331.672662974843 , wealth 41527.692611
    max: 0.3973225711 , hc : 6486.79502005508 , wealth 60223.6115696
    max: 0.309687181688 , hc : 7892.17798842388 , wealth 86686.9981026
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_14_1.png)


    max: 0.863118943304 , hc : 4000 , wealth 5000
    max: 0.671354346382 , hc : 4866.6116096000005 , wealth 9366.16619234
    max: 0.565498803957 , hc : 5920.977139673377 , wealth 15808.1722862
    max: 0.516886870032 , hc : 7203.774022027665 , wealth 25200.1298569
    max: 0.476419922343 , hc : 8764.49257213368 , wealth 38916.0034157
    max: 0.44565994872 , hc : 10663.345325949686 , wealth 58691.6848415
    max: 0.423727208892 , hc : 12973.59004011016 , wealth 86979.0989574
    max: 0.309828603044 , hc : 15784.35597684776 , wealth 127244.790337
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_14_3.png)


    max: 1.0 , hc : 6000 , wealth 5000
    max: 0.775421129622 , hc : 7299.917414400001 , wealth 10468.315
    max: 0.625920525982 , hc : 8881.465709510065 , wealth 18808.3681367
    max: 0.557628374026 , hc : 10805.661033041497 , wealth 31083.9480224
    max: 0.505108294067 , hc : 13146.73885820052 , wealth 49156.7905038
    max: 0.466712948276 , hc : 15995.017988924526 , wealth 75378.8754435
    max: 0.438338631399 , hc : 19460.385060165238 , wealth 113077.420817
    max: 0.309828603044 , hc : 23676.533965271636 , wealth 166907.574749
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_14_5.png)


    max: 1.0 , hc : 8000 , wealth 5000
    max: 0.86556702432 , hc : 9733.223219200001 , wealth 11273.57
    max: 0.673983347297 , hc : 11841.954279346754 , wealth 21408.7948136
    max: 0.58831763697 , hc : 14407.54804405533 , wealth 36423.9009848
    max: 0.526311001386 , hc : 17528.98514426736 , wealth 58652.1463124
    max: 0.482014904749 , hc : 21326.69065189937 , wealth 91049.0976978
    max: 0.448453296719 , hc : 25947.18008022032 , wealth 137789.296496
    max: 0.309687181688 , hc : 31568.71195369552 , wealth 204655.908813
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_14_7.png)


    max: 1.0 , hc : 10000 , wealth 5000
    max: 0.9390603775 , hc : 12166.529024000001 , wealth 12078.825
    max: 0.710338303326 , hc : 14802.442849183442 , wealth 24016.2426711
    max: 0.610689280187 , hc : 18009.435055069163 , wealth 41776.0044642
    max: 0.541117983112 , hc : 21911.231430334203 , wealth 68166.5175322
    max: 0.49254554773 , hc : 26658.363314874216 , wealth 106740.813158
    max: 0.455319137837 , hc : 32433.975100275402 , wealth 162526.076246
    max: 0.309828603044 , hc : 39460.8899421194 , wealth 242433.1131
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_14_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2000</th>
      <th>4000</th>
      <th>6000</th>
      <th>8000</th>
      <th>10000</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>62.0</td>
      <td>86.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>62.0</td>
      <td>86.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>53.0</td>
      <td>67.0</td>
      <td>78.0</td>
      <td>87.0</td>
      <td>94.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>47.0</td>
      <td>57.0</td>
      <td>63.0</td>
      <td>67.0</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>45.0</td>
      <td>52.0</td>
      <td>56.0</td>
      <td>59.0</td>
      <td>61.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>43.0</td>
      <td>48.0</td>
      <td>51.0</td>
      <td>53.0</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>41.0</td>
      <td>45.0</td>
      <td>47.0</td>
      <td>48.0</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>40.0</td>
      <td>42.0</td>
      <td>44.0</td>
      <td>45.0</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________

# 4. Don't consider human capital

* inital hc
    * 0로 고정
    
    
* inital wealth amount
    * 5,000부터 25,000까지 5,000씩 변화한 경우의 glide path 변화
    

* risk aversion coefficient
    * gamma = 4로 고정


```python
wealth_list2 = [5000, 10000, 15000, 20000, 25000]
df_noglide_path = pd.DataFrame()
for i in wealth_list2:
    glide_path = gen_glide_path(i, 0, init_period, size, mu_rf, sigma_rf, mu_market, sigma_market, mu_h, sigma_h, rho, 4)
    df_noglide_path = pd.concat([df_noglide_path, glide_path], axis=1, sort=False)
df_noglide_path.columns = wealth_list2
df_noglide_path
```

    max: 0.326401418228 , hc : 0 , wealth 5000
    max: 0.314387784502 , hc : 0.0 , wealth 6892.99436569
    max: 0.306336434672 , hc : 0.0 , wealth 9475.9390699
    max: 0.311970915621 , hc : 0.0 , wealth 13002.1802758
    max: 0.312205973383 , hc : 0.0 , wealth 17864.2290702
    max: 0.312488816095 , hc : 0.0 , wealth 24545.7510167
    max: 0.325710884262 , hc : 0.0 , wealth 33728.5125118
    max: 0.309828603044 , hc : 0.0 , wealth 46490.5619508
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_18_1.png)


    max: 0.326401418228 , hc : 0 , wealth 10000
    max: 0.314387784502 , hc : 0.0 , wealth 13785.9887314
    max: 0.306336434672 , hc : 0.0 , wealth 18951.8781398
    max: 0.311970915621 , hc : 0.0 , wealth 26004.3605516
    max: 0.312205973383 , hc : 0.0 , wealth 35728.4581405
    max: 0.312488816095 , hc : 0.0 , wealth 49091.5020334
    max: 0.325710884262 , hc : 0.0 , wealth 67457.0250236
    max: 0.309828603044 , hc : 0.0 , wealth 92981.1239016
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_18_3.png)


    max: 0.326401418228 , hc : 0 , wealth 15000
    max: 0.314387784502 , hc : 0.0 , wealth 20678.9830971
    max: 0.306336434672 , hc : 0.0 , wealth 28427.8172097
    max: 0.311970915621 , hc : 0.0 , wealth 39006.5408275
    max: 0.312205973383 , hc : 0.0 , wealth 53592.6872107
    max: 0.312488816095 , hc : 0.0 , wealth 73637.2530501
    max: 0.325710884262 , hc : 0.0 , wealth 101185.537535
    max: 0.309828603044 , hc : 0.0 , wealth 139471.685852
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_18_5.png)


    max: 0.326401418228 , hc : 0 , wealth 20000
    max: 0.314387784502 , hc : 0.0 , wealth 27571.9774628
    max: 0.306336434672 , hc : 0.0 , wealth 37903.7562796
    max: 0.311970915621 , hc : 0.0 , wealth 52008.7211033
    max: 0.312205973383 , hc : 0.0 , wealth 71456.9162809
    max: 0.312488816095 , hc : 0.0 , wealth 98183.0040668
    max: 0.325710884262 , hc : 0.0 , wealth 134914.050047
    max: 0.309828603044 , hc : 0.0 , wealth 185962.247803
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_18_7.png)


    max: 0.326401418228 , hc : 0 , wealth 25000
    max: 0.314387784502 , hc : 0.0 , wealth 34464.9718285
    max: 0.306477856028 , hc : 0.0 , wealth 47379.6953495
    max: 0.311970915621 , hc : 0.0 , wealth 65013.0589654
    max: 0.312316182604 , hc : 0.0 , wealth 89324.1097475
    max: 0.312488816095 , hc : 0.0 , wealth 122736.001614
    max: 0.325569462906 , hc : 0.0 , wealth 168652.520074
    max: 0.309828603044 , hc : 0.0 , wealth 232458.827049
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_18_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>5000</th>
      <th>10000</th>
      <th>15000</th>
      <th>20000</th>
      <th>25000</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________

# 5. Expected return of risk free 

* expected return of risk free
    * 0.01부터 0.01씩 0.05까지 변경
    

* initial hc & initial wealth amount & risk aversion coefficient
    * initial hc = 5,000으로 고정
    * initial wealth amount = 5,000으로 고정
    * gamma = 4로 고정


```python
rf_list = [0.01, 0.02, 0.03, 0.04, 0.05]
df_rfglide_path = pd.DataFrame()
for i in rf_list:
    glide_path = gen_glide_path(initial_wealth, initial_hc, init_period, size, i, sigma_rf, mu_market, sigma_market, mu_h, sigma_h, rho, 4)
    df_rfglide_path = pd.concat([df_rfglide_path, glide_path], axis=1, sort=False)
df_rfglide_path.columns = rf_list
df_rfglide_path
```

    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 10065.6875
    max: 1.0 , hc : 7401.221424591721 , wealth 18660.1799579
    max: 1.0 , hc : 9004.717527534582 , wealth 33032.3417032
    max: 1.0 , hc : 10955.615715167101 , wealth 56824.4635427
    max: 0.888982754894 , hc : 13329.181657437108 , wealth 95927.3989465
    max: 0.763305573081 , hc : 16216.987550137701 , wealth 152729.26193
    max: 0.568221441278 , hc : 19730.4449710597 , wealth 228980.628963
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_22_1.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 10065.6875
    max: 1.0 , hc : 7401.221424591721 , wealth 18660.1799579
    max: 0.967757035632 , hc : 9004.717527534582 , wealth 33032.3417032
    max: 0.839813421951 , hc : 10955.615715167101 , wealth 56161.3302622
    max: 0.766271554572 , hc : 13329.181657437108 , wealth 89461.1399227
    max: 0.681123744719 , hc : 16216.987550137701 , wealth 137167.660354
    max: 0.503381959152 , hc : 19730.4449710597 , wealth 202261.749496
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_22_3.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 10065.6875
    max: 0.923917144447 , hc : 7401.221424591721 , wealth 18660.1799579
    max: 0.799205052906 , hc : 9004.717527534582 , wealth 32240.3946438
    max: 0.714034317326 , hc : 10955.615715167101 , wealth 52089.5819215
    max: 0.660619857774 , hc : 13329.181657437108 , wealth 80554.4704751
    max: 0.601304647376 , hc : 16216.987550137701 , wealth 121128.387857
    max: 0.438785821195 , hc : 19730.4449710597 , wealth 177297.65763
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_22_5.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 0.929636246146 , hc : 6083.264512000001 , wealth 10065.6875
    max: 0.745414114236 , hc : 7401.221424591721 , wealth 18304.8271579
    max: 0.663036726655 , hc : 9004.717527534582 , wealth 30267.9912154
    max: 0.600888669714 , hc : 10955.615715167101 , wealth 47733.1711087
    max: 0.558624676432 , hc : 13329.181657437108 , wealth 72814.8768535
    max: 0.518369860777 , hc : 16216.987550137701 , wealth 108567.386575
    max: 0.374079474018 , hc : 19730.4449710597 , wealth 158772.894337
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_22_7.png)


    max: 0.970448460818 , hc : 5000 , wealth 5000
    max: 0.724282117595 , hc : 6083.264512000001 , wealth 9998.26511065
    max: 0.596988810088 , hc : 7401.221424591721 , wealth 17417.8695247
    max: 0.538253924435 , hc : 9004.717527534582 , wealth 28291.5419045
    max: 0.49164288173 , hc : 10955.615715167101 , wealth 44239.5004746
    max: 0.45687283926 , hc : 13329.181657437108 , wealth 67312.5992603
    max: 0.431566426688 , hc : 16216.987550137701 , wealth 100406.757145
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 147595.729099
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_22_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0.01</th>
      <th>0.02</th>
      <th>0.03</th>
      <th>0.04</th>
      <th>0.05</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>97.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>97.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>93.0</td>
      <td>72.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>92.0</td>
      <td>75.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>100.0</td>
      <td>97.0</td>
      <td>80.0</td>
      <td>66.0</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>100.0</td>
      <td>84.0</td>
      <td>71.0</td>
      <td>60.0</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>89.0</td>
      <td>77.0</td>
      <td>66.0</td>
      <td>56.0</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>76.0</td>
      <td>68.0</td>
      <td>60.0</td>
      <td>52.0</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>57.0</td>
      <td>50.0</td>
      <td>44.0</td>
      <td>37.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>57.0</td>
      <td>50.0</td>
      <td>44.0</td>
      <td>37.0</td>
      <td>31.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________

# 6. Expected return of market

* expected return of market
    * 최근 3년 데이터
        * mu : 0.09, sigma : 0.12
    * 최근 5년 데이터
        * mu : 0.09, sigma : 0.13
    * 최근 10년 데이터
        * mu : 0.13, sigma : 0.17
    * 최근 20년 데이터
        * mu : 0.06, sigma : 0.19
    * 최근 30년 데이터
        * mu : 0.09, sigma : 0.17
        

* initial hc & initial wealth amount & risk aversion coefficient
    * initial hc = 5,000으로 고정
    * initial wealth amount = 5,000으로 고정
    * gamma = 4로 고정


```python
marketmu_list = [0.09, 0.09, 0.13, 0.06, 0.09]
marketsigma_list = [0.12, 0.13, 0.17, 0.19, 0.17]

df_marketglide_path = pd.DataFrame()
for i, j in zip(marketmu_list, marketsigma_list):
    glide_path = gen_glide_path(initial_wealth, initial_hc, init_period, size, mu_rf, sigma_rf, i, j, mu_h, sigma_h, rho, 4)
    df_marketglide_path = pd.concat([df_marketglide_path, glide_path], axis=1, sort=False)
df_marketglide_path.columns = list(zip(marketmu_list, marketsigma_list))
df_marketglide_path
```

    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 9616.39971813
    max: 1.0 , hc : 7401.221424591721 , wealth 17135.9870917
    max: 1.0 , hc : 9004.717527534582 , wealth 29212.7643751
    max: 1.0 , hc : 10955.615715167101 , wealth 48411.1775801
    max: 0.984949674104 , hc : 13329.181657437108 , wealth 78700.7407046
    max: 0.917325859635 , hc : 16216.987550137701 , wealth 125869.824784
    max: 0.69983887248 , hc : 19730.4449710597 , wealth 196890.172189
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_26_1.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 9616.39971813
    max: 1.0 , hc : 7401.221424591721 , wealth 17135.9870917
    max: 1.0 , hc : 9004.717527534582 , wealth 29212.7643751
    max: 0.906928351602 , hc : 10955.615715167101 , wealth 48411.1775801
    max: 0.841469046188 , hc : 13329.181657437108 , wealth 77365.8887964
    max: 0.788957252637 , hc : 16216.987550137701 , wealth 120594.221085
    max: 0.595019131003 , hc : 19730.4449710597 , wealth 184474.608623
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_26_3.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 1.0 , hc : 6083.264512000001 , wealth 11515.2198706
    max: 1.0 , hc : 7401.221424591721 , wealth 24018.0513225
    max: 1.0 , hc : 9004.717527534582 , wealth 47660.7703754
    max: 0.891391337367 , hc : 10955.615715167101 , wealth 91959.5321003
    max: 0.838087087036 , hc : 13329.181657437108 , wealth 167870.279671
    max: 0.801276654804 , hc : 16216.987550137701 , wealth 297760.757853
    max: 0.701086805463 , hc : 19730.4449710597 , wealth 518043.053146
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_26_5.png)


    max: 0.232967200784 , hc : 5000 , wealth 5000
    max: 0.17016810842 , hc : 6083.264512000001 , wealth 8065.64490685
    max: 0.140780916322 , hc : 7401.221424591721 , wealth 12334.4887142
    max: 0.138262124628 , hc : 9004.717527534582 , wealth 18225.4821796
    max: 0.13098030585 , hc : 10955.615715167101 , wealth 26306.5031781
    max: 0.109299196599 , hc : 13329.181657437108 , wealth 37301.9072874
    max: 0.120582797817 , hc : 16216.987550137701 , wealth 52131.166772
    max: 0.0621307548763 , hc : 19730.4449710597 , wealth 72121.1067413
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_26_7.png)


    max: 1.0 , hc : 5000 , wealth 5000
    max: 0.818885270937 , hc : 6083.264512000001 , wealth 9616.39971813
    max: 0.675599472991 , hc : 7401.221424591721 , wealth 16574.042576
    max: 0.610830701543 , hc : 9004.717527534582 , wealth 26700.475105
    max: 0.557973641009 , hc : 10955.615715167101 , wealth 41454.3912295
    max: 0.51816601515 , hc : 13329.181657437108 , wealth 62657.9642334
    max: 0.489155302171 , hc : 16216.987550137701 , wealth 92869.4512366
    max: 0.34383740087 , hc : 19730.4449710597 , wealth 135665.166195
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_26_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>(0.09, 0.12)</th>
      <th>(0.09, 0.13)</th>
      <th>(0.13, 0.17)</th>
      <th>(0.06, 0.19)</th>
      <th>(0.09, 0.17)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>23.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>23.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>17.0</td>
      <td>82.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>14.0</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>14.0</td>
      <td>61.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>100.0</td>
      <td>91.0</td>
      <td>89.0</td>
      <td>13.0</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>98.0</td>
      <td>84.0</td>
      <td>84.0</td>
      <td>11.0</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>92.0</td>
      <td>79.0</td>
      <td>80.0</td>
      <td>12.0</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>70.0</td>
      <td>60.0</td>
      <td>70.0</td>
      <td>6.0</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>70.0</td>
      <td>60.0</td>
      <td>70.0</td>
      <td>6.0</td>
      <td>34.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________

# 7. income & market rho

* income & market rho
    * 0.1, 0.2, 0.5, 0.7, 1로 변경하면서 glide path 구성
        

* initial hc & initial wealth amount & risk aversion coefficient
    * initial hc = 5,000으로 고정
    * initial wealth amount = 5,000으로 고정
    * gamma = 4로 고정


```python
rho_list = [0.1, 0.2, 0.5, 0.7, 1]
df_rhoglide_path = pd.DataFrame()
for i in rho_list:
    glide_path = gen_glide_path(initial_wealth, initial_hc, init_period, size, mu_rf, sigma_rf, mu_market, sigma_market, mu_h, sigma_h, i, 4)
    df_rhoglide_path = pd.concat([df_rhoglide_path, glide_path], axis=1, sort=False)
df_rhoglide_path.columns = rho_list
df_rhoglide_path
```

    max: 0.980696261087 , hc : 5000 , wealth 5000
    max: 0.730434499018 , hc : 6083.264512000001 , wealth 10021.6046094
    max: 0.600817959036 , hc : 7401.221424591721 , wealth 17477.8888487
    max: 0.541330115147 , hc : 9004.717527534582 , wealth 28404.836047
    max: 0.493855904984 , hc : 10955.615715167101 , wealth 44435.3196623
    max: 0.458599174175 , hc : 13329.181657437108 , wealth 67627.9850525
    max: 0.432916282484 , hc : 16216.987550137701 , wealth 100895.58976
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 148333.11828
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_30_1.png)


    max: 0.970448460818 , hc : 5000 , wealth 5000
    max: 0.724282117595 , hc : 6083.264512000001 , wealth 9998.26511065
    max: 0.596988810088 , hc : 7401.221424591721 , wealth 17417.8695247
    max: 0.538253924435 , hc : 9004.717527534582 , wealth 28291.5419045
    max: 0.49164288173 , hc : 10955.615715167101 , wealth 44239.5004746
    max: 0.45687283926 , hc : 13329.181657437108 , wealth 67312.5992603
    max: 0.431566426688 , hc : 16216.987550137701 , wealth 100406.757145
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 147595.729099
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_30_3.png)


    max: 0.939020878957 , hc : 5000 , wealth 5000
    max: 0.706053678174 , hc : 6083.264512000001 , wealth 9926.95941418
    max: 0.584723545784 , hc : 7401.221424591721 , wealth 17237.5480819
    max: 0.529559825589 , hc : 9004.717527534582 , wealth 27947.2813198
    max: 0.485051596918 , hc : 10955.615715167101 , wealth 43652.6468075
    max: 0.451733333058 , hc : 13329.181657437108 , wealth 66369.8623024
    max: 0.427909911231 , hc : 16216.987550137701 , wealth 98948.2669608
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 145410.880708
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_30_5.png)


    max: 0.918259737771 , hc : 5000 , wealth 5000
    max: 0.693624066786 , hc : 6083.264512000001 , wealth 9880.0782621
    max: 0.576405926057 , hc : 7401.221424591721 , wealth 17117.8609372
    max: 0.523478154844 , hc : 9004.717527534582 , wealth 27718.5744211
    max: 0.480350994104 , hc : 10955.615715167101 , wealth 43260.8916353
    max: 0.448108029736 , hc : 13329.181657437108 , wealth 65737.4405476
    max: 0.425391119536 , hc : 16216.987550137701 , wealth 97967.6655905
    max: 0.309828603044 , hc : 19730.4449710597 , wealth 143941.914789
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_30_7.png)


    max: 0.886079197674 , hc : 5000 , wealth 5000
    max: 0.673943848755 , hc : 6083.264512000001 , wealth 9807.76093252
    max: 0.563183857889 , hc : 7401.221424591721 , wealth 16932.2668537
    max: 0.514124734168 , hc : 9004.717527534582 , wealth 27363.3659369
    max: 0.473273020953 , hc : 10955.615715167101 , wealth 42655.3492369
    max: 0.442544259466 , hc : 13329.181657437108 , wealth 64765.2620158
    max: 0.421593182723 , hc : 16216.987550137701 , wealth 96463.5259287
    max: 0.309687181688 , hc : 19730.4449710597 , wealth 141694.083298
    


![png]({{ site.baseurl }}/assets/img/post/20181206/output_30_9.png)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0.1</th>
      <th>0.2</th>
      <th>0.5</th>
      <th>0.7</th>
      <th>1.0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2060</th>
      <td>98.0</td>
      <td>97.0</td>
      <td>94.0</td>
      <td>92.0</td>
      <td>89.0</td>
    </tr>
    <tr>
      <th>2055</th>
      <td>98.0</td>
      <td>97.0</td>
      <td>94.0</td>
      <td>92.0</td>
      <td>89.0</td>
    </tr>
    <tr>
      <th>2050</th>
      <td>73.0</td>
      <td>72.0</td>
      <td>71.0</td>
      <td>69.0</td>
      <td>67.0</td>
    </tr>
    <tr>
      <th>2045</th>
      <td>60.0</td>
      <td>60.0</td>
      <td>58.0</td>
      <td>58.0</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>2040</th>
      <td>54.0</td>
      <td>54.0</td>
      <td>53.0</td>
      <td>52.0</td>
      <td>51.0</td>
    </tr>
    <tr>
      <th>2035</th>
      <td>49.0</td>
      <td>49.0</td>
      <td>49.0</td>
      <td>48.0</td>
      <td>47.0</td>
    </tr>
    <tr>
      <th>2030</th>
      <td>46.0</td>
      <td>46.0</td>
      <td>45.0</td>
      <td>45.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>43.0</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
  </tbody>
</table>
</div>



_______________________________________


```python
total_change_plot(df_hcglide_path, df_weathglide_path, df_gammaglide_path)
```


![png]({{ site.baseurl }}/assets/img/post/20181206/output_32_0.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_32_1.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_32_2.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_32_3.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_32_4.png)



```python
total_change_plot(df_rfglide_path, df_marketglide_path, df_rhoglide_path)
```


![png]({{ site.baseurl }}/assets/img/post/20181206/output_33_0.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_33_1.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_33_2.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_33_3.png)



![png]({{ site.baseurl }}/assets/img/post/20181206/output_33_4.png)

