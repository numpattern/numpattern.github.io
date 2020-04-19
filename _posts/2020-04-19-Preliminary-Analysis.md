---
layout: post
title: Preliminary Data Analysis
subtitle: Histograms and Boxplots
tags: [EDA,Statistics]
---

##Analyzing Tabular Data

In this post we will use the information that we extracted from the **Getting Disclosed GeoInfo** analysing different type of data. In the first example we take the Gold Assay from the Access Database that came from Victoria-Australia.

```python
df_quantVal = df['quantVal'].astype('float64')
df_quantVal.describe()
```

---|---
Statistics | Value 
--- | --- 
count |   672587.000000
mean   |     144.849423
std     |   2628.235571
min      |     0.000100
25%       |    4.600000
50%      |    18.000000
75%          52.900002
max     | 710000.000000

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


---|---
Column name | Data type
--- | ---
siteId | int64
id        |         int64
gcSampleId |        int64
quantVal    |     float64
repeatCd     |     object
bdlFlag       |    object
batchSampleId  |    int64
batchSeqId     |    int64
quantType       |  object
unitId        |     int64
unitCd         |   object
unitbdlFlag     |  object
maxValue         |  int64
minValue          | int64

Let's take a first sight of our Gold data be means of histogram and cumulative histogram

```python
import matplotlib.pyplot as plt
import numpy as np


plt.subplot(121)
plt.hist(df1['quantVal'], facecolor='red', histtype='step', bins=np.linspace(0,100,50),alpha=1,density=False,edgecolor='black',label='Gold')
plt.xlim([0,100]); 
#plt.ylim([0,])
plt.xlabel('Porosity (fraction)'); plt.ylabel('Frequency'); plt.title('Porosity Well 1 and 2')
plt.legend(loc='upper left')
plt.grid(True)

plt.subplot(122)
plt.hist(df1['quantVal'], facecolor='green',histtype='stepfilled', cumulative=True,bins=np.linspace(0,100,50), alpha=0.7, density=True,edgecolor='black',label='Gold')
plt.xlim([0,100])
plt.legend(loc='upper left')
plt.grid(True)

plt.subplots_adjust(left=0.0, bottom=0.0, right=2.0, top=1.2, wspace=0.2, hspace=0.3)
plt.show()
```

```python

```

```python

```