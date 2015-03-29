# geo-experiment

> `experiment`一个定位相关的试验项目，下面是简单思路与方案

### Why?

> 通过经纬度，快速定位区域，常见`吃喝玩乐`、`群活动`等同城或同区域需求，在不通过服务器的前端快速定位方案

### WTF!

> Q: 脑子有问题？为啥需要前端来定位？
> A: 为强定位H5 APP，提供一种性能优化的新思路。

### 原理

* 为区域画矩形

类似的，我们可以得到一个深圳的矩形：

(22.8617484, 113.7569184)            (22.8617484, 114.627296)

(22.4450271, 113.7569184)            (22.4450271, 114.627296)

即对于位置(lat, lng)，满足`22.4450271 <= lat <= 22.8617484 && 113.7569184 <= lng <= 114.627296`，则有可能在深圳。

* SVM（Support vector machine）

接下来就是`机器学习`经常遇到的场景，有试验大量样本的情况下，如何得到一个超平面来对样本分类，使得任何一个新的样本可以通过这个超平面来分类。

`SVM`的科普介绍请参见我写的几篇文章：
  1. [OpenCV 2.4+ C++ SVM介绍](http://www.cnblogs.com/justany/archive/2012/11/23/2784125.html)
  2. [OpenCV 2.4+ C++ SVM线性不可分处理](http://www.cnblogs.com/justany/archive/2012/11/26/2788509.html)
  3. [OpenCV 2.4+ C++ SVM文字识别](http://www.cnblogs.com/justany/archive/2012/11/27/2789767.html)

更多详情请参见：
  1. [wikipedia](http://en.wikipedia.org/wiki/Support_vector_machine)
  2. [支持向量机通俗导论（理解SVM的三层境界）](http://blog.csdn.net/v_july_v/article/details/7624837)
  
故通过样本提取，我们可以利用[node-svm](https://github.com/nicolaspanel/node-svm)来训练，找出该超平面以及对应的参数。

反过来通过这个来确定任意一个经纬坐标是否是深圳这个区域。

### 实施

* 样本准备

在刚刚说的区域以一定间隔产生样本，通过google maps api，使用方法参见[geonode](https://github.com/feliperazeek/geonode)，来确定样本点是否在深圳。

* 将样本丢进`node-svm`training

* 产生超平面参数

* 在真实场景中统计误差
