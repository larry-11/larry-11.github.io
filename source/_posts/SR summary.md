---
title: SR summary
date: 2020-03-06 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: inrto of SR
tags:
- SR
categories:
- DL summary
---



# SR

## categories

- supervised SR（有监督学习的图像超分辨率）
- unsupervised SR（无监督学习的图像超分辨率）
- domain-specific SR （特定应用领域的图像超分辨率）

## dataset

![](image-20200306194236412.png)

## summary

![](image-20200306194207180.png)

## innovation

1. Framework ：

   - lightweight：考虑实际应用
   - Local and Global Information：大的感受野可以提供更多的纹理信息
   - Low- and High-level Information：低层为颜色边缘信息，高层为目标信息
   - Context-specific Attention：结合特定内容的注意力机制
   - Upsampling Layers：multi-scale

2. Learning Strategies

   - loss：pixel，VGG，GAN 最有效的损失函数还不明确
   - Normalization：代替BN

3. Evaluation Metric

   - SRGAN提出传统的PSNR/SSIM图像质量评价方法存在缺陷，存在细节模糊但数值较高情况

4. unsupervised SR

   - 常见方法为 Bicubic 方法获得LR图像，用LR-HR作为SR网络的训练数据，这样SR问题会变成预先定义图像退化过程的逆过程，在自然低分辨率图像上应用这类SR方法效果较差
   - 目前多方法多围绕cycle-GAN

5. Real World Scenaries

   - Dealing with Various Degradation：解决多种图像退化问题，针对不同方式获得的LR图像
   - Domain-specific Applications：视频监控、人脸识别、目标跟踪等应用场景
   - Multi-scale Super-resolution：任意缩放因子
   - Zoom：基于真实环境中的图像退化模型
     Zoom to Learn, Learn to Zoom
     Camera Lens Super-Resolution

6. multi-task SR

   - Recovering Realistic Texture in Image Super-resolution by Deep Spatial Feature Transform

   