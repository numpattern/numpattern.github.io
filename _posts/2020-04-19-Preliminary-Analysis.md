---
layout: post
title: Preliminary Data Analysis
subtitle: Histograms and Boxplots
tags: [EDA,Statistics, ]
---

## Analyzing Tabular Data

_What do we understand by tabular data?_ It means that our data is arranged in rows and columns. Pandas Dataframe is a good package to deal with this format as it is designed to handle data which is arranged in a set of labeled columns and numbered by row indexes; the columns can have different data types, but elements in a single column must be the same datatype. In this post we will use the information that we extracted from the **Getting Disclosed GeoInfo** post from the Access Database from Victoria State Goverment. As a first example let's evaluate the _quantVal_ column.

```python
df_quantVal = df['quantVal'].astype('float64')
df_quantVal.describe()
```

![df_quantVal_Dataframe](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/post002_dataframe.PNG){: .center-block :}


We know forehead that there are some columns that hold wrong data types due to the interpretation that pandas gave to them. Prior further analysis we must ensure that out data types are correct. Re-assigning data types would be faster and in fact  every operation would be faster in a numpy array but so far let's do it with a Dataframe.

```python
#Getting column names and data types in a dictionary so we can use the astype method in order
#to change the datatype
keys = list(df.columns)
keys_values = list(df.dtypes)
dtype_dict = dict(zip(keys, keys_values))
dtype_dict["quantVal"] = 'float' #additional changes to the original data types

df1 = df.astype(dtype_dict)
df1.dtypes
```

<center>

| Column name   | Data type |
| ------------- | --------- |
| siteId        | int64     |
| id            | int64     |
| gcSampleId    | int64     |
| quantVal      | float64   |
| repeatCd      | object    |
| bdlFlag       | object    |
| batchSampleId | int64     |
| batchSeqId    | int64     |
| quantType     | object    |
| unitId        | int64     |
| unitCd        | object    |
| unitbdlFlag   | object    |
| maxValue      | int64     |
| minValue      | int64     |

</center>

We now can evaluate de resulted statistics from the _quantVal_ column. At first glance, we have minimum values as 0.0001 and maximun as 710 000, and the Q3 is 52.9 so It seems from the description below that we have a strongly skewed data.

<center>

Statistics | Value
--- | ---
count | 672587
mean  | 144.84
std   | 2628.23
min   | 0.0001 
25%   | 4.60
50%   | 18.00
75%   | 52.90
max   | 710000.00

</center>

### Histograms and Cumulative Ditrubutions

Let's take a first sight of our _quantVal_ filtered by _quantType_ column = 'Gold', by means of histograms of the original values, cumulative distribution of the log(base e) of the values and histogram of the ln of the original values

```python
import numpy as np

df1 = df1[['quantVal','quantType']]
df1 = df1.loc[df1['quantType'] == 'Gold']
df1['ln_quantVal'] = np.log(df1['quantVal'])
```


```python
import matplotlib.pyplot as plt
import numpy as np

plt.subplot(221)
plt.hist(df1['quantVal'], facecolor='red', bins=np.linspace(0,100,100),alpha=1,density=True,edgecolor='black',label='Gold')
plt.xlabel('Gold values'); plt.ylabel('Frequency'); plt.title('Gold')
plt.legend(loc='upper left')
plt.grid(True)

plt.subplot(222)
plt.hist(df1['quantVal'], facecolor='red',histtype='stepfilled', cumulative=True,bins=np.linspace(0,20,100), alpha=0.3, density=True,edgecolor='black',label='Gold')
plt.xlabel('Gold values'); plt.ylabel('Cumulative frequency'); plt.title('Gold Cumulative Distribution')
plt.legend(loc='upper left')
plt.grid(True)

plt.subplot(223)
plt.hist(df1['loggs'], facecolor='red', bins=np.linspace(-7.5,7.5,100),alpha=1,density=True,edgecolor='black',label='Gold')
plt.xlabel('ln(Gold values'); plt.ylabel('Frequency'); plt.title('Gold')
plt.legend(loc='upper left')
plt.grid(True)

plt.subplots_adjust(left=1.0, right=3.0, wspace=0.2, hspace=0.5)
plt.show()
```

![Histograms](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/histogram.PNG){: .center-block :}

## Boxplots

By means of a boxplot we will get an insight of the distributions of all the Data we have in our Categorical Datatable with values.

```python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set(style="ticks", color_codes=True)
sns.set_context("paper")

dims = (11.7,25) #in inches
fig, ax = plt.subplots(figsize=dims)

df2["log2"]=np.log(df2["quantVal"])
sns.boxplot(x="quantVal",y="quantType",ax=ax,data=df2)

plt.xlabel('boxplots'); plt.title('Boxplots of all quantTypes')
plt.xscale('log')
plt.savefig('book.jpg')
plt.show()
```

![Boxplotfig](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/boxplot.PNG){: .center-block :}


#### Notification
{: .box-note}
**Note:** This is a notification box.
_if there is a high number of values below detection (<25%) there is no chance that this data will approach a normal or lognormal distribution_ [Riemman,1999](https://link.springer.com/article/10.1007/s002549900081)


[test for normalilty](https://machinelearningmastery.com/a-gentle-introduction-to-normality-tests-in-python/)

Best Regards,

**_Harold G. Velasquez_**.
_Geoscientist_