---
layout: post
title: Detect breaks on unidimensional signals.
#subtitle: .
tags: [Signal Processing]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---


PELT is a powerful algorithm for detecting changepoints—especially when you're tracking shifts in the mean level of a signal. However if your data
 shows thin, sharp triangular peaks—those fleeting, high-intensity blips—PELT might not be your best ally. These aren't level shifts; they're transient, 
 localized anomalies. In such cases, you're better off exploring methods tailored to pinpoint short-lived disruptions rather than sustained changes.


<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center; margin-right: 10px;">
    <img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/pelt_on_signal.jpg?raw=true" alt="Image 1" style="width: 210px;">
  </div>
  <div style="text-align: center; margin-right: 10px;">
    <img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_02.JPG?raw=true" alt="Image 2" style="width: 210px;">
  </div>
</div>


<img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_04.JPG?raw=true" style="width: 100%; height: auto;">