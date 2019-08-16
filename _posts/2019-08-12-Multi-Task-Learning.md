---
layout: post
title: 多任务学习Multi-Task-Learning
categories: 计算机视觉
comments: false
description: 
keywords: 深度学习
mathjax: true
---

#### 多任务学习

多任务学习涉及多个相关的任务同时并行学习，梯度同时反向传播，多个任务通过底层的共享表示（shared representation）来互相帮助学习，提升泛化效果。多任务学习最简单的共享方式，多个任务在浅层共享参数。
<br>

**Regression** is used to predict continuous values. **Classification** is used to predict which class a data point is part of (discrete value). 

<br>


![](/images/blog/2019-08-12-19-19-49.jpg)

对于机器学习或者深度学习来说，大致的任务类型可以分为：监督学习和无监督学习。对于现在的主要应用场景，是通过大量的标注数据学习的，现阶段数据与模型各占一半的重要性。细分任务的话，又可以分为分类和回归：离散值和连续值得区别。

那么问题来了：
- Is there any algorithm combining classification and regression?


##### 方法1

```mathjax
Loss = k·MSE_{reg}+(1-k)CrossEntropy_{clc}
```

经过实验，这种方法比固定一个网络去训练另一个要简单一些，就是这个系数需要根据训练过程去调整。对于两个数据集标注类型不同，输入不同。这个需要对齐。

另外torch真的很好用，除了通过enumerate方式读批量数据，还可以用next方式去调用，在两个patchsize不同的情况下，可以分别计算loss，进行回传。

``` python

dataloader_iterator = iter(dataloader)
for i in range(iterations):     
    try:
        X, Y = next(dataloader_iterator)
    except:
        dataloader_iterator = iter(train_loader)
        X, Y = next(dataloader_iterator)
    do_backprop(X, Y)
```

可以参考得论文，目前我只知道MTCNN（Multi-task Cascaded Convolutional Networks），这篇论文采用级联网络将人脸检测+定位+landmark对齐。MTCNN人脸检测是2016年的论文提出来的，MTCNN的“MT”是指多任务学习(Multi-Task)，在同一个任务中同时学习”识别人脸“、”边框回归“、”人脸关键点识别“。

<br>
根据智慧金融研究所一篇文章<[MTCNN人脸检测推断过程](http://www.sfinst.com/?p=1683)>，可以简单总结MTCNN有几个特点。

- MTCNN使用了上图的(a)图像金字塔来解决目标多尺度问题，即把原图按照一定的比例(如0.5)，多次等比缩放得到多尺度的图片。但是这种操作是比较慢的。
- MTCNN算法可以接受任意尺度的图片,因为使用了Kaiming He等提出的SPP（Spatial Pyramid Pooling，空间金字塔池化）
- 整体网络由三个子网络级联组成。处理流程为：1.金字塔。2.Proposal Network(P-Net)、Refine Network(R-Net)、Output Network(O-Net)
![](http://www.sfinst.com/wp-content/uploads/2018/12/6-1.jpg)
![](/images/blog/2019-08-12-20-37-13.jpg)



