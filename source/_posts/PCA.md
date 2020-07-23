---
title: PCA
date: 2019-07-10 12:00:00
top: false
cover: false
password:
toc: true
mathjax: false
summary: PCA数学原理
tags:
- ML
categories:
- ML
---

# 向量内积

向量投影

# 基变换的矩阵表示

![](clip_image002.png)

R决定基变换后数据维度，若R<M，则为降维变换

# 协方差矩阵

## （1）方差（数据分离程度）

![](clip_image004.png)

预处理：各字段减去均值，使得新字段均值为零，简化方差

​    问题转换：寻找一个低维基，使得所有数据变换为这个基上的坐标表示后，方差值最大

## （2 ）协方差（数据相关性）

​    ![](clip_image006.png)

​    cov（基）== 0，正交基

优化目标：将一组N维向量降为K维（K大于0，小于N），其目标是选择K个单位（模为1）正交基，使得原始数据变换到这组基上后，各字段两两间协方差为0，而字段的方差则尽可能大（在正交的约束下，取最大的K个方差）

## （3）协方差矩阵

![](clip_image008.png)

![](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image010.png)

对角线为a、b方差。其余部分为ab协方差，优化目标统一

## （4）协方差矩阵对角化

设原始数据矩阵X对应的协方差矩阵为C，而P是一组基按行组成的矩阵，设Y=PX，则Y为X对P做基变换后的数据。设Y的协方差矩阵为D

![](clip_image012.png)

# 优化目标

寻找一个矩阵P，满足D是一个对角矩阵，并且对角元素按从大到小依次排列，那么P的前K行就是要寻找的基，用P的前K行组成的矩阵乘以X就使得X从N维降到了K维并满足上述优化条件

![](clip_image014.png)

其中e为特征向量

![](clip_image016.png)

其中Lamda为特征值

![](clip_image018.png)