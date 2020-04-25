---
layout: post
title: Quantile-Quantile-Plot
subtitle: Comparing to Normal Distribution.

tags: [EDA,Statistics, ]
---


In statistics, a probability plot is a graphical technique for comparing two data sets, either _two sets of empirical observations_, _one empirical set against a theoretical set, or (more rarely) two theoretical sets against each other._ So we mentioned 3 different cases of choosing a couple of distribution to use them in both axis **x** and axis **y**.

In this specific case, we will try the case of testing 1 empirical dataset distribution against a theoretical distribution (**Normal Standard Distribution**). For this, let's use the plobplot function of the _scipy_ package. We have to mention that there are other packages in order to make a Q-Q plot.

```python
import scipy.stats as stats
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
from seaborn_qqplot import qqplot

#We are selecting logarithm base _e_ of Gold Values. Filtering from -4.01 to 2050 which are the 0.0035 quantile and the 0.9965 quantile left out after the bloxplot analysis of the logarithm (base e) of quantVal Gold Values
df3 = df2.loc[df2['quantType'] == 'Gold']
df3=df3[(df3['log2'] < 2050) & (df3['log2']> -4.01)]

plt.figure(figsize=(12,12))
stats.probplot(df3["log2"], dist=norm, plot=plt)

plt.title("Normal Q-Q plot")
plt.xlim([-5, 5])
plt.ylim([-10, 15])
plt.show()
```

Base upon this Q-Q Plot we can see that out distribution is definitively not Normal, but further analysis must be carried out to select our data as Normal as possible.