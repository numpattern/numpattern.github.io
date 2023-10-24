---
layout: post
title: Create a Compute for Azure ML with CUDA enabled
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [Azure, CUDA]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---

#### **What is a compute in Azure ML**
Compute instances in Azure ML Studio are single-owned cloud-based workstations used as a pre configured, optimized and fully customizable environment for ML applications. They also allow us to share files between instances and built manageable applications.

#### **How to create a compute in Azure ML**
Setting a compute instance is a first step prior to development. Typically for ML applications, we must ensure our compute has GPU in order to enable CUDA. The steps to create a compute instance are:
1. Create Azure ML Compute : name : Choose an option with GPU
2. Select from all options: Standard_NC8as_T4_v3 

The are several compute options in Azure ML Studio, the selected one has a reasonable price at 0.75/hr.
#### **What is CUDA and why do we need it?**
CUDA is a parallel computing platform and programming model created by NVIDIA used to speed up applications by harnessing the power of GPUs

#### **Do I have CUDA?**
CUDA is a standard feature in all NVIDIA GeForce, Quadro, and Tesla GPUs as well as NVIDIA GRID solutions, a comprehensive list of products with CUDA are found [here](https://developer.nvidia.com/cuda-gpus). The command `torch.cuda.is_available()` is a simple way to check if we have access to GPUs in our environment, it returns True if the system has the NVIDIA driver correctly installed.

#### **Setting up the environment**  
To create an environment within our compute, we run some steps in the Terminal. A common encountered issue while setting an environment with CUDA is the incompatibility between torch and torchvision packages; the pair `torch=2.0.0+cu118` and `torchvision=0.15.1` is our choice. After the installation of the previous packages, CUDA should be enabled in our environment.  


{% highlight python linenos %}
conda create -n myenv python==3.8.10
pip install cython
pip install torch==2.0.0+cu118 torchvision==0.15.1+cu118  --extra-index-url https://download.pytorch.org/whl/cu118

conda activate myenv
import torch
torch.cuda.is_available() 

pip install opencv-python
pip install azureml
pip install azureml.core
pip install azureml-dataset-runtime --upgrade

conda install pip
conda install ipykernel
python -m ipykernel install --user --name my_kernel --display-name "my_kernel_display"
conda list --explicit > spec.txt 
conda create --name myenv --file spec-file.txt 
{% endhighlight %}

This post showed how to create a compute in azure ML Studio, and do a basic set up for ML applications enabling CUDA in our environment.  

#### References
1. [Installing previous versions of pytorch](https://pytorch.org/get-started/previous-versions/)  
2. [CUDA-enabled GPUs](https://developer.nvidia.com/cuda-gpus)