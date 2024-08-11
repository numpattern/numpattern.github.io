---
layout: post
title: Rock Particles Shape Analysis
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [Image Segmentation]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---

Image analysis extracts feature information to quantify shapes, enumerate object's structures and characterize shape of structures. 
Rock particles result from erosion, blasting, comminution, and others. The geometric characteristics of these particles encode information about their generating process. The mineralogy
influences mechanical properties and determines the shape and size of individual particles. Digital images are first partitioned into constituents prior analysis. 

The applicability of segmentation models such as Segment Anything, Mask R-CNN, and DeepLab for rock particle analysis depends on the requirements of the task, flexibility, customization, resource efficiency.
The example shows a result on an image with numerous clasts. The coloured masks partition the image. The required self-descriptve metrics are calculated for the red contours to derive shape descriptors. 

<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center; margin-right: 10px;">
    <img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_01.JPG?raw=true" alt="Image 1" style="width: 210px;">
  </div>
  <div style="text-align: center; margin-right: 10px;">
    <img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_02.JPG?raw=true" alt="Image 2" style="width: 210px;">
  </div>
  <div style="text-align: center;">
    <img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_03.JPG?raw=true" alt="Image 2" style="width: 210px;">
  </div>
</div>


The table shows the metrics of the constituents obtained from masks' outlines. These include solidity, sphericity, convexity, compactness, circularity, etc. Solidity measures the density of an object, 
a circle has a solidity of 1. Sphericity measures the similarity of an object to a sphere, a circle has a value of 1. Convexity measures the similartiy of an object to a convex shape, a convex object has a value of 1. 
Compactness measures how  tightly an object's  area is distributed around its centroid, a circle has a value of 1. Circularity or roundness differs to compactness as it excludes local irregularities by
considering the convex perimiter of the object.

A sensitive and common feature come from the visible light spectrum. This may be of some use in controlled environments. The example summarizes the most characteristic colors of the clasts, adjusted by their
proportion.

<img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_04.JPG?raw=true" style="width: 100%; height: auto;">