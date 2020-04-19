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
| Statistics | Value |
| --- |:---:|
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

Column name  | Data type
--- | ---
siteId   |         int64
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


```python

```

```python

```

```python

```