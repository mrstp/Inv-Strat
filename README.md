# Inv-Strat

import pandas as pd
import numpy as np
source = 'file.xlsx'
df=pd.read_excel(source)
df=df[['Var1']]

# 20 day moving average
df['20sma'] = df['Var1'].rolling(20).mean()

# returns as (closing price/closing of the day before) -1
df['returns'] =df['Var1']/df['Var1'].shift(1)-1

# index +1 if closing price is > than the 20sma, index -1 if closing price is < than the 20sma
df.loc[df['Var1']>df['20sma'],'index'] = 1
df.loc[df['Var1']<df['20sma'],'index'] = (-1)

# returns for the day of a strategy going long or short 
df['str_ret'] = df['returns']*df['index'].shift(1)

# mean,minimum and maximum return
df["str_ret"].mean()
df["str_ret"].min()
df["str_ret"].max()

# annualized standard deviation of returns
df["str_ret"].std()*(252**0.5)
Sharpe ratio
df["str_ret"].mean()/df["str_ret"].std()

# kurtosis and skewness
from scipy.stats import kurtosis
from scipy.stats import skew
df["str_ret"].skew()

df["str_ret"].kurtosis()


# cumulative performance of both the L/S strategy and the long only
df["cum_str_ret"] = df["str_ret"].cumsum()
df["cum_ret"] = df["returns"].cumsum()

# plot of both charts
df["cum_str_ret"].plot.line()
df["cum_ret"].plot.line()

