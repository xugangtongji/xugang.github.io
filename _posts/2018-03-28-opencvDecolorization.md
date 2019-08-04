---
layout: post
title: 不损失颜色的灰度化方法
categories: 计算机视觉
comments: false
description: 
keywords: opencv
mathjax: true
---


![](/images/blog/2018-03-28-10-46-43.jpg)

OpenCV和Matlab中灰度化的方法都非常简单：

$$Y = 0.2989 \times R +  0.5870 \times G + 0.1140 \times B$$

很明显，这种线性灰度化方法用于解决这一非线性问题，有些时候会导致图像对比度严重损失。

看到OpenCV3.x中集成了新的灰度化方法[decolor](http://docs.opencv.org/3.0-beta/modules/photo/doc/decolor.html)：

原论文：[Contrast Preserving Decolorization, ICCP 2012](http://www.cse.cuhk.edu.hk/~leojia/papers/decolorization_iccp12.pdf)，方法也很简单，相比于简单的使用R/G/B线性组合，作者构造了更为复杂的多项式拟合,来构造灰度化函数。

```c++
#include <opencv2/opencv.hpp>
using namespace cv;
int main()
{
    Mat src = imread("test.png");
    Mat grayScale, color_boost;
    decolor(src, grayScale, color_boost);

    imshow("test", grayScale);
    waitKey(0);
    return 0;
}
```


![](/images/blog/2018-03-28-10-48-30.jpg)
