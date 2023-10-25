# GeneSegNet: a deep learning framework for cell segmentation by integrating gene expression and imaging. [Genome Biology](https://link.springer.com/article/10.1186/s13059-023-03054-0?utm_source=rct_congratemailt&utm_medium=email&utm_campaign=oa_20231019&utm_content=10.1186/s13059-023-03054-0#availability-of-data-and-materials)

## Overview
<div align=center><img src="https://github.com/BoomStarcuc/GeneSegNet/blob/master/data/GeneSegNet_framework.png" width="1000" height="360"/></div>

## Installation
1. Create conda environments, use:

``` 
conda create -n GeneSegNet python=3.8
conda activate GeneSegNet
```

2. Install Pytorch (1.12.1 Version), use:

``` 
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch
```

but the above command may not match your CUDA environment, please check the link: https://pytorch.org/get-started/previous-versions/#v1121 to find the proper command that satisfied your CUDA environment.

3. Clone the repository, use:

``` 
git clone https://github.com/BoomStarcuc/GeneSegNet.git
```

4. Install dependencies, use:

```
pip install -r requirement.txt
```

## Datasets and Model

1. Download the demo datasets at [GoogleDrive](https://drive.google.com/drive/folders/1rF6U5fSq8D-UpZW-iUy4DG16dyxAzvK7?usp=share_link) and unzip them to your project directory.
2. Download GeneSegNet pre-trained model at [GoogleDrive](https://drive.google.com/drive/folders/1hzavxQ_zkH6At0vkCzskyg7hlRnKDEC3?usp=sharing), and put it into your project directory.

If you want to run the algorithm on your data, please see  **Training** part.

## Datasets Structure
```
your dataset
 |-train
 |   |-Image sample1
 |   |   |-HeatMaps
 |   |   |   |-HeatMap
 |   |   |   |-HeatMap_all
 |   |   |-images            
 |   |   |-labels 
 |   |   |-spots
 |   |-Image sample2
 |   |-...
 |-val
 |   |-Image sample1
 |   |   |-HeatMaps
 |   |   |   |-HeatMap
 |   |   |   |-HeatMap_all
 |   |   |-images            
 |   |   |-labels 
 |   |   |-spots
 |   |-Image sample2
 |   |-...
 |-test
 |   |-Image sample1
 |   |   |-HeatMaps
 |   |   |   |-HeatMap
 |   |   |   |-HeatMap_all
 |   |   |-images            
 |   |   |-labels 
 |   |   |-spots
 |   |-Image sample2
 |   |-...
```
## Data preprocess
If you use the demo dataset we provided, you can skip this section. But if you want to train on your own dataset, you first need to run the preprocessing code to satisfy the dataset structure during training.

```
python Generate_Image_Label_locationMap.py
```
Note: ```base_dir``` and ```save_crop_dir``` need to be modified to your corresponding path. For the directory structure for data preprocessing, please see our raw simulation datasets at [GoogleDrive](https://drive.google.com/drive/folders/17Xj4XH9zs2zJradk2-ZzHNNBFdTOwWrM?usp=drive_link).

## Training from scratch
To run the algorithm on your data, use:

```
python -u GeneSeg_train.py --use_gpu --train_dir  training dataset path --val_dir validation dataset path --test_dir test dataset path --pretrained_model None --save_png --save_each --img_filter _image --mask_filter _label --all_channels --verbose --metrics --dir_above --save_model_dir save model path
```

Here:

- ```use_gpu``` will use GPU if torch with cuda installed.
- ```train_dir``` is a folder containing training data to train on.
- ```val_dir``` is a folder containing validation data to train on.
- ```test_dir``` is a folder containing test data to validate training results.
- ```img_filter```, ```mask_filter```, and ```heatmap_filter``` are end strings for images, cell instance mask, and heat map.
- ```pretrained_model``` is a model to use for running or starting training.
- ```chan``` is a parameter to change the number of channels as input (default 2 or 4).
- ```verbose``` shows information about running and settings and saves to log.
- ```save_each``` save the model under per n epoch for later comparison.
- ```save_png``` save masks as png and outlines as a text file for ImageJ.
- ```metrics``` compute the segmentation metrics.
- ```save_model_dir``` save training model to a directory

To see the full list of command-line options run:

```
python GeneSeg_train.py --help
```

## Test

After training, to run the test, use:

```
python GeneSeg_test.py --use_gpu --test_dir test dataset path --pretrained_model your trained model --save_png --img_filter _image --mask_filter _label --all_channels --metrics --dir_above --output_filename a folder name
```

## Run a pre-trained model
Before running the pre-trained model, please download the pre-trained model provided. 

```
python GeneSeg_test.py --use_gpu --test_dir test dataset path --pretrained_model pre-trained model --save_png --img_filter _image --mask_filter _label --all_channels --metrics --dir_above --output_filename a folder name
```

## Network Inference
To obtain final full-resolution segmentation results, use slidingwindows_gradient.py in ```Inference``` directory:

```
python slidingwindows_gradient.py
```
Note: ```root_dir```, ```save_dir```, and ```model_file``` need to be modified to your corresponding path.

## Citation
If you find our work useful for your research, please consider citing the following paper.
```
@article{wang2023genesegnet,
  title={GeneSegNet: a deep learning framework for cell segmentation by integrating gene expression and imaging},
  author={Wang, Yuxing and Wang, Wenguan and Liu, Dongfang and Hou, Wenpin and Zhou, Tianfei and Ji, Zhicheng},
  journal={Genome Biology},
  volume={24},
  number={1},
  pages={235},
  year={2023},
  publisher={Springer}
}
```
