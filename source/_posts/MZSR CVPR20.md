---
title: MZSR
date: 2020-03-09 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: MZSR:Meta-Transfer Learning for Zero-Shot Super-Resolution（CVPR 2020）
tags:
- SR
- Meta Learning
categories:
- DL paper
---

# motivation

- 现有 SISR 模型不能挖掘测试图片内部的信息
- SISR 模型仅适用于固定领域：例如使用“bicubic”进行下采样的图片
- ZSSR 需要上千次迭代

# contribution

- 通过 meta-transfer learning 为 ZSSR 提供初始参数
- 利用了 internal 与 external 信息
- *“fast, flexible, lightweight and unsupervised at meta-test time”*  可应用于实际应用

# method

![](image-20200309171528334.png)

## Large-scale Training

- 使用 DIV2K 数据集，进行“预训练”，使用“bicubic”下采样

- loss：![](image-20200309172242758.png)

- why：

  （1）SR任务具有相似的属相，因此可以学习到图片的能够代表HR的有效表示

  （2）MAML训练不稳定，使用 pre-trained 来简化训练

## Meta-Transfer Learning

- 借鉴 MAML 思想，输出初始权重

- *In this step, we seek to find a sensitive and transferable initial point of the parameter space where a few gradient updates lead to large performance improvements*

- *external dataset for meta-training, internal learning for meta-test*

  why： *we intend our meta-learner to more focus on the kernel-agnostic property with the help of a large-scale external dataset*

- 用大量不同模糊核k合成训练数据集：![](image-20200309181946514.png)

- 模糊核分布：![](image-20200309182629382.png)

- meta-data 分为 D(tr) 与 D(te)：task-level training, task-level test

- task-level training: ![](image-20200309182852335.png)

- meta-objective: ![](image-20200309182921770.png)

- meta-training: ![](image-20200309182945472.png)

## Meta-Test

- 通过 *“kernel estimation algorithms”* 从输入图像中得到 blur kernel
- meta-transfer learning 对相应的kernel 生成初始权重

## Algorithm

![](image-20200309184409434.png)

![](image-20200309184429461.png)

# experiment

![](image-20200309185128226.png)

![](image-20200309184704620.png)

实验效果能够看出其快速适应的特点

# advantage

- 综合内部外部信息
- 快速适应

# disadvantage

-  *“kernel estimation algorithms”* 通过输入的 LR 图片预测 blur kernel，影响模型整体性能

