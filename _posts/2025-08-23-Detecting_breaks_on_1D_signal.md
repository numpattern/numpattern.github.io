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

Pruned Exact Linear Time detects changepoints. Specifically shifts in the mean level of a signal. 
<img src="https://raw.githubusercontent.com/numpattern/numpattern.github.io/main/img/Signalbreaks_pelt_on_synthetic_signal.jpg" style="width: 100%; height: auto;">
" style="width: 100%; height: auto;">

If your data shows thin, sharp triangular peaks—those fleeting, high-intensity blips— PELT might not be your best ally. These aren't level shifts; they're transient, localized anomalies. In such cases, you're better off exploring methods tailored to pinpoint short-lived disruptions rather than sustained changes.

<img src="https://raw.githubusercontent.com/numpattern/numpattern.github.io/main/img/Signalbreaks_pelt_on_signal.jpg" style="width: 100%; height: auto;">