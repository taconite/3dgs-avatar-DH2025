# 3DGS-Avatar: Animatable Avatars via Deformable 3D Gaussian Splatting
## [Paper](https://arxiv.org/abs/2312.09228) | [Project Page](https://neuralbodies.github.io/3DGS-Avatar/index.html)

<img src="assets/teaser.gif" width="800"/> 

This repository contains the implementation of the CVPR 2024 submission 
[3DGS-Avatar: Animatable Avatars via Deformable 3D Gaussian Splatting](https://arxiv.org/abs/2312.09228).

You can find detailed usage instructions for using pretrained models and training your own models below.

If you find our code useful, please cite:

```bibtex
@article{qian20233dgsavatar,
   title={3DGS-Avatar: Animatable Avatars via Deformable 3D Gaussian Splatting}, 
   author={Zhiyin Qian and Shaofei Wang and Marko Mihajlovic and Andreas Geiger and Siyu Tang},
   booktitle={CVPR},
   year={2024},
}
```

## Installation
### Euler-specific Setup
Run the following to load the required modules (`eth_proxy` is the network proxy that allows you to access the internet on a compute node - it is not necessary on the login node):
```
module purge
module load stack/2024-06
module load cuda/11.8.0 eth_proxy
export CC=gcc-11    # CUDA 11.8 requires GCC 11
export CXX=g++-11   # same as above
```


### Environment Setup
This repository has been tested on the following platform:
1) Python 3.7.13, PyTorch 1.12.1 with CUDA 11.6 and cuDNN 8.3.2, Ubuntu 22.04/CentOS 7.9.2009

To clone the repo, run either:
```
git clone --recursive https://github.com/mikeqzy/3dgs-avatar-release.git
```
or
```
git clone https://github.com/mikeqzy/3dgs-avatar-release.git
cd 3dgs-avatar-release
git submodule update --init --recursive
```

Next, you have to make sure that you have all dependencies in place.
The simplest way to do so, is to use [anaconda](https://www.anaconda.com/). 

You can create an anaconda environment called `3dgs-avatar` using
```
conda env create -f environment.yml
conda activate 3dgs-avatar
# install tinycudann
pip install git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch
```

### SMPL Setup
Please reach out to the TAs to get body model assets.  After you get the assets, extract them to the project directory. It should have the following structure: 
```
body_models
 └-- misc
 └-- smpl
    ├-- male
    |   └-- model.pkl
    ├-- female
    |   └-- model.pkl
    └-- neutral
        └-- model.pkl
```

## Dataset preparation
Please follow the steps in [DATASET.md](DATASET.md).

## Results on ZJU-MoCap
For easy comparison to our approach, we also store all our pretrained models and renderings on the ZJU-MoCap dataset [here](https://drive.google.com/drive/folders/1-miCqOPoOO1XATQECyHz1qgocrtTSD8L?usp=drive_link).

## Training
To train new networks from scratch, run
```shell
# ZJU-MoCap
python train.py dataset=zjumocap_377_mono
```
To train on a different subject, simply choose from the configs in `configs/dataset/`.

We use [wandb](https://wandb.ai) for online logging, which is free of charge but needs online registration.

## Evaluation
To evaluate the method for a specified subject, run
```shell
# ZJU-MoCap
python render.py mode=test dataset.test_mode=view dataset=zjumocap_377_mono
```

## Test on out-of-distribution poses
First, please download the preprocessed AIST++ and AMASS sequence for subjects in ZJU-MoCap [here](https://drive.google.com/drive/folders/17vGpq6XGa7YYQKU4O1pI4jCMbcEXJjOI?usp=drive_link) 
and extract under the corresponding subject folder `${ZJU_ROOT}/CoreView_${SUBJECT}`.

To animate the subject under out-of-distribution poses, run
```shell
python render.py mode=predict dataset.predict_seq=0 dataset=zjumocap_377_mono
```

We provide four preprocessed sequences for each subject of ZJU-MoCap, 
which can be specified by setting `dataset.predict_seq` to 0,1,2,3, 
where `dataset.predict_seq=3` corresponds to the canonical rendering.

Currently, the code only supports animating ZJU-MoCap models for out-of-distribution models.

## License
We employ [MIT License](LICENSE) for the 3DGS-Avatar code, which covers
```
configs
dataset
models
utils/dataset_utils.py
extract_smpl_parameters.py
render.py
train.py
```

The rest of the code are modified from [3DGS](https://github.com/graphdeco-inria/gaussian-splatting). 
Please consult their license and cite them.

## Acknowledgement
This project is built on source codes from [3DGS](https://github.com/graphdeco-inria/gaussian-splatting). 
We also use the data preprocessing script and part of the network implementations from [ARAH](https://github.com/taconite/arah-release).
We sincerely thank these authors for their awesome work.

