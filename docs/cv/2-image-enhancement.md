## **前言**

图像增强是图像处理中一种常用的技术，它的目的是增强图像中全局或局部有用的信息。合理利用图像增强技术能够针对性的增强图像中感兴趣的特征，抑制图像中不感兴趣的特征，这样能够有效的改善图像的质量，增强图像的特征。

## **介绍**

计算机视觉主要有两部分组成：

- 特征提取
- 模型训练

其中第一条特征提取在计算机视觉中占据着至关重要的位置，尤其是在传统的计算机视觉算法中，更为明显，例如比较著名的HOG、DPM等目标识别模型，主要的研究经历都是在图像特征提取方面。图像增强能够有效的增强图像中有价值的信息，改善图像质量，能够满足一些特征分析的需求，因此，可以用于计算机视觉数据预处理中，能够有效的改善图像的质量，进而提升目标识别的精度。

图像增强可以分为两类：

- 频域法
- 空间域法

首先，介绍一下频域法，顾名思义，频域法就是把图像从空域利用傅立叶、小波变换等算法把图像从空间域转化成频域，也就是把图像矩阵转化成二维信号，进而使用高通滤波或低通滤波器对信号进行过滤。采用低通滤波器（即只让低频信号通过）法，可去掉图中的噪声；采用高通滤波法，则可增强边缘等高频信号，使模糊的图片变得清晰。

其次，介绍一下空域方法，空域方法用的比较多，空域方法主要包括以下几种常用的算法：

- 直方图均衡化
- 滤波

## 直方图均衡化

直方图均衡化的作用是图像增强，这种方法对于背景和前景都太亮或者太暗的图像非常有用。直方图是一种统计方法，根据对图像中每个像素值的概率进行统计，按照概率分布函数对图像的像素进行重新分配来达到图像拉伸的作用，将图像像素值均匀分布在最小和最大像素级之间。

![KfQ9bt.png](https://s2.ax1x.com/2019/10/29/KfQ9bt.png)

具体原理和示例如下：

原图像向新图像的映射为：

$$s_k = \sum_{j=0}^{k}{\frac{n_j}{n}}, k=0, 1, ...,L-1$$ 

其中 ![L](https://www.zhihu.com/equation?tex=L)L 为灰度级。

用直白的语言来描述：把像素按从小到大排序，统计每个像素的概率和累计概率，然后用灰度级乘以这个累计概率就是映射后新像素的像素值。

![KfQiUf.png](https://s2.ax1x.com/2019/10/29/KfQiUf.png)

假设一幅图像像素分布如图，像素级为255，利用直方图对像素分布进行统计，示例如下：

![KfQAPS.png](https://s2.ax1x.com/2019/10/29/KfQAPS.png)

## 滤波

基于滤波的算法主要包括以下几种：

- 均值滤波
- 中值滤波
- 高斯滤波

这些方法主要用于图像平滑和去噪，在前一讲中已经阐述，感兴趣的可以看一下[【动手学计算机视觉】第一讲：图像预处理之图像去噪](https://zhuanlan.zhihu.com/p/57521026)。

## **编程实践**

```shell
完整代码地址： 
https://github.com/jakpopc/aiLearnNotes/blob/master/computer_vision/image_enhancement.py 
requirement:matplotlib/opencv
```

> 本文主要介绍直方图均衡化图像增强算法，前一讲已经实现了滤波法，需要的可以看一下。

首先利用opencv读取图像并转化为灰度图，图像来自于voc2007:

```python
img = cv2.imread("../data/2007_000793.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
```

![KfQZvj.png](https://s2.ax1x.com/2019/10/29/KfQZvj.png)

可以显示图像的灰度直方图：

```python
def histogram(gray):
    hist = cv2.calcHist([gray], [0], None, [256], [0.0, 255.0])
    plt.plot(range(len(hist)), hist)
# opencv calcHist函数传入5个参数：
# images：图像
# channels：通道
# mask：图像掩码，可以填写None
# hisSize：灰度数目
# ranges：回复分布区间
```

![KfQMV0.png](https://s2.ax1x.com/2019/10/29/KfQMV0.png)

直方图均衡化，这里使用opencv提供的函数：

```python
dst = cv2.equalizeHist(gray)
```

![KfQ8GF.png](https://s2.ax1x.com/2019/10/29/KfQ8GF.png)

均衡化后的图像为：

![KfQsRe.png](https://s2.ax1x.com/2019/10/29/KfQsRe.png)

可以从上图看得出，图像的对比度明显比原图像更加清晰。

------

## 往期回顾

[Jackpop：【动手学计算机视觉】第一讲：图像预处理之图像去噪](https://zhuanlan.zhihu.com/p/57521026)

------

> 感兴趣的可以关注一下，也可以关注公众号"平凡而诗意"，我在公众号共享了一些资源和学习资料，关注后回复相应关键字可以获取。


