---
layout: post
title: Disclosed Geological Data
#subtitle: From Beginning
bigimg: /img/per010rz.jpg
tags: [Database, GIS, geopandas, Access]
share-img: "https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/bigdataspace.jpg"
---

## Getting Data we can work with
______

There is plenty of geological information online, some of them include:

- [Victoria State Government - Earth Resources publications](http://earthresources.efirst.com.au/product.asp?pID=1016&cID=12)
- [USGS GeoSpatial Datasets](https://mrdata.usgs.gov/catalog/science.php?thcode=2&term=474)

Usually, this datasets come in a variety of formats such as GIS, binary, text, .dbf, .mdb, .hdf, among others), then we must be familiarized with them to proceed with the analysis.

## Open .dbf files in Python using geopandas
______

Now we will see first how to import .dbf files. Note that the dBASE table (.dbf) file is one of the three files required for a valid ESRI Shapefile. One option to import .dbf files is through [geopandas](https://geopandas.org/) that can be installed with `conda install geopandas` . Geopandas may cause conflicting issues in some conda environments, it is advisable to retain in its own environment if you are unsure. Optionally you can run the code snippet below. Geopandas installation requires descartes package, so you may need to install it as well. 

```python
conda create -n yourenv
conda activate yourenv
conda config --env --add channels conda-forge
conda config --env --set channel_priority strict
conda install python=3 geopandas
```

Once geopandas is installed, the WaterLab data is imported as:

```python
import geopandas
df = geopandas.read_file('WaterLab.dbf')
```

The column named geometry in geopandas tables stores shapes. WaterLab data did not present content in the geometry column.

![Result](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/Geopandas_table.PNG){: .center-block :}


### What else we can you with geopandas
___

Geopandas comes with its own example database, the snippet below allows to plot geo-referenced geometries.

```python
import geopandas
import pandas as pd
df = geopandas.read_file(geopandas.datasets.get_path('nybb'))
ax = df.plot(alpha=0.5, edgecolor='k')
```
![Result](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/gdp_plot.png){: .center-block :}


-Next what we want to try is the package _contextily_ which is a wonderful tool to retrieve tile maps from the Internet and embed them in our shapes. I really suggest that if you want to use geopandas and contextily you preferably create a new empty conda environment with a 3.7 python version. Then you install the _contextily_ package with conda-forge channel, geopandas and **pip install descartes**. I must mention that I tried installing the _contextily_ package into my 3.8 python with a CUDA 10.2 driver installed and I experienced lots of problems.

Below I show the results of using _contextily_ package

![Contextily](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/Contextily.PNG){: .center-block :}


## Opening Micrsoft Access Database files
___

To access Database files we are required to prior establish a connection with our database, for that purpose there is the ODBC (stands for _Open Database Connection_) which is a Standard for acccesing Databases. It's needed to have the right _ODBC Driver_ for the specific Database.

I strongly recommend the next link to give you an insight of what we need to set our connection. [mdb accdb files](https://github.com/mkleehammer/pyodbc/wiki/Connecting-to-Microsoft-Access)

We run the next code to check what ODBC drivers we have installed.
```python
import pyodbc
[x for x in pyodbc.drivers() if x.startswith('Microsoft Access Driver')]
```

I obtained a empty list which meant that I lacked of _Access Database Driver_ on my machine, even when I had the Office installed, I did not have compatible drivers so I had to install them. For that, I googled _Microsoft Access Database Engine 2010_ from the Microsoft homepage. Recall that it is important to know what is the _bit version_ of both your Office (Microsoft Access) and your Python software; they have to be the same I mean compatible. In my case, it was _64bits_ Office and _64bits_ Python. 

After a lot of problems trying to figure out why I got the next error _General error Unable to open registry key Temporary (volatile) …” from Access ODBC_ This page saved my life. [Solve Error](https://stackoverflow.com/questions/26244425/general-error-unable-to-open-registry-key-temporary-volatile-from-access). I recommend you read that after continuing, but mentioning one of the common issues... It is preferably to verify user permissions to the path of your Access file, in my case I opted to relocate my Access Database file in the working directory of my _Jupyter Notebook Environment_.

Let's now try the read_sql function from pandas. It is easier than using the old cursor (for Dataframes purpose)

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

Belows as we can see, is the result. We are now able to make SQL queries to our databases and bring them into python.

![Dataframe](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/post002_dataframe.PNG){: .center-block :}


[Note](https://pbpython.com/pandas_dtypes.html) _Use this link to follow instructions how to deal with non clean database from scratch regarding al dtypes on your dataframe._
