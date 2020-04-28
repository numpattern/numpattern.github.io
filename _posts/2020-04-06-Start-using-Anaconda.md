---
layout: post
title: How to start using Jupyter
subtitle: Setting a New workable environment
bigimg: /img/bigdataspace.jpg
tags: [Anaconda, notebook, jupyter]
comments: true
share-img: "https://raw.githubusercontent.com/haroldvelasquez/haroldvelasquez.github.io/master/img/jupyter.jpg"
---

**Specifications:**
These are the specifications of my laptop, as you can see not much is needed.

--- | ---
Item | Details
--- | ---
Windows Edition:        | Windows 10 Pro
Processor:              | Intel(R) Core i5-6200U CPU @ 2.30 GHz 2.40 GHz
Installed memory (RAM): | 8.00 GB (7.87 GB usable)
System type:            | 64-bit Operating System, x64 based processor

**Assumptions:**
- We have succesfully installed Anaconda3 (I recommend  version 3 instead of 2)
- We wish to work with Jupyter notebooks
- We are agree to use Conda environments

Jupyter Notebook can be called by means of your  command line interface _Anaconda prompt_ on Windows. To do that type _Anaconda_ on your windows search field, once we are in the prompt, just type _jupyter notebook_ and press Enter, it will call jupyter notebook. We can also initialize the _Anaconda Navigator_ and launch _jupyter notebook_ option while in the _Home_ tab (I prefer to execute tasks since the prompt)

Jupyter Notebooks are a powerful choice to write and iterate on Python code for data analysis, you are able to write lines of code and run them one at a time. 

You can start by knowing your active environment by default which is named _base_ so try the following on your Anaconda prompt:

```python
#To know the version of your conda
conda --version
#To know the default python version installed in your conda enviroment
python --version
#To get a list of all environments installed, you could try also just _conda info_ for detailed destription of the current active environment.
conda info --envs
```

In my case I have conda 4.8.3 and python 3.8.2, the name of our default enviroment is as said before _(base)_. Now I wil tell you how to set up some specific characteristics in such a way that we can have more control of our environment. For instance, adding the well known _conda-forge_ channel or setting up some characteristic on your _.condarc_ file (this _.condarc_ firstly is a unique text file existing on a root you can find by running _conda info_)

```python
#To add the conda-forge channel which is a important channel where there are several packages we will use in the future
conda config --add channels conda-forge

#This sets .condarc update conda to False. This prevents your conda to be auto-updating and in order to not getting some version mismatches while installing new packages.
conda config --set auto_update_conda False
```
Now we are ready to create our new environment by running the next line where _myenv_ is the name you chose for your new environment and where we are telling conda to install a specific python version (you can chose what python version you want your conda to have:

```python
#Create a new environment in the file directory you currently are. You can change the directory using cd before you run conda create
conda create -n myenv python=3.8
```

One you created your environment, you can switch between your default _base_ environment and your new ones. For that, use the next code:
```python
#To activate a specific environment
conda activate myenv
```

So, when you install a new environment, this attains a default list of packages that were installed. To know what packages were installed just do the following:
```python
#This is a simpler way to know it. All the packages you have installed
conda list -n myenv
```

So far, we know how to create environments, how to switch between them and how to know what are the packages installed in every one of them. But what if we need to install a new package that is outside of the defaults?. Well!, there are many different ways to install packages depending on what is the source of them, but I will mention by the moment the _conda install mypackage_.

```python
#In this case we are installing ipykernel package using conda, we need this package in order to make the next section works, so run it
conda activate myenv
conda install ipykernel
```

Once we are here, having 2 one more environments, does not mean that we are able to use them in _Jupyter_ , so to solve that, we need to install and set this new environments into _Jupyter_ doing the folowwing:

```python
#Activate the specific environment you want to use in Jupyter. You will be asked to install ipykernel if did not do before.
#In the second line of the code: myenv is just the name of a folder that would be created on your machine and python(myenv) is the label that you will see in Jupyter Notebooks"
conda activate myenv
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

Finally, to start your _notebook_ run the following while in your _base_ environment (do not try to do it, being in other new environment; do not close the Anaconda prompt) , and wait until Jupyter loads in your _Internet Explorer_ using a default localhost.

```python
#This is a simpler way to know it. All the packages you have installed
conda activate base  #Activate your default environment
jupyter notebook
```

If you want to kill the kernel (close the Jupyter) type CTRL+C in your keyboard while you are in in your Anaconda prompt.

There are many others functionalities that we will learn on the way but meanwhile, we are ready to get to the next stage.


### Notification
{: .box-note}
**Note:** This is a notification box.

### Warning
{: .box-warning}
**Warning:** This is a warning box.

{: .box-error}
**Error:** This is an error box.


Best Regards,

**_Harold G. Velasquez_**  
_Geoscientist_