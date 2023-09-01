I need permission to create a resource group under my subscription, then I cannot create a storage account. I cannot create a DataStore as I need subscription Key

Start using Automatic Logging:

Create 

**Assets**
Data > Create > MyDataAsset > type: File > From local files > Next > Data Storage Type: Azure Blob Storage > Select a datastore (ml-datasets) > Upload > Drag and Drop > Next.
	I need an Account key to create a new data store

**Manage**
Data Labeling > **Add project** > Image > Polygon (instance Segmentation) > Next (Dont add vendor from Marketplace ) > Select / Create data asset: MyDataAsset > Next > Enable Incremental refresh at regular intervals > Add labels > Add labels instruction (will appear as guidance textbox) > OK

Select your created project > Label Data > 

Labeling Short Keys:
	L: Lock drawn regions so you can draw on top. L again to unlock
	Esc: Cancel drawing
	F: toggle regions background color for better visibility
	CTRL+left click: add node to existing polygon (click over line and drag to desired position)

Select your created project > Export> Export Format: COCO > Coordinate Type: Normalized > Export FileName: ProjectName_hvs > Submit > Download File 


**Compute Instance and setting environment**  (Current Compute: areharogpu)
Ensure the machine you create has GPU, Create Azure ML Compite > GPU > Select from all options > Standard_NC8as_T4_v3 0.75/hr
```
conda create -n myenv python==3.8.10
pip install cython
pip install torch==2.0.1+cu118 torchvision -f https://download.pytorch.org/whl/torch_stable.html

#reference: https://pytorch.org/get-started/previous-versions/  Use this instead
#to avoid torch vs torchvision compatibility issues
pip install torch==2.0.0+cu118 torchvision==0.15.1+cu118 --extra-index-url https://download.pytorch.org/whl/cu118


(python) import torch
(python) torch.cuda.is_available()  , should output True

conda install -c anaconda git
git clone https://github.com/facebookresearch/detectron2.git
cd detectron2   , cd to the downloaded repository
pip install -e .

pip install opencv-python
pip install azureml
pip install azureml.core
pip install azureml-dataset-runtime --upgrade

conda install pip
conda install ipykernel
python -m ipykernel install --user --name ame --display-name "ame_kernel" 

conda list --explicit > spec.txt  , export specifications
conda create  --name myenv --file spec-file.txt 
```
Reference for detectron2: https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5#scrollTo=FsePPpwZSmqt

**Notebook**
Data > Data Assets > Select your data_asset > Consume > Python snippet to connect data

4. There are some requirements from Antoine Cate notebook, not sure in what moment they are meant to be used. On Azure terminal
```
pip install azureml-core
pip install torch==1.6.0 torchvision==0.7.0
pip uninstall Pillow -y
pip install Pillow==7.1.0
pip install detectron2 -f  https://dl.fbaipublicfiles.com/detectron2/wheels/cu101/torch1.6/index.html
conda install shapely
conda install -c "conda-forge/label/gcc7" opencv
conda install scikit-image
conda install cudatoolkit=10.1
conda install pandas
pip install tensorflow
```