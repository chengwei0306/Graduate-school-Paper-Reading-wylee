Paper-Reading Lecturer:Wylee

### 罕見事件預測:以APPLE股價（調整收盤價）做分析

### 文章原文：
節錄自（Rare Events Analysis for High-Frequency Equity Data Written by Dragos Bozdog, Ionu ̧t Florescu, Khaldoun Khashanah, and Jim Wang)  
- A thorough investigation of the distribution of price changes, conditional on cumulative trading window, would involve the evaluation of all observa- tions for each equity (Sn – Sj|vk + vk+1 + . . . + vn < V0) for k ≤ j ≤ n where n runs through all the trades, Sn is the price, and vn is the volume associated with trade n. Although the distribution would be accurate, such a task would involve significant computational effort considering the large database we use. 
- (Motivation) Because we are interested only in the rare events, we chose to select the extreme observations in this sequence and subsequently analyze only a per- centage of these observations at the tails of the distribution ΔP| V < V0.
<br><br>
- Specifically, we construct the sequence of consecutive trades Sk,Sk+1, . . ., Sn and their associated volumes vk , vk+1, . . . , vn such that vk + vk+1+ . . . + vn < V0 and we consider<br><br>
Δpn = max{Sn – Sk, Sn – Sk+1, . . ., Sn – Sn–1}
<br><br>
- We repeat the process for every trade by calculating a corresponding maximum price movement within the last V0 trades. Once we obtain these values for the entire sequence of trades, we detect the extreme observations by applying a univariate “quantile-type” rule. Namely, for a fixed level α we select all the observations in the set:<br><br>
Q+α(x) = {x:prob(Δp < x) < α or prob (Δp > x) > 1 – α} (1)
<br><br>
- Using rule (1) with returns instead of change in price is preferable in a trad- ing environment. We use change in price (Δp) for clarity of exposition.
Note: The Δpn quantities are not independent. Thus, the empirical distribution is only an approximation of the true probabilities of price movement. However, rule (1) will identify candidates for rare events which may or may not correspond to the true probability level α. If we use non- overlapping windows, the resulting rare events will correspond better to the level α. There are two reasons why this is not feasible. First, by considering non-overlapping windows we can lose extreme price differences calculated using prices from the non-overlapping windows. Second, in a previous study (Mariani et al., 2009) the authors have shown that returns calculated from tick data exhibit long memory behavior. Thus, even by considering non-overlapping windows one cannot guarantee that the observations are independent.

Python
### Environment
- Google Colab

### Dataset
- Yahoo Finance

### Abstract
- This work present a methodology to detect rare events which are defined as large price movements relative to the volume traded. We analyze the behavior of equity after the detection of these rare events. 

### Objective
- Rare Event Detection

### Process
- Install yfinance
```python
!pip install yfinance
```

- Get Apple stock informations from  2021-11-15 to 2022-11-15
```python
import pandas as pd                                #導入 pandas套件
import yfinance as yf                              #導入 finance套件

tickers = ['AAPL']   #設置股票代碼
start_date = '2021-11-15'                          #set start date
end_date = '2022-11-15'                            #set end date

data = yf.download(tickers, start_date, end_date)   #data value
data['Date']=data.index

data=data[['Adj Close','Volume','Date']]               #Get Adj Close
Close=list(data.iloc[:,0])
Volume=list(data.iloc[:,1])
Date=list(data.iloc[:,2])
# print(len(Close))
print(Volume)
```
- Set V0 as averaged Volume
```python
import numpy as np
V0=np.mean(Volume)
V0
```

- sum the volume between j --> i as volume 
- if volume<V0 <br/>
then a set append Spread <br/>
b set append date
```python
def RareEvent(n,end_date):
  a=[]
  b=[]
  volume=0
  # (Sn – Sj|vk + vk+1 + . . . + vn < V0) for k ≤ j ≤ n 
  for i in range(0,n):                  #len of all
    volume=0
    for j in range(i):                  #for k ≤ j ≤ n 
      volume+=Volume[j]                 #(vk + vk+1 + . . . + vn)
      if volume<V0:                     #(vk + vk+1 + . . . + vn < V0) 
        a.append(Close[n]-Close[i])     #(Sn – Sj)
        b.append([end_date,Date[i]])    #Q+α(x) = {x:prob(Δp < x) < α or prob (Δp > x) > 1 – α}
        
  print(max(a))                         #ΔPn = max{Sn – Sk, Sn – Sk+1, . . ., Sn – Sn–1}
  print(b[-1:])


RareEvent(251,end_date)                #251-->amount of the data
```
