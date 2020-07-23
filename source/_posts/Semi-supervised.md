---
title: Semi-supervised 李宏毅
date: 2020-03-09 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: 半监督学习李宏毅课程笔记
tags:
- semi-supervised
categories:
- DL summary
---



# Semi-supervised

## Intro

- supervised learning：

  ![](image-20200309144949140.png)

- Semi-supervised learning：

  ![](image-20200309145015366.png)

- Why？ 利用好未标注信息

  ![](image-20200309145102909.png)

- Why helps？ 假设影响效果

  ![](image-20200309145258396.png)

## Generative Model

类似EM，逐步逼近

![](image-20200309150057821.png)

![](image-20200309150307629.png)

## Low-density Separation

### self training

![](image-20200309150526844.png)

![](image-20200309150912805.png)

hard label有绝对的类别，soft label为各类别概率

### Entropy-based

![](image-20200309151321244.png)

### Semi-supervised SVM

![](image-20200309151833922.png)

## Smoothness Assumption

### Definition

![](image-20200309152101626.png)

### Application

![](image-20200309152253696.png)

![](image-20200309152411421.png)

### Approach

#### Cluster

![](image-20200309152450444.png)

![](image-20200309152649530.png)

#### Graph-based

![](image-20200309152829189.png)

![](image-20200309153144204.png)

 ![](image-20200309153417329.png)

![](image-20200309153554298.png)

![image-20200309153743118](image-20200309153743118.png)

## Better Representation

找到本质信息

![](image-20200309154008397.png)



