---
title: Meta Learning （Hongyi Li）
date: 2020-02-04 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: 李宏毅 Meta Learning 课程笔记
tags:
- Meta Learning
categories:
- DL summary
---



# Meta Learning

## Introduction

### Meta Learning = Learn 2 Learn 

### Life-Long Learning & Meta Learning

Life-Long Learning: 不断用同一个模型进行学习

Meta Learning: 不同任务拥有不同的模型

### ML

![](image-20200204142328027.png)

### Meta Learning

![](image-20200204142350120.png)

### Machine Learning & Meta Learning

![](image-20200204142525193.png)

## Training

### Loss函数，多任务loss值之和

![](image-20200204143128683.png)

### 数据集划分

ML为训练集&测试集，Meta Learning为训练任务&测试任务

![](image-20200204143322759.png)

### Meta Learning常与few-shot learning联系，仅用少量数据集进行训练

### Support & Query set

![](image-20200204144029253.png)

### 整体流程

![](image-20200204144126330.png)

## Omniglot

### Few-Shot Classification

![](image-20200204144643335.png)

## 常见方法

### MAML

#### 固定模型架构，仅针对初始化参数

![](image-20200204145154001.png)

#### MAML & Model Pre-Training

![](image-20200204145458988.png)
![](image-20200204145530859.png)
![](image-20200204145558559.png)

#### MAML训练仅update一次，测试中为取得更好效果可update多次

![](image-20200204145858562.png)

#### Example：拟合三角函数

![](image-20200204150034660.png)
![](image-20200204150319806.png) 

#### 数学推理

![](image-20200204151409192.png)

#### first-order approximation 近似简化计算

![](image-20200204151527685.png)
![](image-20200204151613428.png)

#### 实际应用：

MAML参数进行两次update：

 （1）得到训练后参数theta

  （2）得到theta对loss的偏微分，用于更新初始参数

![](image-20200204152145493.png)

### Reptile

#### 训练

![](image-20200204162459449.png)

#### 区别：Pre-train & MAML & Reptile   

![](image-20200204162516883.png)

## Gradient Descent as LSTM

### V1

#### GD视为LSTM简化版：

input & forget gate的参数并非学习而来，而是人为设定

![](image-20200204191826717.png)

#### LSTM for GD

动态调整学习率并对先前参数进行处理

![](image-20200204192217178.png)

#### training

![](image-20200204192420750.png)

#### 简化运算

![](image-20200204192639254.png)

#### 实际应用

LSTM适用于所有参数的更新，仅有一个LSTM cell

training&testing 应用不同model：CNN&RNN (MAML无法做到)

![](image-20200204192844628.png)

### v2

下层LSTM类似于倒数（加速度）
![](image-20200204193331312.png)

## Metric-based Approach

### 概述

![](image-20200204210726424.png)



### Face Verification & Face Identification

Face Identification:判断是一组人中哪一个

Face Verification:判断是否是同一个人

![](image-20200204210930504.png)

### Face Verification训练 

类似word2vec

![](image-20200204211516701.png)

![](image-20200204211728786.png)

### 实例

![](image-20200204214507405.png)

#### Prototypical Network

效果大于Matching

![](image-20200204214643877.png)

n-shots情景，对embedding取平均值：
![](image-20200204214816320.png)

#### Matching Network:

![](image-20200204214856730.png)

#### Relation Network

不同于Prototypical Network & Matching Network，Relation Network采用两个module，第一个用于提取embedding，第二个用于计算class与test set的embedding之间的相似度。

（彩色长方体为提取出的各类别embedding，黄色为输入embedding）

之前方法的相似度使用特定的计算方式（eg.余弦）

![](image-20200204221345831.png)

### Imaginary Data

GAN网络部分与learning+prediction网络共同权重更新进行训练

![](image-20200204222016936.png)

## Test as RNN

#### 一般思路 general

但实验中模型无法成功训练

![](image-20200204222426498.png)

### MANN & SNAIL

SNAIL类似wavenet网络构造，加强输入之间的联系

![](image-20200204222710008.png)