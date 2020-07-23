---
title: MoCo v2
date: 2020-04-22 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: Improved Baselines with Momentum Contrastive Learning
tags:
- self-supervised
categories:
- DL paper
---

# motivation

- SimCLR提出projection head & data augmentation能够大大提高表现
- SimCLR训练过程使用过大batch size，需要TPU支持

# contribution

- 在MoCo v1中使用了projection head & data augmentation
- 通过v1中的queue机制解决SimCLR过大batch size问题（仅concat mini_batch的低维特征向量）

# method

![](image-20200422110621465.png)

- SimCLR为end2end机制，需要较大batch size来提供negative set
- MoCo通过queue保存negative key，每轮迭代仅encode mini_batch样本
- 依旧使用InfoNCE

# experiment

![](image-20200422111821704.png)

## MLP head

![](image-20200422111732879.png)

- 通过两层MLP（ReLU）代替v1中的fc
- *in contrast to the big leap on ImageNet, the detection gains are smaller*

## Augmentation

- 引入blur augmentation（stronger color distortion在实验中没有较明显的效果提升）
- 单纯引入aug+在检测任务中效果优于MLP，但在线性分类中效果较差
- *linear classification accuracy is not monotonically related to transfer performance in detection*

## Comparison with SimCLR

![](image-20200422112621430.png)

## Computational cost

![](image-20200422112954818.png)

- SimCLR需要更新q、k enconder，而MoCo仅更新q encoder，k encoder通过Momentum update进行更新