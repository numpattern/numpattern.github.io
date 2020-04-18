---
layout: post
title: How can we get Disclosed Geo-scientific Data from Geological Organizations
subtitle: From Beginning
bigimg: /img/per009rz.jpg
tags: [Database, GIS, geopandas]
---

As we need geological informations in all its types and sizes for our further analysis that we will face in this blog, it becames important to get involved with different online resources in order to download different chunks of information around the world. Diving into the Internet, I found myself with the following resources I proceed to list below:


- [Victoria State Government - Earth Resources publications](http://earthresources.efirst.com.au/product.asp?pID=1016&cID=12)
- [USGS GeoSpatial Datasets](https://mrdata.usgs.gov/catalog/science.php?thcode=2&term=474)

But these Datasets come in a wide range of Dataformats (e.g GIS Data, big data, binary and so on) so we ought to be able to handle ever type of data that exist. Now we will see first how to import _.dbf_ files. The dBASE table (.dbf) file is one of the three files required for a valid ESRI Shapefile.

At this point I realized that we need to use a library to import this kind of data, we will use geopandas library which you can find [here](https://geopandas.org/). There you will find all the documentation in order to install it. I suggest that you previously you have Anaconda installed and you better use _conda_ instead of _pip_, and preferably on created new empty environment inside your Anaconda.

The next code is a good choice to run inside your Anaconda Prompt, although _conda install geopandas_ worked well for me
~~~
conda create -n geo_env
conda activate geo_env
conda config --env --add channels conda-forge
conda config --env --set channel_priority strict
conda install python=3 geopandas
~~~

In my case, after installation, I also needed the _descartes_ package so I googled it and installed.

So, in your Anaconda notebook you can run the following code
~~~
import geopandas
import pandas as pd
df = geopandas.read_file(geopandas.datasets.get_path('nybb'))
ax = df.plot(figsize=(10,10), alpha=0.5, edgecolor='k')
~~~
![Result](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/gdp_plot.png){: .center-block :}


Ok, It resulted well. I will be updating this post for other types of Data, as I look for a Dataset that fits with our next analysis. If you have questions regarding this post, feel free to contact me so I can help you.

Next what we want to try is the package _contextily_ which is a wonderful tool to retrieve tile maps from the Internet and embed it in our shapes. I really suggest that if you want to use geopandas and contextily you preferably create a new empty conda environment with a 3.7 python version. Then you install the _contextily_ package with conda-forge channel, geopandas and **pip install descartes**. I must mention that I tried installing the _contextily_ package into my 3.8 python with a CUDA 10.2 driver installed and I experienced lots of problems.

Below I show the results of using _contextily_ package

![Contextily](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .center-block :}


