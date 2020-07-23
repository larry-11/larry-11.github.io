---
title: SSGAN CVPR2019
date: 2020-04-22 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: Self-Supervised GANs via Auxiliary Rotation Loss
tags:
- self-supervised
- GAN
categories:
- DL paper
---

# motivation

- Conditional GAN需要大量标签
- GAN训练不稳定：*GANs are typically trained using alternating stochastic gradient descent which is often unstable and lacks theoretical guarantees*
- *instability, divergence, cyclic behavior, or mode collapse*
- GAN训练过程中D模块的遗忘现象
  - In non-stationary online environments, neural networks forget previous tasks
  - training may become unstable or cyclic
  - This issue is usually addressed either by reusing old samples or by applying continual learning techniques
  -  These issues become more prominent in the context of complex data sets - > conditioning（labeled data）
  - CGAN中标记数据可以帮助D模块训练更稳定的表达，并且CGAN对于单一类别训练易于整体数据集训练

# contribution

-  *take a step towards bridging the gap between conditional and unconditional GANs*，*Under the same conditions, the self-supervised GAN attains a similar performance to state-of-the-art conditional counterparts*
- unsupervised generative model that combines adversarial training with self supervised learning
-  add an auxiliary, self-supervised loss to the discriminator

# Key Issue: Discriminator Forgetting

- original value function for GAN

  ![](image-20200422124554575.png)

  训练过程中PG(x)参数更新，因此导致D模块在线学习不稳定

- 训练过程中D模块往往会根据G模块学习到的局部特征（纹理、边缘、结构等）来进行惩罚，因此难以学习到整体的有效表达（局部陷阱）

  *discriminator is not incentivised to maintain a useful data representation as long as the current representation is useful to discriminate between the classes*

- 训练后期PG = Pdata，D模块输出为常数0.5，因此D模块将无法继续学习到有效信息。

  同时如果训练过程中加入正则化，D模块可能会忽略有效信息而专注于次要信息，因此无法正确辨别

- 常规Uncond-GAN的遗忘现象（500k后遗忘有效表达）
  ![](image-20200422125628942.png)
- mnist测试，尽管任务相似，训练类别切换时仍存在遗忘现象
  ![](image-20200422125811573.png)

# method

 ![](image-20200422130346468.png)

##  pretext task

- 引入旋转角度预测，学习到更有效地表达

- rotation-based loss

  ![](image-20200422160149394.png)

## Collaborative Adversarial Training

- *generator and discriminator collaborate on the task of representation learning, and compete on the generative task*
- D模块通过预测真实数据旋转角度进行训练
- 促进G模块生成具有可检测旋转的真实图片特征的表达
- α>0不能保证收敛到PG = Pdata，因此训练过程中逐渐将α衰减至0
- *the generator is encouraged to generate images that are rotation-detectable because they share features with real images that are used for rotation classification*

# experiment

![](image-20200422161522929.png)

![](image-20200422161543467.png)

![](image-20200422161850627.png)

![](image-20200422161909392.png)