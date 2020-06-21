# Augmented-Sliced-Wasserstein-Distances

This repository provides the code to reproduce the experimental results in the paper **[Augmented Sliced Wasserstein Distances](https://arxiv.org/abs/2006.08812)** by **[Xiongjie Chen](https://github.com/xiongjiechen), [Yongxin Yang](https://scholar.google.com/citations?user=F7PtrL8AAAAJ&hl=en&oi=ao)** and **[Yunpeng Li](https://scholar.google.com/citations?user=JzyKdRUAAAAJ&hl=en&oi=ao)**.
## Prerequisites

### Python packages
pytorch==1.4.0

torchvision==0.5.0

cudatoolkit==10.1.243

cupy==7.40

numpy==1.18.1

pot==0.7.0

imageio==2.8.0
### Datasets
Two datasets are used in this repository, namely the [CIFAR10](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.222.9220&rep=rep1&type=pdf) dataset and [CELEBA](http://openaccess.thecvf.com/content_iccv_2015/html/Liu_Deep_Learning_Face_ICCV_2015_paper.html) dataset.
The CIFAR10 dataset (64x64 pixels) will be automatically downloaded from https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz when running the experiment on CIFAR10 dataset. 
The dataset needs be be manually downloaded and can be found on the website http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html, we use the cropped CELEBA dataset with 64x64 pixels.

### Precalculated Statistics

To calculate the [Fréchet Inception Distance (FID score)](https://arxiv.org/abs/1706.08500), precalculated statistics for datasets

- [cropped CelebA](http://bioinf.jku.at/research/ttur/ttur_stats/fid_stats_celeba.npz) (64x64, calculated on all samples)
- [CIFAR 10](http://bioinf.jku.at/research/ttur/ttur_stats/fid_stats_cifar10_train.npz) (calculated on all training samples)

are provided at: http://bioinf.jku.at/research/ttur/.
## Project & Script Descriptions
Two experiments are included in this repository, where benchmarks are from the [GSWD paper](http://papers.nips.cc/paper/8319-generalized-sliced-wasserstein-distances) and the [DSWD paper](https://arxiv.org/pdf/2002.07367.pdf), respectively. The first one is on the task of sliced Wasserstein flow, and the second one is on generative modellings with GANs. For more details and setups, please refer to the original paper **[Augmented Sliced Wasserstein Distances](https://arxiv.org/abs/2006.08812)**.

The sliced Wasserstein flow example can be found in the [jupyter notebook](https://github.com/xiongjiechen/ASWD/blob/master/Sliced%20Waaserstein%20Flow.ipynb).

The following scripts belong to the generative modelling example:
- [main.py](https://github.com/xiongjiechen/ASWD/blob/master/main.py) : run this file to conduct experiments.
- [utils.py](https://github.com/xiongjiechen/ASWD/blob/master/utils.py) : contains implementations of different sliced-based Wasserstein distances.
- [TransformNet.py](https://github.com/xiongjiechen/ASWD/blob/master/TransformNet.py) : edit this file to modify architectures of neural networks used to map samples. 
- [experiments.py](https://github.com/xiongjiechen/ASWD/blob/master/experiments.py) : functions for generating and saving randomly generated images.
- [DCGANAE.py](https://github.com/xiongjiechen/ASWD/blob/master/DCGANAE.py) : neural network architectures and optimization objective for training GANs.
- [fid_score.py](https://github.com/xiongjiechen/ASWD/blob/master/fid_score.py) : functions for calculating statistics (mean & covariance matrix) of distributions of images and the FID score between two distributions of images.
- [inception.py](https://github.com/xiongjiechen/ASWD/blob/master/inception.py) : download the pretrained [InceptionV3](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Szegedy_Rethinking_the_Inception_CVPR_2016_paper.html) model and generate feature maps for FID evaluation.
## Experiment options for the generative modelling example
The generative modelling experiment evaluates the performances of GANs trained with different sliced-based Wasserstein metrics. To train and evaluate the model, run the following command:

```
python main.py  --model-type ASWD --dataset CIFAR --epochs 200 --num-projection 1000 --batch-size 512 --lr 0.0005
```

### Basic parameters
- ```--model-type``` type of sliced-based Wasserstein metric used in the experiment, available options: ASWD, DSWD, SWD, MSWD, GSWD. Must be specified.
- ```--dataset``` select from: CIFAR, CELEBA, default as CIFAR.
- ```--epochs``` training epochs, default as 200.
- ```--num-projection``` number of projections used in distance approximation, default as 1000.
- ```--batch-size``` batch size for one iteration, default as 512.
- ```--lr``` learning rate, default as 0.0005.

### Optional parameters

- ```--niter``` number of iteration, available for the ASWD, MSWD and DSWD, default as 5.
- ```--lam``` coefficient of regularization term, available for the ASWD and DSWD, default as 0.5.
- ```--r``` parameter in the circular defining function, available for GSWD, default as 1000.

## References 
### Code
The code of generative modelling example is based on the implementation of [DSWD](https://github.com/VinAIResearch/DSW)** by **[VinAI Research](https://github.com/VinAIResearch).

The pytorch code for calculating the FID score is from https://github.com/mseitzer/pytorch-fid.

### Papers
- [Distributional Sliced-Wasserstein and Applications to Generative Modeling](https://arxiv.org/pdf/2002.07367.pdf)
- [Generalized Sliced Wasserstein Distances](http://papers.nips.cc/paper/8319-generalized-sliced-wasserstein-distances)
- [Sliced Wasserstein Auto-Encoders](https://openreview.net/forum?id=H1xaJn05FQ)
- [Max-Sliced Wasserstein Distance and its Use for GANs](http://openaccess.thecvf.com/content_CVPR_2019/html/Deshpande_Max-Sliced_Wasserstein_Distance_and_Its_Use_for_GANs_CVPR_2019_paper.html)
