---
layout: post
title: CUM： Panoptic Studio
categories: 肢体检测
comments: false
description: 
keywords: 数据库，openpose
---
#DIY A Multiview Camera System:Panoptic Studio
身体语言可以传达巨大的信息，人类会通过肢体语言，面部表情表现出来一些真实的情感，甚至肢体语言是全人类的通用型语言，这是语言所不能达到的。

> “body language is an elaborate code that is written nowhere, known to no one, and understood by all” 
--------[Sapir, 1949]

参数简介：
1.摄像机：480 VGA, 31 HD, 10 RGBD
2.531 GB/秒
数据库：
1.相机参数：Calibration Data (VGAs + HDs + Kinects)
2.视频：crf压缩格式；kinects深度信息未提供
3.3D Pose 坐标[[方法]](http://oxyyfe6db.bkt.clouddn.com/PanopticStudio2016.pdf)；定义15个