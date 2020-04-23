---
layout: post
title: How can we get Disclosed Geo-scientific Data from Geological Organizations
subtitle: From Beginning
bigimg: /img/bigdataspace.jpg
tags: [Database, GIS, geopandas, Access]
---

## Getting Data we can work with
______

As we need geological information in all its types and sizes for our further analysis that we will be facing in this blog, it becames important to get involved with different and disclosed online resources in order to download different chunks of information from around the world. Diving into the Internet, I found the following resources I proceed now to list them below:

- [Victoria State Government - Earth Resources publications](http://earthresources.efirst.com.au/product.asp?pID=1016&cID=12)
- [USGS GeoSpatial Datasets](https://mrdata.usgs.gov/catalog/science.php?thcode=2&term=474)

But these Datasets come in a wide range of Dataformats (e.g GIS Data, Big data, binary and so on), so we ought to be able to handle whichever type of data that exist. 

I downloaded files from each of the links listed and the datafiles I found was .dbf, .mdb, .txt, .hdf, .csv and so forth. I will continue to add different kind  of formats on the progress and how to handle this data in order to getting further analysis.

### Opening .dbf files into Python using geopandas
______

Now we will see first how to import _.dbf_ files. Note that the dBASE table (.dbf) file is one of the three files required for a valid ESRI Shapefile.

At this point I realized that we need to use a library to import this kind of data, we will use geopandas library which you can find [here](https://geopandas.org/). There you will find all the documentation in order to install it. I suggest that you previously you have Anaconda installed and you better use _conda_ instead of _pip_, besides preferably you must create a new empty environment inside your Anaconda.

The next code is a good choice to run inside your Anaconda Prompt, although _conda install geopandas_ worked well for me since beginning (_I had already an empty environment created_)

```python
conda create -n yourenv
conda activate yourenv
conda config --env --add channels conda-forge
conda config --env --set channel_priority strict
conda install python=3 geopandas
```
In my case, after installation, I also needed the _descartes_ package. So it may be possible that you will be asked to install it. 

```python
import geopandas
df = geopandas.read_file('WaterLab.dbf')
df.head() #to show the first rows in our geopandas DataFrame
```

The geopandas table is different from a regular DataFrame, because it contains the column named _geometry_ where all the shapes are stored. In our case, we did not find data in our geometry object. See image below.

![Result](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/Geopandas_table.PNG){: .center-block :}


### What else we can you with geopandas
___

In your notebook, you can run the following code, this will show you how you can plot a geo-informations or georeferenced shapes. This will be discussed in future posts

```python
import geopandas
import pandas as pd
df = geopandas.read_file(geopandas.datasets.get_path('nybb'))
ax = df.plot(figsize=(10,10), alpha=0.5, edgecolor='k')
```
![Result](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/gdp_plot.png){: .center-block :}


-Next what we want to try is the package _contextily_ which is a wonderful tool to retrieve tile maps from the Internet and embed them in our shapes. I really suggest that if you want to use geopandas and contextily you preferably create a new empty conda environment with a 3.7 python version. Then you install the _contextily_ package with conda-forge channel, geopandas and **pip install descartes**. I must mention that I tried installing the _contextily_ package into my 3.8 python with a CUDA 10.2 driver installed and I experienced lots of problems.

Below I show the results of using _contextily_ package

![Contextily](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/Contextily.PNG){: .center-block :}


## Opening Micrsoft Access Database files
___

I strongly recommend the next link to give you an insight of what we need to set our connection. [mdb accdb files](https://github.com/mkleehammer/pyodbc/wiki/Connecting-to-Microsoft-Access)

When I first tried the next code, I obtained a empty list which meant that I lacked of the right _Access Database Driver_ on my machine, even when I had the Office installed. Recall that it is important to know what is the _bit version_ of both your Office and your Python. They have to be the same I mean compatible. In my case, it was 64bits Office and 64bits Python. The next code let us know all that pyodbc drivers we have available to use.

```python
import pyodbc
[x for x in pyodbc.drivers() if x.startswith('Microsoft Access Driver')]
```

I encountered an empty list from the previous python code and that meant I did not have compatible drivers so I had to install it. _Microsoft Access Database Engine 2010_ from the Microsoft homepage.

After a lot of problems trying to figure out why I got the error _General error Unable to open registry key Temporary (volatile) …” from Access ODBC_ This page saved my life. 
[Solve Error](https://stackoverflow.com/questions/26244425/general-error-unable-to-open-registry-key-temporary-volatile-from-access). I recommend you read that after continuing. It is preferably to verify user permissions to the path of your Access file, in my case I opted to locate my Access Database file in my working directory.

Let's try the read_sql function from pandas. It is easier than use the old cursor (for Dataframes purpose)

```python
import pyodbc
import pandas as pd
conn_str = (
    r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};' #Driver for 64 bits mdb
    r'DBQ=path\to\your\file\Geochemistry.mdb;' #Database
    )
conn = pyodbc.connect(conn_str)

SQL='SELECT * FROM GSITEASSAY'
df = pd.read_sql(SQL, conn)

conn.close()
```

![Dataframe](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/post002_dataframe.PNG){: .center-block :}


[Note](https://pbpython.com/pandas_dtypes.html) _Use this link to follow instructions how to deal with non clean database from scratch regarding al dtypes on your dataframe._


Ok, It resulted well. I will be updating this post for other types of Data, as I look for a Dataset that fits with our next analysis. If you have questions regarding this post, feel free to contact me so I can help you. Also, if you have some dataformats you would like to know how to handle, tell me.

Bests,

_Harold G. Velasquez
