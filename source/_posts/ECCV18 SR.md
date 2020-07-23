---
title: Super resolution
date: 2020-03-06 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: ECCV 2018 SR 相关论文整理
tags:
- SR
categories:
- DL paper
---



# RCAN：Image Super-Resolution Using Very Deep Residual Channel Attention Networks

## motivation：

- 在图像超分辨领域，卷积神经网络的深度非常重要，但过深的网络却难以训练。
- 低分辨率的输入以及特征包含丰富的低频信息，但却在通道间被平等对待，因此阻碍了网络的表示能力。

## contribution：

- 提出了一个深度残差通道注意力网络（RCAN）。特别地，作者设计了一个残差中的残差（RIR）结构来构造深层网络，每个 RIR 结构由数个残差组（RG）以及长跳跃连接（LSC）组成，每个 RG 则包含一些残差块和短跳跃连接（SSC）。
- 提出了一种通道注意力机制（CA），通过考虑通道之间的相互依赖性来自适应地重新调整特征。

## method：

![](image-20200306165058127.png)

准备10个残差组(RG)，其中每组包含20个残差通道注意模块(RCAB)

### RIR（Residual In Residual）架构

RG（Residual Group）作为基本模块，LSC（Long Skip Connection）则用来进行粗略的残差学习，在每个 RG 内部则叠加数个简单的残差块和 SSC（Short Skip Connection）。

LSC、SSC 和残差块内部的短连接可以允许丰富的低频信息直接通过恒等映射向后传播，这可以保证信息的流动，加速网络的训练。

### CA（Channel Attention）机制

![](image-20200306165649288.png)

类似Autoencoder，先进行全局池化（GP），随后依次进行下采样，上采用，激活函数，得到通道描述。

### RCAB (Residual channel attention block) 基本模块

![](image-20200306165953838.png)

结合 CA 和残差

### loss

网络的损失函数是 L1 损失

## advantage:

- 在模型结构上进行改性，增加网络深度，通过LSC与SSC更好的提取了输入图片的低频信息
- 引入注意力机制

## disadvantage:

- 仍使用L1损失
- 模型过大，速度慢

# Fast, Accurate, and Lightweight Super-Resolution with Cascading Residual Network

## motivation：

- 目前的主流的方法都是奔着性能表现去的，并没有考虑实际应用的情况
- 目前最常用方法是减少参数的数量，实现这一目标的方法有很多，但最简单和有效的方法是使用递归网络。然而，这些模型有两个缺点：
  （1）在将输入图像输入CNN模型之前，先对其进行上采样（？？？难以理解）
  （2）增加网络的深度或宽度，以补偿由于使用递归网络而造成的损失。这些点使模型能够在重构时维持图像的细节，但增加了操作次数和时间

## contribution：

- 提出一种快速，精确并且轻量级的网络

## method：

![](image-20200306171812173.png)

- 全局和局部级联连接
- 中间特征是级联的，且被组合在1×1大小的卷积块中
- 使多级表示和快捷连接，让信息传递更高效

### advancement：

- 结合多层的特征，可以学习到多水平的表示。
- 级联可以让低层的特征很快的传播到高层。

### Efficient Cascading Residual Network

![](image-20200306172843168.png)

- 使用group conv，优点是使用者可以通过调整分组数来调整网络的速度和效果

  使用group conv而是不是 depthwise convolution，作者解释说group conv比 depthwise convolution可以更好的调整模型的有效性。

- 引入了递归神经网络的思想，这里是让级联模块的参数能够被共享,从而达到减少参数的目的

## experiment

![](image-20200306174638404.png)

## advantage：

- 减少para：   递归神经网络，共享权重
- 减少FLOPs：有限FLOPs提高表现->多层级联

## disadvantage：

- 损失函数仍为L1

# To learn image super-resolution, use a GAN to learn how to do image degradation first

## motivation：

- 传统的SR的方法获取LR的手段是人工获取，比如通过双线性下采样或者是先通过一个模糊核然后在进行双线性下采样。但是真实世界的图像往往具有复杂的退化模型，比如运动、去焦、压缩、传感器噪声等复杂的情况。所以目前一些主流的SR方法可能在人造的LR图像上重建效果很好，但是在真实世界图像上表现不一定很好。

## contribution：

- 提出一种2个阶段的处理，分别是High-to-Low GAN和Low-to-High GAN
- 提出 GAN-centered ，也就是以GAN损失作为主导，pixel损失作为辅导

## method：

![](image-20200306181449004.png)

#### High-to-Low

![](image-20200306182000373.png)

普通的ResNets堆叠而成的，其中ResNets使用的pre-activation而且不使用BatchNorm，结果如下：

其中输出区别为不同输入噪音向量

![](image-20200306182119329.png)

#### Low-to-High

![](image-20200306182226496.png)

#### D

![](image-20200306182407661.png)

- 共两组D模块，用于LR、HR判别
- 成对输入

#### loss

![](image-20200306182535236.png)

![](image-20200306182546854.png)

- GAN损失占主导
- pixel损失：MSE
- GAN损失：作者发现**WGAN-GP**和**SN-GAN**表现差不多。作者在本篇文章中选择了后者

## advantage：

- 更贴近实际应用，提出HR2LR
- 提出GAN-centered的损失函数计算
- 使用FID作为评价指标

## disadvantage：

- 模型简单
- 针对性设计，应用场景较为单一，未必适用于普遍场景