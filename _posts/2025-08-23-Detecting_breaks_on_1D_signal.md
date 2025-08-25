---
layout: post
title: When Signals Break - Detecting Discontinuities.
#subtitle: .
tags: [Signal Processing]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---

Signals aren’t always smooth. They present irregularities such as sudden drops, peaks or subtle shifts that break the pattern. These features mark important transitions wether the data represent financial markets, 
seismic data, geological scans and others. Detecting of these patterns requires precision and a 
correct analytical approach. Available algorithms are tailored to specific applications. Among them, change point detection and peak detection stand out. Though different in scale and focus, both aim to uncover hidden structure within the signal. Understanding when and how to apply each method is key to extracting meaningful insights. This post explores how signal irregularities reveal deeper truths and why choosing the right detection strategy can make all the difference.

**Pruned Exact Linear Time (PELT)** detects shifts in the mean level of a signal. It detects changepoints in time series data by minimizing a cost function that balances model fit and complexity. The complexity refers to the number of change points detected. It’s especially effective for identifying shifts in mean or variance. PELT uses dynamic programming with a clever pruning strategy to discard unlikely changepoint candidates, drastically reducing computation time. Unlike brute-force methods, it guarantees an exact solution while scaling linearly with data size under certain conditions. This makes it ideal for large datasets where speed and accuracy are crucial. 

<img src="https://raw.githubusercontent.com/numpattern/numpattern.github.io/main/img/Signalbreaks_pelt_on_synthetic_signal.jpg" style="width: 100%; height: auto;">

A higher penalty means PELT is more conservative—it avoids adding change points unless the fit improves significantly. A lower penalty makes it more permissive, allowing more change points even for small improvements.

PELT might not be your best ally for cases with narrow, sharp triangular peaks. These aren't level shifts; they're transient, localized anomalies. In such cases, methods tailed to pinpoint short-lived disruptions rather than sustained changes are the way to go. The image shows PELT stturgling to detect those features. The narrow asymmetric peaks can represent cracks or anomalies.

<img src="https://raw.githubusercontent.com/numpattern/numpattern.github.io/main/img/Signalbreaks_pelt_on_signal.jpg" style="width: 100%; height: auto;">