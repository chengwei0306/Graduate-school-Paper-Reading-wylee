### Graduate-school-Paper-Reading
### Lecture wylee

- Install yfinance
```python
!pip install yfinance
```

- Get stock informations of Apple from  2021-11-15 to 2022-11-15
```python
import pandas as pd                                #導入 pandas套件
import yfinance as yf                              #導入 finance套件

tickers = ['AAPL']   #設置股票代碼
start_date = '2021-11-15'                          #設定開始日期
end_date = '2022-11-15'                            #設定結束日期

data = yf.download(tickers, start_date, end_date)  #獲取股票數據存取變量
data['Date']=data.index

data=data[['Adj Close','Volume','Date']]               #取出調整後收盤價
Close=list(data.iloc[:,0])
Volume=list(data.iloc[:,1])
Date=list(data.iloc[:,2])
# print(len(Close))
print(Volume)
```
- Set Volume as mean 
```python
import numpy as np
V0=np.mean(Volume)
V0
```
