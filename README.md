# 📦 Segmentation Gym :muscle:
[![Last Commit](https://img.shields.io/github/last-commit/Doodleverse/segmentation_gym)](
https://github.com/Doodleverse/segmentation_gym/commits/main)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/Doodleverse/segmentation_gym/graphs/commit-activity)
[![Wiki](https://img.shields.io/badge/wiki-documentation-forestgreen)](https://github.com/Doodleverse/segmentation_gym/wiki)
![GitHub](https://img.shields.io/github/license/Doodleverse/segmentation_gym)
[![Wiki](https://img.shields.io/badge/discussion-active-forestgreen)](https://github.com/Doodleverse/segmentation_gym/discussions)

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![TensorFlow](https://img.shields.io/badge/TensorFlow-%23FF6F00.svg?style=for-the-badge&logo=TensorFlow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-%23D00000.svg?style=for-the-badge&logo=Keras&logoColor=white)

<!-- ![](https://user-images.githubusercontent.com/3596509/153691733-1fe98e37-5379-4122-8d02-adbcb0ab0db3.png) -->
![gym](https://user-images.githubusercontent.com/3596509/153696396-0b3148c5-77e4-48b2-b3ce-fd9038ba21ab.png)

## :scroll: Paper 
[![Earth ArXiv Preprint
DOI](https://img.shields.io/badge/%F0%9F%8C%8D%F0%9F%8C%8F%F0%9F%8C%8E%20EarthArXiv-doi.org%2F10.31223%2FX5HS81-%23FF7F2A)](https://doi.org/10.31223/X5HS81)

 Buscombe, D., & Goldstein, E. B. (2022). A reproducible and reusable pipeline for segmentation of geoscientific imagery. Earth and Space Science, 9, e2022EA002332. https://doi.org/10.1029/2022EA002332 


## New in May 2023
`make_datasets` (as well as `doodleverse_utils\make_mndwi_dataset` and `doodleverse_utils\make_ndwi_dataset`) now works in a new way. Before, all files were read in, shuffled, split into train and val sets, then non-augmented and augmented npz files were created for each set. This causes a potential data leak between train and validation subsets, and validation was carried out on augmented imagery. We introduced a clunky 'mode' config parameter to try to control the degree of use of augmentation.

From May 29, 2023, `make_datasets`  creates `train_data` and `val_data` subfolders, then copies splits of train and validation labels and images over (multiple bands of images if necessary). It makes non-augmented npzs for each, then makes augmented npzs for the training set only. This removes the potential data leak, and validation is carried out on non-augmented imagery, which is a better reflection of deployment. Like before, `make_datasets` does not make a test dataset. The test dataset is a domain/task specific problem: please make an independent test set for your problem.

## New in February 2023
We now offer a `segformer` model option in the config file. The [Segformer](https://huggingface.co/docs/transformers/en/model_doc/segformer) is part of the Huggingface transformers library, and we adapted the [keras example](https://keras.io/examples/vision/segformer/) to work within the Segmentation Gym framework. This is a transfer-learning-only option, using the mit-b0 set of weights that are fine-tuned on a new dataset.

## 🌟 Highlights

- Gym is for training, evaluating, and deploying deep learning models for image segmentation
- We take transferability seriously; Gym is designed to be a "one stop shop" for image segmentation on "N-D" imagery (i.e. any number of coincident bands in a multispectral image). It is tailored to Earth Observation and aerial remote sensing imagery.
- Gym encodes relatively powerful models like UNets, and provides lots of ways to manipulate data, model training, and model architectures that should yield good results with some informed experimentation
- Gym works seamlessly with [Doodler](https://github.com/Doodleverse/dash_doodler), a human-in-the loop labeling tool that will help you make training data for Gym. 
- It would also work on any imagery in jpg or png format that has corresponding 2d greyscale integer label images (jpg or png), however acquired.
- Gym implements models based on the U-Net. Despite being one of the "original" deep learning segmentation models (dating to [2016](https://arxiv.org/abs/1505.04597)), UNets have proven themselves enormously flexible for a wide range of image segmentation tasks and spatial regression tasks in the natural sciences. So, we expect these models, and, perhaps more importantly, the training and implementation of those models in an end-to-end pipeline, to work for a very wide variety of cases. Additional models may be added later.
- You can read more about the models [here](https://github.com/Doodleverse/segmentation_gym/wiki/Models-in-Zoo) but be warned! We at Doodleverse HQ have discovered - often the hard way - that success is more about the data than the model. Gym helps you wrangle and tame your data, and makes your data work hard for you (nothing fancy, we just use augmentation)
* As well as a family of UNets, we offer a Transformer model option, using the SegFormer model architecture from HuggingFace, and the mit-b0 set of weights that are fine-tuned on a new dataset
* This is a "tranfer-learning" option, and imagery can be any size

## ℹ️ Overview

Gym is a toolbox to segment imagery with a variety of a family of UNet models, which are supervised deep-learning models for image segmentation. Gym supports segmentation of image with any number of bands, and any number of classes (memory limited). We have built an end-to-end workflow that facilitates a fully reproducible label-to-model workflow when used in conjunction with companion program [Doodler](https://github.com/Doodleverse/dash_doodler), however pairs of images and corresponding labels however-acquired may be used with Gym.

* Preprocessing of imagery for deep learning model training and prediction, such as image padding and/or resizing
* Coupling of N-dimensional imagery, perhaps stored across multiple files, with corresponding integer label images
* Use of an existing (i.e. pre-trained) model to segment new imagery (by using provided code and model weights)
* Use of images and corresponding label images, or 'labels', to develop a 'model-ready' dataset. A model-ready dataset is a set of images and corresponding labels in a serial binary archive format (we use `.npz`) that contain all your data for model training and validation, and that can be unpacked directory as tensorflow tensors. We initially used tfrecord format files, but abandoned the approach because of the relative complexity, and because the npz format is more familiar to Earth scientists who code with python.
* Training a new model from scratch using this new dataset
* Evaluating the model against a validation subset
* Applying the model (or ensemble of models) on sample imagery, i.e. model deployment

We have tested on a variety of Earth and environmental imagery of coastal, river, and other natural environments. However, we expect the toolbox to be useful for all types of imagery when properly applied.

### ✍️ Authors

Package maintainers:
* [@dbuscombe-usgs](https://github.com/dbuscombe-usgs)
* [@ebgoldstein](https://github.com/ebgoldstein)

Contributions:
* [@2320sharon](https://github.com/2320sharon)
* `doodleverse_utils` functions in `model_metrics.py` use minimally modified code from [here](https://github.com/zhiminwang1/Remote-Sensing-Image-Segmentation/blob/master/seg_metrics.py)

## 🚀 Usage

This toolbox is designed for 1,3, or 4-band imagery, and supports both `binary` (one class of interest and a null class) and `multiclass` (several classes of interest).

We recommend a 6 part workflow:

1. Download & Install Gym
2. Decide on which data to use and move them into the appropriate part of the Gym [directory structure](https://github.com/Doodleverse/segmentation_gym/wiki/3_Directory-Structure-and-Tests). *(We recommend that you first use the included data as a test of Gym on your machine. After you have confirmed that this works, you can import your own data, or make new data using [Doodler](https://github.com/Doodleverse/dash_doodler))*
3. Write a `config` file for your data. You will need to make some decisions about the model and hyperparameters.
4. Run `make_dataset.py` to augment and package your images into npz files for training the model.  
5. Run `train_model.py` to train a segmentation model.
6. Run `seg_images_in_folder.py` to segment images with your newly trained model, or `ensemble_seg_images_in_folder.py` to point more than one trained model at the same imagery and ensemble the model outputs

* Here at Doodleverse HQ we advocate training models on the augmented data encoded in the datasets, so the original data is a hold-out or test set. This is ideal because although the validation dataset (drawn from augmented data) doesn't get used to adjust model weights, it does influence model training by triggering early stopping if validation loss is not improving. Testing on an untransformed set is also a further check/reassurance of model performance and evaluation metric

* Doodleverse HQ also advocates the use of `ensemble` models where possible, which requires training multiple models each with a config file, and model weights file


## ⬇️ Installation 

**(Feb. 2024 - if these instructions no longer work for you, please submit an Issue)**

We advise creating a new conda environment to run the program. We recommend [miniconda](https://docs.conda.io/en/latest/miniconda.html)

1. Create a conda environment called `gym`

[OPTIONAL] First you may want to do some conda and pip housekeeping (recommended):

```
conda update -n base conda
conda clean --all -y
python -m pip install --upgrade pip 
```

[OPTIONAL] Set mamba to the default installer  (recommended - it is faster and more stable):

```
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

### Windows:


1) you wish to use GPU for model training and the latest Tensorflow version, you now must use WSL2 and refer to the [official Tensorflow instructions](https://www.tensorflow.org/install/pip#windows-native_1). These instructions are therefore catered to WSL2 users.

Install miniconda:
```
sudo apt-get update
sudo apt-get install wget
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
bash Miniconda3-py39_4.12.0-Linux-x86_64.sh
bash
```

Create the conda environment:
```
conda create -n gym_gpu python=3.10 -y
conda activate gym_gpu
conda install -c conda-forge cudatoolkit=11.8.0 -y
conda install -c nvidia cuda-nvcc -y

python3 -m pip install nvidia-cudnn-cu11 tensorflow[and-cuda]
```

Verify the tensorflow GPU install:
```
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

For example, if you have 2 nvidia GPUs, you should see something like this:

```
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU'), PhysicalDevice(name='/physical_device:GPU:1', device_type='GPU')]
```


If you see your GPU listed, then success! Now install the rest of the dependencies:

```
conda install -c conda-forge scikit-image ipython tqdm pandas natsort matplotlib -y
python -m pip install doodleverse_utils chardet

python -m pip install transformers
```

Finally, test your transformers library installation:

```
python  -c "from transformers import TFSegformerForSemanticSegmentation"
```

If the above returns no error, congratulations! You are in business.


(and if you don't have `git` installed, `conda install git`)


### Linux/Ubuntu:

```
conda create -n gym python -y
conda activate gym
pip install tensorflow[and-cuda]
```

Verify install:
```
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

For example, if you have 2 nvidia GPUs, you should see something like this:

```
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU'), PhysicalDevice(name='/physical_device:GPU:1', device_type='GPU')]
```

### Troubleshooting


If you get conda errors you may need to configure the system paths: 
```
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```

From [here](https://www.tensorflow.org/install/pip), you may encounter the following error:

```
Can't find libdevice directory ${CUDA_DIR}/nvvm/libdevice.
...
Couldn't invoke ptxas --version
...
InternalError: libdevice not found at ./libdevice.10.bc [Op:__some_op]
```

To fix this error, you will need to run the following commands:

```
# Install NVCC
conda install -c nvidia cuda-nvcc=11.3.58 -y
# Configure the XLA cuda directory
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
printf 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/\nexport XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib/\n' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
# Copy libdevice file to the required path
mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice
cp $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/
```

In my case, I also had to link the path to the lib folder in miniconda to `LD_LIBRARY_PATH`:

```
ln -sf /usr/lib/x86_64-linux-gnu/libstdc++.so.6 ~/miniconda3/envs/gym/bin/../lib/libstdc++.so.6
```

If you get errors associated with loading the model weights you may need to:

```
pip install "h5py==2.10.0" --force-reinstall
```

and just ignore any errors.


2. Clone the repo:

```
git clone --depth 1 https://github.com/Doodleverse/segmentation_gym.git
```

(`--depth 1` means "give me only the present code, not the whole history of git commits" - this saves disk space, and time)



## How to use
Check out the [wiki](https://github.com/dbuscombe-usgs/segmentation_gym/wiki) for a guide of how to use Gym

1. Organize your files according to [this guide](https://github.com/Doodleverse/segmentation_gym/wiki/3_Directory-Structure-and-Tests)
2. Create a configuration file according to [this guide](https://github.com/Doodleverse/segmentation_gym/wiki/4_Creation-of-%60config%60-file)
3. Create a model-ready dataset from your pairs of images and labels. We hope you find [this guide](https://github.com/Doodleverse/segmentation_gym/wiki/Create-a-model-ready-dataset) helpful
4. Train and evaluate an image segmentation model according to [this guide](https://github.com/Doodleverse/segmentation_gym/wiki/Train-an-image-segmentation-model)
5. Deploying / evaluate model on unseen sample imagery tends to be task specific. We offer basic implementation examples [here](https://github.com/Doodleverse/segmentation_gym/blob/main/seg_images_in_folder.py) as well as in Segmentation Zoo [here](https://github.com/Doodleverse/segmentation_zoo/tree/main/scripts) and [here](https://github.com/Doodleverse/segmentation_zoo/tree/main/notebooks)

## Test Dataset

A test data set, including a set of images/labels, model config files, and a dataset and models created with Gym, are available [here](https://zenodo.org/record/7677961/files/my_segmentation_gym_datasets.zip?download=1) and [described on the zenodo page]([https://zenodo.org/record/7677961](https://zenodo.org/records/8170543))

You can train a model on the test set using the following commands:

```
wget https://zenodo.org/records/8170543/files/my_segmentation_gym_datasets_v5.zip
sudo apt-get install unzip
unzip my_segmentation_gym_datasets_v5.zip
```

then

```
git clone --depth 1 https://github.com/Doodleverse/segmentation_gym.git
cd segmentation_gym
python train_model.py
```
First folder to navigate to is this one

![image (2)](https://github.com/Doodleverse/segmentation_gym/assets/3596509/bd5a70af-ccc9-4f9f-9519-94056681ba8b)

then this one

![image (3)](https://github.com/Doodleverse/segmentation_gym/assets/3596509/b6693305-215d-464c-a703-5720c639f8d9)

then the 'segformer' config file

![image (4)](https://github.com/Doodleverse/segmentation_gym/assets/3596509/c6b3ba88-266d-427c-af96-92cc4f2ad876)

If you get an "OOM error", open the config file and reduce the `BATCH_SIZE` and try again.


## 💭 Feedback and Contributing

Please read our [code of conduct](https://github.com/Doodleverse/segmentation_gym/blob/main/CODE_OF_CONDUCT.md)

Please contribute to the [Discussions tab](https://github.com/Doodleverse/segmentation_gym/discussions) - we welcome your ideas and feedback.

We also invite all to open issues for bugs/feature requests using the [Issues tab](https://github.com/Doodleverse/segmentation_gym/issues)

