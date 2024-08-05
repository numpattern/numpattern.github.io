---
layout: post
title: Rock Particles Characterization via Images
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [Segmentation]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---


Rock particles results from erosion, blasting, comminution, and others. In all cases, the geometric characteristics of these particles encode information about the process that generated them. The mineralogy
influences the mechanical properties that determine the shape and size of individual particles. Digital images are first partitioned into its constituents prior analysis. 

Segment Anything (SAM), Mask R-CNN and DeepLab are segmentation models. Their applicability for rock particle analysis depend on the requirements of the task, flexibility, customization, resource efficiency among others.
SAM excels in zero-shot scenarios, however it is resource-intensive due to its architecture. The example shows a zero-short result on an image with numerous clasts. The coloured masks are SAM's outputs to partition the image. 


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


The metrics of the constituents are obtained from their mask's outlines. These features include solidity, area, extent and eccentricity.

<img src="https://github.com/numpattern/numpattern.github.io/blob/main/img/rockparticle_04.JPG?raw=true" style="width: 100%; height: auto;">