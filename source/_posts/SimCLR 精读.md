---
title: SimCLR
date: 2020-04-20 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: A Simple Framework for Contrastive Learning of Visual Representations
tags:
- self-supervised
categories:
- DL paper
---

# motivation

- generative & discriminative: 
  *pixel-level generation is computationally expensive and may not be necessary for representation learning*

# contribution

- SSL（自监督）相比SL（监督）更加依赖数据增强
- 特征层与最终loss之间引入非线性转化层（MLP）能够提高效果
- 归一化的embeddin与合适的温度参数（temperature parameter，softmax）利于表达
- 更大的batch-size、较长的训练时长、deeper&wider network都利于SSL

# method

## The Contrastive Learning Framework

![](image-20200421173804185.png)

如上图所示，文章主要结构为：

- A stochastic data augmentation module
  - 对于同一张图片进行不同组合的augmentation，得到不同multi-view作为positive pair
  - 文中使用：random cropping，random color distortions, and random Gaussian blur
- A neural network base encoder f(·)
  - 文中使用：ResNet50
- A small neural network projection head g(·)
  - 将encoder得到的representations映射到contrastive loss所在latent space
  - 文中使用两层的MLP
- A contrastive loss function
  - ![](image-20200421174416816.png)
  - sim为余弦相似度

整体算法流程如下：

![](image-20200421174505376.png)

## Training with Large Batch Size

- 作者在实验部分调整batch-size（256-8192）
- 常见优化器（SGD/Momentum）在大batch-size训练中不稳定，因此选用LARS optimizer
- 使用了128 TPU v3 cores。。。
- Global BN
  - 多卡BN各自算mean和std会泄露信息，从而用了 global BN

## Evaluation Protocol

- 数据集：ImageNet ILSVRC-2012 dataset
- 效果检测：*a linear classifier is trained on top of the frozen base network*

# Data Augmentation for Contrastive Representation Learning

## Composition of data augmentation operations is crucial for learning good representations

![](image-20200421175226271.png)

![](image-20200421175250735.png)

上图展示了常见的数据增强方法，作者在实验部分将其进行对比，发现组合的效果往往大于单独增强（对角线）

##  Contrastive learning needs stronger data augmentation than supervised learning

![](image-20200421175630380.png)

作者对比了color distortion的影响，可以看出较大的color distortion对于ssl有促进作用，却抑制sl效果，应该是color distortion加大了train、val之间的分布不均

# Architectures for Encoder and Head

## Unsupervised contrastive learning benefits (more) from bigger models

![](image-20200421175936946.png)

可以看出当模型变大参数增多，SSL效果的提升要大于SL

## A nonlinear projection head improves the representation quality of the layer before it

![](image-20200421181952116.png)

- non-linear效果往往比linear效果强3%

- *the hidden layer before the projection head is a better representation than the layer after*
  因此使用h来进行作为representation

- z = g(h)在训练过程中对于data transformation不敏感，因此可能会舍弃部分颜色或方向信息，不利于分类

- 下图中可以看出g(h)对于rotation、corrupted、Sobel变换不敏感

  ![](image-20200421182516923.png)

# Loss Functions and Batch Size

## Normalized cross entropy loss with adjustable temperature works better than alternatives

- NT-Xent：Normalized Temperature-scaled Cross Entropy取得最优效果

![](image-20200421184119087.png)

![](image-20200421184440737.png)

- l2中合适的温度参数可以帮助模型学习hard negative样本

![](image-20200421184240775.png)

## Contrastive learning benefits (more) from larger batch sizes and longer training

![](image-20200421184620460.png)

#  Comparison with State-of-the-art

## Linear evaluation

![](image-20200421184856747.png)

## Semi-supervised learning

 *sample 1% or 10% of the labeled ILSVRC-12 training datasets in a class-balanced way*

![](image-20200421185018340.png)

## Transfer learning

在混合数据集上SSL训练，并在特定数据集上fine-tune

![](image-20200421185128805.png)