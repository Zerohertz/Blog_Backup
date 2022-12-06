---
title: Python-based Frequency Analysis of Acceleration Signal
date: 2022-10-26 21:08:51
categories:
- Python
- Basic
tags:
- Python
- Mechanical Engineering
mathjax: true
---
# Introduction

![스크린샷 2022-10-28 오후 2 31 53](https://user-images.githubusercontent.com/42334717/198510356-021c07b9-b667-4262-bf1d-4175850326c7.png)

<!-- More -->

***

# Load CSV Dataset

~~~python
import pandas as pd
Raw_Data = pd.read_csv('Raw_Data.csv')
Raw_Data.head()
~~~

![Load CSV Dataset](https://user-images.githubusercontent.com/42334717/197938179-7a5d867f-0d83-49db-ad8c-7995dd6cf6bf.png)

+ `pd.read_csv()`: Dataset load
+ `pd.head()`: Load 된 데이터 확인

## Slicing

~~~python
df = pd.concat([Raw_Data.iloc[:, 0], Raw_Data.iloc[:, 4], Raw_Data.iloc[:, 5], Raw_Data.iloc[:, 6]], axis = 1)
df.rename(columns = {'Target(Class)' : 'Label'}, inplace = True)
df.head()
~~~

![Slicing](https://user-images.githubusercontent.com/42334717/197940966-d7fcfecc-6075-4459-91dc-50a191fd6fa5.png)

+ `pd.concat()`: 필요한 columns를 선정하여 새로운 DataFrame 생성

## Split & Unit Conversion

![image](https://user-images.githubusercontent.com/42334717/197956602-7bcc78a7-330f-4fb1-a160-66116d6bee2a.png)

## Export data to csv

~~~python
df1.to_csv('Data/SlicedData/df1.csv', sep = ',')
df2.to_csv('Data/SlicedData/df2.csv', sep = ',')
df3.to_csv('Data/SlicedData/df3.csv', sep = ',')
df4.to_csv('Data/SlicedData/df4.csv', sep = ',')
~~~

***

# Data Visualization

~~~python
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt

def readDF(name):
    df = pd.read_csv('Data/SlicedData/' + name + '.csv')
    df = df.rename(columns = {'Unnamed: 0' : 'Index'})
    df = df.set_index('Index', drop = True)
    return df

df1 = readDF('df1')
df2 = readDF('df2')
df3 = readDF('df3')
df4 = readDF('df4')

returnTime = lambda df: np.linspace(0, len(df) / 2 - 0.5, len(df))
time1 = returnTime(df1)
time2 = returnTime(df2)
time3 = returnTime(df3)
time4 = returnTime(df4)

mpl.rc('font', family = 'Times New Roman')

tit = ("Sudden acceleration", "Sudden right turn", "Sudden left turn", "Sudden break")
plt.figure(figsize = (10, 10))

for i, df in zip(range(1, 5), [df1, df2, df3, df4]):
    plt.subplot(2, 2, i)
    t = returnTime(df)
    plt.plot(t, df.AccX, 'r', t, df.AccY, 'b', t, df.AccZ, 'g')
    plt.title(tit[i-1])
    plt.xlabel('Time [sec]')
    plt.ylabel('Acceleration [m/$s^2$]')
    plt.legend(['AccX', 'AccY', 'AccZ'])
    plt.ylim([-15, 15])
    plt.grid()
plt.show()
~~~

![Data Visualization](https://user-images.githubusercontent.com/42334717/197956125-f2f61dba-2777-4c83-852a-6b3991745f7a.png)