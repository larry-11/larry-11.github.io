---
title: MoCo v1
date: 2020-04-21 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: Momentum Contrast for Unsupervised Visual Representation Learning
tags:
- self-supervised
categories:
- DL paper
---

# motivation

- NLP中常用的SSL往往是由于NLP中常使用离散变量（单词），而CV中多数为高维连续变量
- contrastive learning作为字典查询角度需要构建较大的字典

# contribution

- 提出MoCo构建用于contrastive learning的large and consistent dictionaries
-  purpose：*pre-train representations (i.e., features) that can be transferred to downstream tasks by fine-tuning*
- 适用于*real-world, billionimage scale, and relatively uncurated scenario*

# method

![](image-20200421221349267.png)

## Contrastive Learning as Dictionary Look-up

- InfoNCE（contrastive loss function） t为温度变量

  ![](image-20200421221109336.png)

- a (K+1)-way softmax-based classifier that tries to classify q as k+

## Momentum Contrast

- 动态字典
- key encoder在训练过程中不断进化
- 假设：good features can be learned by a large dictionary that covers a rich set of negative samples, while the encoder for the dictionary keys is kept as consistent as possible despite its evolution

### Dictionary as a queue

- *At the core of our approach is maintaining the dictionary as a queue of data samples*
- 字典可以大于mini-batch size，可以作为超参数进行调节
- 每次更新mini batch个样本，FIFO

### Momentum update

- 使用队列可以扩充字典的大小，但是对key encoder进行反向传播变得更难

- 将query encoder 直接复制给key encoder ，这样快速地改变key encoder会破坏键值表示的一致性，不够稳定

- 队列保存特征图信息，而非原始图像信息

- momentum update

  ![](image-20200421231524714.png)

- 实验中较大的m（0.999）取得较好的结果，

  *slowly evolving key encoder is a core to making use of a queue*

### Relations to previous mechanisms

![](image-20200421231720678.png)

- end2end
  - 字典大小和mini-batch大小相同，受限于GPU显存
  - 对大的mini-batch进行优化也是挑战
- memory bank
  - 包含数据集所有数据的特征表示，从Memory Bank中采样数据不需要进行反向传播，所以能支持比较大的字典
  - 一个样本的特征表示只在它出现时才在Memory Bank更新，因此具有更少的一致性
  - 更新只是进行特征表示的更新，不涉及encoder

## Pretext Task

![](image-20200422001549632.png)

- 2 random view ：positive pair
- queries & keys独立编码

### Technical details

- ResNet
- output：128-D
- output：L2-norm
- random color jittering, random horizontal flip, and random grayscale conversion

### Shuffling BN

- BN会导致模型难以学习到有效的representation

- *The model appears to “cheat” the pretext task and easily finds a low-loss solution. This is possibly because the intra-batch communication among samples (caused by BN) leaks information*

  模型每个batch的内的样本之间communication 泄露了信息导致的

- shuffling BN：每个GPU分开做BN

  对于键值编码器fk，在当前mini-batch中打乱样本的顺序，再把它们送到GPU上分别进行BN，然后再恢复样本的顺序；

  对于查询编码器fq，不改变样本的顺序。

  这能够保证用于计算查询和其正键值的批统计值出自两个不同的子集

# experiment

## Linear Classification Protocol

### contrastive loss mechanisms

![](image-20200422003719906.png)

### momentum

![](image-20200422003837786.png)

### Comparison with previous results

![](image-20200422003935701.png)