---
layout: post
title: 手部跟踪基于点云匹配的方法
categories: 计算机视觉
comments: false
description: 
keywords: 论文、手势识别
mathjax: true
---

## Robust Articulated-ICP for Real-Time Hand Tracking
[github 地址](https://github.com/OpenGP/htrack)

windows10 编译太痛苦了，还是ubuntu大法好。注意依赖库的32/64位区别，debug和release的区别，不同版本，opencv特别注意，注意Cmake使用的编译器gcc和g—++版本统一。cuda7改cuda8。另外的难点就是相机的接口和SDK。Creative Senz3D测试通过，这相机已经很老了，win10已经不持支了，ubuntu使用的是DepthSenseGrabber，需要#include <DepthSense.hxx>这个是从sony官网下载的，重新编译过的。


3D点云匹配+时间运动先验+骨骼先验（data driven）

![](/images/blog/2018-05-08-14-57-32.jpg)

主要贡献点：
	1. 将数据驱动的骨骼先验融合到了实时tracking的模块里
	2. 根据depthmap提取silhouette image剪影图像
	3. a new regularization strategy 数据到模型
	4. 点到平面匹配的能量函数和牛顿线性优化的关系

突出贡献：regularized geometric registration【规范的 几何的 流程化】

目前手骨骼提取的方法主要是：

- Appearance-based



将图像特征映射位Pose参数，使用数据训练的方式：分类或回归。使用情形：有较好的特征，只是粗略进行的位姿状态估计。常用方法：KNN，随机森林，卷积神经网络。

- model-based methods

基于模型的方法实际上是匹配优化的问题，定义目标函数，根据观测的数据和模型产生的数据，二者之间的匹配程度。难点在tracking loss的判断。本文采用的model-based 的tracking方法，利用一些设计先验来加强跟踪的鲁棒性。

In this work we focus on improving the robustness and accuracy of model-based
approaches by combining effective 2D and 3D registration energies with carefully designed priors.


###  生成方法（Generative Methods）

         生成方法（基于模型）（Generative mthods: model-based）

         - 步骤：首先，创建大量的手势；然后，选择一个最匹配当前深度图像的手势

         - 目标函数（objective function）：基于输入深度图与手模型近似深度图的相似性，然后对此目标函数进行优化，以找到最接近的手模型。

         - 缺点：

          （1）优化(找最匹配的)计算量大 

          （2）其精确性高度依赖人工创建的相似性函数（similarity function）

          （3）如果前面的估计不准确，易于出现错误累积

          （4）为减轻普遍存大的模型漂移（model drift），近来采用“优化+重新初始化”范式

###  判别方法（Discriminative Approaches）

         **判别方法**（基于外貌）（Discriminative approaches：appearane based）

         - 学习从深度图像到手势配置的映射（手势 = mapping(深度图像)）

         - 手深度图低分辨率、自我遮挡、快速移动会产生大量错误

         - 基于局部回归（local regression）的方法：可以提高对遮挡的鲁棒性，但是易产生帧间抖动

1）追踪与检测（Trackers versus Detectors）：

            检测：基于单帧的方法，每帧都会重新初始化它自己

            追踪：基于多帧的方法，不能从错误中立即恢复

2）数据驱动与模型驱动（Data-driven vensus Model-driven）：

           数据驱动：拿着数据总结模型（根据已知数据寻求本质规律）；对于单个图像检测，各种快速的分类算法可以实时地实现；这些分类器由几何模型合成的数据进行训练，可以看作是模型的近似拟合

           模型驱动：拿着模型找与之匹配的数据（已经知道本质规律，来对数据进行判断）；优化一个几何模型以拟合观察到的数据；其目标函数容易出现局部最优；它在追踪领域取得了很大的成功，它的初始化限制了搜索空间



hand-object interactions：行为识别也存在于物体间的交互


Particle-swarm optimization 粒子群算法 【Microsoft Research】
PSO模拟鸟群的捕食行为。设想这样一个场景：一群鸟在随机搜索食物。在这个区域里只有一块食物。所有的鸟都不知道食物在那里。但是他们知道当前的位置离食物还有多远。那么找到食物的最优策略是什么呢。最简单有效的就是搜寻目前离食物最近的鸟的周围区域。
PSO中，每个优化问题的解都是搜索空间中的一只鸟。我们称之为“粒子”。所有的粒子都有一个由被优化的函数决定的适应值(fitness value)，每个粒子还有一个速度决定他们飞翔的方向和距离。然后粒子们就追随当前的最优粒子在解空间中搜索。

为了达到实时性：计算2D/3D最临近点匹配时采用的是closed form和并行计算

手指看成圆柱模型，有效抵抗噪声


### ICP算法

ICP算法能够使不同的坐标下的点云数据合并到同一个坐标系统中，首先是找到一个可用的变换，配准操作实际是要找到从坐标系1到坐标系2的一个刚性变换。

<center>
![](/images/blog/2018-09-09-11-00-12.jpg)
</center>

假设给两个三维点集 X1 和 X2，ICP方法的配准步骤如下：

第一步，计算X2中的每一个点在X1 点集中的对应近点；

第二步，求得使上述对应点对平均距离最小的刚体变换，求得平移参数和旋转参数；

最小化目标函数：


求解

第三步，对X2使用上一步求得的平移和旋转参数，得到新的变换点集；

第四步， 如果新的变换点集与参考点集满足两点集的平均距离小于某一给定阈值，则停止迭代计算，否则新的变换点集作为新的X2继续迭代，直到达到目标函数的要求。

最近点对查找：对应点的计算是整个配准过程中耗费时间最长的步骤，查找最近点，利用 k－d tree提高查找速度 K－d tree 法建立点的拓扑关系是基于二叉树的坐标轴分割，构造 k－d tree 的过程就是按照二叉树法则生成，首先按 X 轴寻找分割线，即计算所有点的x值的平均值，以最接近这个平均值的点的x值将空间分成两部分，然后在分成的子空间中按 Y 轴寻找分割线，将其各分成两部分，分割好的子空间在按X轴分割……依此类推，最后直到分割的区域内只有一个点。这样的分割过程就对应于一个二叉树，二叉树的分节点就对应一条分割线，而二叉树的每个叶子节点就对应一个点。这样点的拓扑关系就建立了。