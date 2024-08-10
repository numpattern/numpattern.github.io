---
layout: post
title: Rock Particles Shape Analysis
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [Image Segmentation]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---


One goal of image analysis is the extraction feature information to quantify shapes, enumerate object's structures and characterize shape of structures. 
Rock particles results from erosion, blasting, comminution, and others. The geometric characteristics of these particles encode information about its genesis. The mineralogy
influences the mechanical properties that determine the shape and size of individual particles. Digital images are first partitioned into its constituents prior analysis. 

The applicability of segmentation models sich as Segment Anything (SAM), Mask R-CNN and DeepLab for rock particle analysis depend on the requirements of the task, flexibility, customization, resource efficiency.
SAM excels in zero-shot scenarios, however it is resource-intensive due to its architecture. The example shows a zero-short result on an image with numerous clasts. The coloured masks are SAM's outputs to partition the image. 
The required self-descriptve metrics are calculated for the red contours to derive shape descriptors. 


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


The table shows the metrics of the constituents obtained from their mask's outlines. These include solidity, sphericity, convexity, compactness, circularity, etc. Solidity measures the density of an object, 
a circle has a solidity of 1. Sphericity measures the similarity of an object to a sphere, a circle has a value of 1. Convexity measures the similartiy of an object to a convex shape, a convex object has a value of 1. 
Compactness measures how  tightly an object's  area is distributed around its centroid, a circle has a value of 1. Circularity or roundness differs to compactness as it excludes local irregularities by
considering the convex perimiter of the object.


<img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_04.JPG?raw=true" style="width: 100%; height: auto;">