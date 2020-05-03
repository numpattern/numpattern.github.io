---
layout: post
title: Declustering - An essential part of Resources Evaluation
subtitle: GSLIB Cell Based Method.
tags: [EDA,Statistics, Declustering ]
share-img: https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/20200502_05.PNG
---

Even modern stochastic simulation algorithms do not correct the impact of clustered data on the target histogram; these algorithms require a distribution model (histogram) that is representative of the entire volume being modeled. Simulation in an area with sparse data relies on the global distribution which must be representative of all areas being modelled.

## Cell based Declustering
___

### Background

A set of python wrappers to provide us access to GSLIB F90 executables. Thanks to GeostatsPy Functions - by **GeostatsGuy** (search him in Github) Regarding to this functions, some comments were included. Maintenance at [here](http://www.statios.com/Quick/gslib.html)  
Note: GSLIB executables: declus.exe must be in the working directory for our purposes
To find what's your working directory you can try executing %pwd

Let's make a declustering analysis on our large Gold ppb dataset from Victoria -Australia

### Requirements:

- Download the Fortran90 for X64 OS Windows package of GSLIB executables, at [here](http://www.statios.com/Quick/gslib.html)  
- Locate the declus.exe executable on your notebook working directory
- You can get your working directory by running here the code: **%pwd**
- Have a previous understanding of what we did on **Getting Disclose GeoInfo** post.
- Have a knowledge of what Declustering is and how to interpret it.

### Recomendations:

- For a better experience in mobile devices, please use the option as **Desktop Site** since I didn't do too much maintenance related to visualizations in mobiles.

### Setting wrappets for GSLIB executables

We define a function to read GeoEAS files. We also define the wrapper **declus** in order to use the declus.exe GSLIB. Notice that some of the **Parameters for Declus.exe** have been set up e.g.:

- Z value has not been considered as we are working with 2D dataset
- The _Number of origin offsets_ where its default value is 4. 
- bmin is set to 0 (which means that the program looks for the minimum declustered mean). In our example we will change to 1 instead, more of this later.


```python
import pandas as pd
import os
import numpy as np
import matplotlib.pyplot as plt                          

# utility to convert GSLIB Geo-EAS files to a pandas DataFrame for use with Python methods
def GSLIB2Dataframe(data_file):
    colArray = []
    with open(data_file) as myfile:   # read first two lines
        head = [next(myfile) for x in range(2)]
        line2 = head[1].split()
        ncol = int(line2[0])
        for icol in range(0, ncol):
            head = [next(myfile) for x in range(1)]
            colArray.append(head[0].split()[0])
        data = np.loadtxt(myfile, skiprows = 0)
        df = pd.DataFrame(data)
        df.columns = colArray
        return df
    
#This is a wrapper for Fortran90 executable declus.exe http://www.statios.com/Quick/gslib.html
def declus(df,xcol,ycol,vcol,cmin,cmax,cnum,bmin):
    import os
    import numpy as np
    nrow = len(df)
    weights = []
    file = 'declus_out.dat'
    file_out = open(file, "w")
    file_out.write('declus_out.dat' + '\n')  
    file_out.write('3' + '\n')  
    file_out.write('x' + '\n') 
    file_out.write('y' + '\n')
    file_out.write('value' + '\n')  
    for irow in range(0, nrow):
        file_out.write(str(df.iloc[irow][xcol])+' '+str(df.iloc[irow][ycol])+' '+str(df.iloc[irow][vcol])+' \n')        
    file_out.close()
    
    file = open("declus.par", "w")
    file.write("                  Parameters for DECLUS                                    \n")
    file.write("                  *********************                                    \n")
    file.write("                                                                           \n")
    file.write("START OF PARAMETERS:                                                       \n")
    file.write("declus_out.dat           -file with data                                   \n")
    file.write("1   2   0   3               -  columns for X, Y, Z, and variable           \n")
    file.write("-1.0e21     1.0e21          -  trimming limits                             \n")
    file.write("declus.sum                  -file for summary output                       \n") 
    file.write("declus.out                  -file for output with data & weights           \n")
    file.write("1.0   1.0                   -Y and Z cell anisotropy (Ysize=size*Yanis)    \n") 
    file.write(str(bmin) + "                -0=look for minimum declustered mean (1=max)   \n") 
    file.write(str(cnum) + " " + str(cmin) + " " + str(cmax) + " -number of cell sizes, min size, max size      \n")
    file.write("5                           -number of origin offsets                      \n")
    file.close()
    
    os.system('declus.exe declus.par')
    df = GSLIB2Dataframe("declus.out")
    for irow in range(0, nrow):
        weights.append(df.iloc[irow,3])    

    return(weights) 
```

### Loading the data

Couples of libraries at fist, like pyodbc to establish database connection with
SQL, Access and so forth (we already saw this type of connection in a previous post "Getting Disclose GeoInfo"). We also import pandas to handle DataFrames.

In this opportunity. we are calling 2 queries from different tables **GSITEASSAY** (containing assay values and characteristics) and **GSITEHEADER** (with location coordinates, datum and zone).

```python
import pyodbc
import pandas as pd
#Previously checked we had all the odbc drivers installed, in this case *.mdb
conn_str = (
    r'DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};' #Driver for 64 bits mdb
    r'DBQ=C:\path\to\yourfile\Geochemistry.mdb;' #Database
    )
conn = pyodbc.connect(conn_str)

SQL='SELECT * FROM GSITEASSAY;'
SQL2='SELECT siteId, calcLongitude, calcLatitude, calcDatum, calcEasting, calcNorthing FROM GSITEHEADER;'

df = pd.read_sql(SQL, conn)
df2 = pd.read_sql(SQL2, conn)
conn.close()
```

We are joining both df and df2 using the df.siteId as main index, so now
we have our 2 dataframes in one df.

Then, we select some the main columns we'll be needing in the next steps. These fields are calcEasting, calcNorthing (UTM Coordinates) , quantVal (assay values), unitCd(measurement unit), quantType(element analyzed), siteId(solely location of measurement); calcAltitude and calcLongitude(WGS84 Lat Lon coordinates)

As we saw in the previous post (Getting GeoInfo), we filter data to 
only ppb measurements of Gold.

```python
#Joining DataFrames
df = df.join(df2.set_index('siteId'), on='siteId')
#Filtering DataFrames
df = df[['calcEasting','calcNorthing','quantVal',
         'calcLongitude', 'calcLatitude', 'quantType','unitCd','siteId']]
df = df.loc[(df['quantType'] == 'Gold') & (df['unitCd'] == 'ppb')]
df.describe()
```

Below is the statistics for DataFrame df:

Param | calcEasting | calcNorthing | calcLongitude | calcLatitude | siteId
--- | --- | --- | --- | --- | ---
count | 93842.000000 | 9.384200e+04 | 93842.000000 | 93842.000000 | 93842.000000 
mean | 528222.666812 | 5.915496e+06 | 144.057888 | -36.883061 | 649464.693655 
std | 208543.295143 | 5.248331e+04 | 1.050237 | 0.473944 | 143902.457764 
min | 229027.000000 | 5.721084e+06 | 141.217100 | -38.655440 | 433360.000000 
25% | 296705.250000 | 5.880075e+06 | 143.553165 | -37.205860 | 541069.250000 
50% | 600044.500000 | 5.919088e+06 | 143.931650 | -36.854025 | 628026.500000 
75% | 738076.750000 | 5.941994e+06 | 144.699290 | -36.647520 | 667880.750000 
max | 771118.000000 | 6.035369e+06 | 147.001250 | -35.789090 | 958454.000000 


We change the datatype for quantVal column
```python
#Again we are making sure our 'quantVal' column is float
keys = list(df.columns)
keys_values = list(df.dtypes)
dtype_dict = dict(zip(keys, keys_values))
dtype_dict["quantVal"] = 'float' 

#Now let's check it out
df = df.astype(dtype_dict)
df.dtypes
#Now the 'quantVal' column is float64
```

As we did many changes and filtering, it is recommended to reset the index of our dataframe. The indexes are the numbers to the left that normally are sequential. When you do filtering or deleting some rows, these indexes doesnt change so it is better to reset them and make then consecutives.
```pythonropna()
df= df.reset_index(drop=True) 
df.isna()
```
The previous code just reseted the indexes of the DataFrame

```python
import matplotlib.pyplot as plt
import matplotlib.colors 

#Some plotting parameters
figfmt = {
    'figure.figsize' : (15, 7.5),
    'grid.color' : 'grey',
    'grid.linestyle' : '-',
    'axes.labelcolor':'white',
    'axes.titlecolor':'white',
    'axes.titlesize': '15',
    'axes.titleweight': '1000',
    'xtick.color':'white',
    'ytick.color':'white'
    #'grid.linewidth':'0.4'
}
plt.rcParams.update(figfmt)
```

### Plotting our datasets
This is not an obligatory step, but I really think it is important to get an insight of our data location. In order to get that, we use the geopandas modules.

```python
import geopandas as gpd
#gdf.crs = "EPSG:32733"
gdf = gpd.GeoDataFrame(df, geometry=gpd.points_from_xy(df.calcLongitude, df.calcLatitude))
```


```python
#These are one of the main projections we need to know to check in our geodataframes
#WGS84 Latitude/Longitude: "EPSG:4326"
#UTM Zones (North): "EPSG:32633"
#UTM Zones (South): "EPSG:32733"

#Some plotting parameters
figfmt = {
    'figure.figsize' : (10, 10)
    #'grid.linewidth':'0.4'
}
plt.rcParams.update(figfmt)

world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))
world1= world.to_crs(epsg=4326)
#Set your gdf  crs to lat lon projection as stated in our original data
gdf.crs = "EPSG:4326"

fig, ax = plt.subplots()
world1[world1.name == 'Australia'].plot(ax=ax,edgecolor='black',alpha=0.3)
gdf.plot(ax=ax, color='red', markersize=0.01)
#plt.ylim(-45,-30)
#plt.xlim(135,150)
plt.title('Map containing the assays')
plt.xlabel('Longitude (degrees)')
plt.ylabel('Latitude (degrees)')
plt.grid()
plt.show()
#As we can see our data lies in the southern zone of Australia in Victoria.
```


![Map with assay locations](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/20200502_01.PNG){: .center-block :}


Now I will show you how to make a simple scatter with coloured points of the quantValue value.
let's make am equal population legend so we need to find equals quantiles first.

```python
print('quantiles and its values')
df['quantVal'].quantile([0.15,0.3,0.45,0.60,0.75,0.9,0.99])
```

We can see now the values in different quantiles for our equal populated legend.  


Quantile | Value
--- | ---
0.15 |     1.00
0.30 |     1.84
0.45 |     3.00
0.60 |     5.00
0.75 |    10.00
0.90 |    32.00
0.99 |   400.00

___

Finally as we already know the values at specific quantiles, we use the cmap to create a legend for our **quantVal** values and the scatter to plot them. It can be seen in the plot that there are lots of low values clustered across all the area (these are the values between 0.00 and 1.00). There is cluster in high values as well but they are considerably fewer. This is important to know when we choose the bmin parameter in the declus.exe.

- bmin=0 means that we are minimizing the declustered mean
- bmin=1 means that we are maximizing the declustered mean

- If low values are clustered, then we will look for maximizing the declustered mean.
- If high values are clustered, then we will look for minimizing the declustered mean.

Anyway, it is a little more complicated than just mazimizing or minimizing a value. Better results were obtained by just picking the coarse grid of sampling, for further  details I encourage you to read some bibliography of Professor Michael Pyrcz **Declustering and Debiasing** . In this post, we will show the case minimizing the declustered mean.


```python
#'b', 'g', 'r', 'c', 'm', 'y', 'k', 'w'
bounds = [0.01,1,1.84,3,5,10,32,400]
colors = ['b', 'g', 'r', 'c', 'm', 'y', 'k']
cmap = matplotlib.colors.ListedColormap(colors)
norm = matplotlib.colors.BoundaryNorm(bounds, len(colors))

#Some plotting parameters
figfmt = {
    'figure.figsize' : (15, 8)
    #'grid.linewidth':'0.4'
}
plt.rcParams.update(figfmt)

fig, ax = plt.subplots()
df.plot.scatter(ax=ax, x='calcEasting',y='calcNorthing', c='quantVal', label='Gold ppb',
                cmap=cmap, norm=norm, s=0.01)
plt.ylim(5750000, 6050000)
#plt.xlim(200000,400000)
plt.xlabel('Easting (m)')
plt.ylabel('Northing (m)')
plt.title('Gold ppb in Victoria-AUSTRALIA')
plt.legend(loc='lower left')
plt.grid()
plt.show()
```


![Gold ppb in Victoria](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/20200502_02.PNG){: .center-block :}


### Declustering Process itself using GSLIB wrapper
___
  
  
Now we are ready to run the wrapper. As it is clear, we need coordinates and a value columns. In our dataset this columns are 'calcEasting', 'calcNorthing' and 'quantVal' respectively. **cmin** and **cmax** are the size of the cells (square cells herein) and **cnum** means that the programm will run that cnum number of cell sizes between **cmin** and **cmax**. As we said before, our **bmin** value is set to 1.


```python
weights = declus(df2,'calcEasting','calcNorthing','quantVal',cmin=100,cmax=25000,cnum=80,bmin=0)
```

Now let's add the column 'Weights' to our DataFrame

```python
df_wts = pd.DataFrame(weights)
df_wts.columns = ["Weights"]
samples_wts = pd.concat([df,df_wts],axis = 1)
```


### Interpreting our results

This is when we can analyze the results of the declus.exe, in the graph below we can observe the variations of the declustered mean in different cell sizes. We previously set the declus.exe to pick the weights that get the highest declustered mean, but was that right? Further calculations could be carried out and as it can be imagined, more samples must be collected  in zone with higher content and so shouldn't we have set the **bmin** value to Zero?

I let you to try the calculations, if you have some questions just comment or e-mail me.


```python
header=['cellsize','mean']
df_ = pd.read_csv('declus.sum', sep='\s+',skiprows=4, names=header)

figfmt = {
    'figure.figsize' : (15, 5)
}
plt.rcParams.update(figfmt)

plt.scatter(df_['cellsize'],df_['mean'],s=10)
plt.title('Cell Size vs Declustered Mean')
plt.xlabel('Cell Size (m)')
plt.ylabel('Declustered Mean')
plt.grid()
plt.xlim(0,25000)
plt.show()
```


![Cell Size vs Declustered mean](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/20200502_03.PNG){: .center-block :}


The graph below 'Gold Values vs Declustered Weights' show us how the Weights we assigned. As we know, the Weight of a Sample is inversely proportional to the number of Samples that lies on a given cell size, it means the the more samples lies on a single cell, the lesser the weight assigned to each samples is and viceverse.

In our case, low values were assigned with higher weight, it is evident since coarser sampling (so less samples in a given cell size) must be taken in zones with low values in exploration

```python
#samples_wts[['quantVal','Weights']].head(100)
x = samples_wts['quantVal']
y = samples_wts['Weights']
plt.scatter(x,y, s=0.05, color='blue')
plt.xlim(0,400)
plt.ylim(0,5)
plt.xlabel('Gold Values (ppb)')
plt.ylabel('Declustered Weights')
plt.title('Gold Values vs Declustered Weights')
plt.grid()
```


![Gold Values vs Declustered weights](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/20200502_04.PNG){: .center-block :}


It is time to compare our results with the original Data, by using weighted histograms.

```python
samples_wts['ln_quantVal'] = np.log(samples_wts['quantVal'])

figfmt = {
    'figure.figsize' : (12, 5),
    'grid.color' : 'grey',
    'grid.linestyle' : '-'
}

plt.rcParams.update(figfmt)
#samples_wts['Weights'].isna()
fig, ax = plt.subplots()
plt.hist(samples_wts['ln_quantVal'], density=True, cumulative=False, label='Declustered',
         facecolor='blue',
        edgecolor='black', bins=np.linspace(-10,10,100), weights=samples_wts['Weights'], alpha=0.4)
#plt.xlabel('Gold values'); plt.ylabel('Cumulative frequency'); plt.title('Gold Cumulative Distribution')
plt.hist(samples_wts['ln_quantVal'], density=True, cumulative=False, histtype='stepfilled',
        edgecolor='red', bins=np.linspace(-10,10,100), alpha=0.4,label='Original')


plt.hist(samples_wts['ln_quantVal'], density=True, cumulative=True, label='Declustered Cumulative',
         facecolor='blue', histtype='step',
        edgecolor='blue', bins=np.linspace(-10,10,100), weights=samples_wts['Weights'], alpha=0.7)
#plt.xlabel('Gold values'); plt.ylabel('Cumulative frequency'); plt.title('Gold Cumulative Distribution')
plt.hist(samples_wts['ln_quantVal'], density=True, cumulative=True, histtype='step',
        edgecolor='red', bins=np.linspace(-10,10,100), alpha=0.8,label='Original Cumulative')

plt.grid()
plt.title('Declustered Au Density vs Original Au Density')
plt.ylim(0,1)
plt.xlim(-5,8)
plt.xlabel('ln ppb Au values')
plt.ylabel('Density')
plt.legend(loc='upper left')
plt.show()
```


![Declustered Density vs Original Density](https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/20200502_05.PNG){: .center-block :}


In the declus.exe, the Weights calculated and assigned to each sample sum up a total equal to the number of samples in 'quantVal'. In order to calculate the mean and Standard Deviation, the weights must sum up 1 so we have to devide all the weights by the number of total samples first and check if this equals 1.

```python
samples_wt = samples_wts['Weights']/70685
print('The weights divided by the number of samples sum up:', round(samples_wt.sum(),5))
```

The weights divided by the number of samples sum up: **1.00002**


Below are the results for the mean and standard deviation of the original data and the declustered data.

```python
import math

samples_wts['wt'] = (samples_wts.quantVal * samples_wts.Weights)/70685
declus_mean = samples_wts['wt'].sum()

samples_wts['var'] = samples_wts.wt*((samples_wts.quantVal-54.45)**2)
declus_dev = math.sqrt(samples_wts['var'].sum())

orig_mean = samples_wts['quantVal'].mean()

samples_wts['orig_var'] = (samples_wts.quantVal-33.87)**2
orig_dev = math.sqrt(samples_wts['orig_var'].sum())

print('orig_mean: ', round(orig_mean,2))
print('orig_dev: ', round(orig_dev))
print('declus_mean: ', round(declus_mean,2))
print('declus_dev: ', round(declus_dev))
```
  
orig_mean:  33.87
orig_dev:  249881
declus_mean:  20.29
declus_dev:  105171



### Conclusions
___

Although hand contouring and a knowledge of the regional geology would reveal the central trend of high and low values of a Random Variable, the details of the true distribution of a specific **RV** (in this example the content of Gold (ppb)),  would be inaccesible in practice.

Assuming that we have defined the parameters for our true distribution, the next step is to compare and our results **orig_mean, orig_dev, declus_mean, declus_dev** with the true mean and standard deviation of the hypothetical True Distribution.

It may be possible that the best Declustered mean will not be the Highest Declustered mean or the Lowest Declustered mean in a given case, and not knowing that will inevitably will lead us to get wrong results. Further analysis must be taken regarding the more regular grid of sampling in the exploration area.

Some Bibliography you can find in:
- Clayton Deutsch **Geostatistical Reservoir Modelling** Oxford University Press - 2002.
- Michael Pyrcz **Declustering and Debiasing** Professor at University of Texas.