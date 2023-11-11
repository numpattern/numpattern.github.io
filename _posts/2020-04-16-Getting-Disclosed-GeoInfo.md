---
layout: post
title: Disclosed Geological Data
#subtitle: From Beginning
bigimg: /img/per010rz.jpg
tags: [Database, GIS, geopandas, Access]
share-img: "https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/bigdataspace.jpg"
---


There is plenty of geological information online, some of them can be downloaded from:

- [Victoria State Government - Earth Resources publications](http://earthresources.efirst.com.au/product.asp?pID=1016&cID=12)
- [USGS GeoSpatial Datasets](https://mrdata.usgs.gov/catalog/science.php?thcode=2&term=474)

Usually, this datasets come in a variety of formats such as GIS, binary, text, .dbf, .mdb, .hdf, among others), then we must be familiarized with them to proceed with the analysis.

### Open .dbf files in Python using geopandas
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


#### Plot shapes and tiles with geopandas

Geopandas comes with its own example database, the snippet allows to plot geo-referenced geometries.

```python
import geopandas
import pandas as pd
df = geopandas.read_file(geopandas.datasets.get_path('nybb'))
_ = df.plot(alpha=0.5, edgecolor='k')
```
![Result](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/gdp_plot.png){: .center-block :}


The package contextily retrieves online tile maps that can be embeded in the plots. Use the conda-forge channel to install contextily and geopandas, and then `pip install descartes` in a conda environment with python3.7. 

![Contextily](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/Contextily.PNG){: .center-block :}


### Open Micrsoft Access Database files (mdb, accdb)
___

First, a connection must be established to access database files. The ODBC (Open Database Connection) is an API to access any database. Further details to set the connection can be found [here](https://github.com/mkleehammer/pyodbc/wiki/Connecting-to-Microsoft-Access). The next code checks the installed ODBC drivers in our computer.
```python
import pyodbc
[x for x in pyodbc.drivers() if x.startswith('Microsoft Access Driver')]
```

An empty list means the lack of any compatible Access Database Driver on your machine, despite having Office installed. For that, The Microsoft Access Database Engine 2010 Redistributable can be downloaded from Microsoft webpage, after installationan ODBC driver will be available. Ensure to install the correct architecture compatible for your machine and Python. In my case, it was 64bits for Office and Python. 

Common issues such as General error Unable to open registry key Temporary (volatile) …” from Access ODBC_ can be encountered, and some advices are found [here](https://stackoverflow.com/questions/26244425/general-error-unable-to-open-registry-key-temporary-volatile-from-access). Verify user permissions to the path of your Access file, relocating the Access Database file in the working directory may help. The read_sql function from pandas. It is easier than using the old cursor (for Dataframes purpose)

```python
import pyodbc
import pandas as pd
conn_str = (
    r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};' #Driver for 64b mdb
    r'DBQ=path\to\your\file\Geochemistry.mdb;')
conn = pyodbc.connect(conn_str)

SQL='SELECT * FROM GSITEASSAY'
df = pd.read_sql(SQL, conn)
conn.close()
```

Below is the information in the database. 
![Dataframe](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/post002_dataframe.PNG){: .center-block :}


[Note](https://pbpython.com/pandas_dtypes.html) _Use this link to follow instructions how to deal with non clean database from scratch regarding al dtypes on your dataframe._
