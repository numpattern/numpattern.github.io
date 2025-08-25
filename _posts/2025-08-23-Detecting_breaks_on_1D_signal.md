---
layout: post
title: When Signals Break - Detecting Discontinuities.
#subtitle: .
tags: [Signal Processing]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---

Signals aren’t always smooth. They present irregularities such as sudden drops, peaks or subtle shifts that break the pattern. These features mark important transitions wether the data represent financial markets, 
seismic data, geological scans and others. The detection of these patterns require precision and the 
right analytical approach. Algorithms are tailored to specific applications. Among them, change point detection and peak detection stand out. Though different in scale and focus, both aim to uncover hidden structure within the signal. Understanding when and how to apply each method is key to extracting meaningful insights. This post explores how signal irregularities reveal deeper truths—and why choosing the right detection strategy can make all the difference.

Pruned Exact Linear Time detects changepoints — especially when you're tracking shifts in the mean level of a signal. 

<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center; margin-right: 10px;">
    <img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/pelt_on_signal.jpg?raw=true" alt="Image 1" style="width: 550px;">
  </div>
</div>

<img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_04.JPG?raw=true" style="width: 100%; height: auto;">

However if your data shows thin, sharp triangular peaks—those fleeting, high-intensity blips—PELT might not be your best ally. These aren't level shifts; they're transient, 
 localized anomalies. In such cases, you're better off exploring methods tailored to pinpoint short-lived disruptions rather than sustained changes.