---
Deep Laplacian Pyramid Networks for Fast and Accurate Super-Resolution
---

[TOC]



# Abstract

1. 逐级放大子带残差来学习高分辨图像
2. 利用转置卷积进行上采样
3. 不需要利用`bicubic`进行下采样，减少计算量
4. 提出`Charbonnier Loss`实现高分辨率重建

5. 实现了多倍超分辨，促进`resource-aware applications`

# Introduction

> 单张图像超分辨旨在将单张低分辨率图像作为输入从而得到重建的高分辨率图像

1. `基于实例的重建方法`学习LR和HR之间的图像块`image patches` [^1]的映射规则：
   - 基于字典学习方法
   - 基于局部线性回归方法
   - 基于随机预测方法

2. 基于**学习**的重建方法：`SRCNN`，通过嵌入一个基于稀疏编码的网络结构或者使用更深的网络，但是存在以下**三个缺点**：
   - 提前进行了**上采样**到想要的超分辨倍率，使得计算都在**高维度**空间，导致计算开销很大，也会产生**重建的伪影**。虽然后面也有采用**转置卷积**或**亚像素卷积**的方式代替`bicubic`，但是受限于网络规模也无法学习到复杂的映射关系
   - 目前多数方法利用`L2 Loss`作为损失函数，带来了重建后的模糊感。因为`L2`无法捕获 HR 和 LR 对应 patches 之间的潜在多模分布。
   - 目前网络的重建部分都是一步到位的，在放大倍数很大时是困难的。这也导致了它们无法获得超分辨的中间预测结果。如：超分8倍，一步到位到8倍，缺少了可用的2倍和4倍预测。
3. 基于上述三个弊端，提出了基于<font color='red'>**级联**</font>卷积神经网络的`LapSRN`模型：
   - 只需一张LR输入，逐级重建得到不同放大规模的子带残差`sub-band residuals`
   - <font color='green'>对于每一级的放大，都是先使用卷积层来进行提取特征层`feature maps`，然后对特征层进行转置卷积进行放大到更高的水平，然后用一个卷积层来预测子带残差。将子带残差和上采样后的图像进行相加来重建HR。如图1所示</font>
   - `Charbonnier Loss`

![Snipaste_2020-12-05_14-13-00](https://tvax4.sinaimg.cn/large/005tpOh1ly1glcyay6d2kj30sr0ktgpq.jpg)

<center><font color='green'><b>图1 LapSRN网络结构</b></font></center>

![Snipaste_2020-12-05_14-17-44](https://tva4.sinaimg.cn/large/005tpOh1ly1glcyfn0qaqj30mi0apjsv.jpg)

<center><img src="https://tva1.sinaimg.cn/large/005tpOh1ly1gld20a2trfj30np06274w.jpg" alt="Snipaste_2020-12-05_16-21-22" width="853" data-width="853" data-height="218"></center>

<center><img src="https://tva1.sinaimg.cn/large/005tpOh1ly1gld2o8l5yyj30w50h4q6q.jpg" alt="Snipaste_2020-12-05_16-44-25" width="1157" data-width="1157" data-height="616"></center>

<center><font color='green'><b>图2 其他超分辨网络</b></font></center>

4. <font color='red'><b>从三个方面来对比`LapSRN`和其他超分辨网络：</b></font>

- 准确率：`LapSRN`直接从LR卷积[^2]来进行特征提取，使用卷积核来优化滤波器从而预测子带残差。`Charbonnier Loss`能够更好地处理边缘细节，所以提升了网络的表现效果。其他，网络的学习能力足够，能过学习到更多复杂的映射关系，从而减少伪影，提高了准确度。
- 速度：`LapSRN`模型速度超过其他大部分模型，而且在有更好的重建准确度。
- 逐级重建：可以通过一个输入，通过逐级重建产生多个中间的超分辨预测[^3]。其他网络没有这么好的灵活性。



[^1]: 因为图像的像素往往比较大，无法整张input，所以要使用其patches来作为输入，一般获取patches的两种方式[`centercrop` 和 分块](https://www.cnblogs.com/zgqcn/p/13997436.html)
[^2]: 比较[从LR卷积与上采样后卷积的优缺点](https://www.cnblogs.com/zgqcn/p/14034289.html#%E6%9C%89%E7%9B%91%E7%9D%A3%E5%AD%A6%E4%B9%A0%E7%9A%84%E8%B6%85%E5%88%86%E8%BE%A8%E6%96%B9%E6%B3%95)

[^3 ]: 如超分辨8倍时，可以同时也产生超分辨2倍和超分辨4倍的结果，这就是逐级重建带来的好处



# Related Work and Problem Context

1. 基于**内部数据库**的超分辨重建：
   - 利用自相似性，构建基于低分辨率输入图像的尺度空间金字塔
   - 分解图像块到特定频率的子带，在每一子带中独立地确定它们之间的更好映射关系
   - 通过扩展图像块的搜索空间来容纳仿射变换和透视变形

- 基于内部数据集的主要确实是计算量大，计算慢

2. 基于**外部数据库**的超分辨重建，利用监督学习的方式：
   - `nearest neighbor`、`manifold embedding`、`kernel ridge regression`、 `sparse representation`等
   - 利用`K-means`、`sparse dictionary`、`random forest`等方法对图像进行分区归类

3. <font color='blue'>基于**卷积神经网络**的超分辨重建：</font>

- `SRCNN`：共同优化所有步骤，学习图像空间的**非线性映射**
- `VDSR`：进一步增加卷积层到20层，重建效果超过`SRCNN`。但是网络太大太深，影响学习的速度，<font color='red'>所以`VDSR`进行网络残差的学习，而不是真实的像素重建。</font>
- `SCN`：结合稀疏编码域知识和级联网络模型，从而实现了对特定放大倍数的逐级超分辨[^4]
- `DRCN`：利用递归模块构建较为浅层的网络从而减少了参数量
- `ESPCN`：为了实时表现，在LR的空间进行特征提取[^2]，而不是先使用`bicubic`进行上采样，最后才进行亚像素卷积进行上采样。
- `FSRCNN`：采用了一个带有更多卷积层但参数量更少的沙漏形状的卷积网络实现实时表现
  - <font color='red'>以上网络全部采用`L2 Loss`，这将导致过于平滑，不符合人眼感知</font>

- ⭐`LapSRN`：
  - 通过卷积层和转置卷积层通过学习残差和上采样的参数。使用学习后的上采样滤波器，不仅有效地抑制了重建的伪影，而且减少计算的复杂度
  - 利用`Charbonnier Loss`能够更好地处理边缘细节，提升超分辨精度
  - 逐级放大，同时获得不同倍数的超分辨结果

[^4]: [Deep networks for image super-resolution with sparse prior](http://www.ifp.illinois.edu/~dingliu2/iccv15/)

4. 拉普拉斯金字塔

   > 拉普拉斯金字塔已被广泛应用到多个领域，如：图像盲复原、纹理图像生成、边缘感知过滤、语义分割、图像去雨去雾等领域。

   - 本论文中主要讨论了`LapSRN`与`LAPGAN`的不同之处，这里不做重点
   - 我之前看过`厦大smartDSP实验室`的论文，也是有关拉普拉斯金字塔的应用[LPNet](https://www.cnblogs.com/zgqcn/p/14048801.html)

5. 对抗训练

- `SRGAN`：通过**感知损失**和**对抗损**失来对真实图像超分辨。
  - 作者表示`LapSRN`可以容易地应用对抗训练的框架，但是论文中没有具体阐述

<center><font color='green'><b>Table1 比较多种网络模型</b></font></center>

<center><img src="https://tva2.sinaimg.cn/large/005tpOh1ly1gld2rdmfnfj314509f772.jpg" alt="Snipaste_2020-12-05_16-47-19" width="1445" data-width="1445" data-height="339"></center>

# <font color='red'>Deep Laplacian Pyramid Network for SR</font>

> 网络利用单一输入，在![](https://latex.codecogs.com/png.latex?\log_2S)层进行残差的预测，![](https://latex.codecogs.com/png.latex?S)是放大倍数

## 网络结构

### 特征提取`Feature Extraction`

1. 在第 `s` 层，特征提取使用 `d` 个卷积层和一个转置卷积层来获得放大 2 倍的特征图。每个特征提取的转置卷积层的输出与两个层连接：
   - 和利用转置卷积重建后的图来计算残差
   - 作为下一级放大的输入来获得 `s + 1` 层

2. 在低分辨率上进行特征提取，仅使用一个转置卷积层便可以生成更好分辨率的特征图

3. 减少了计算的复杂度。因为在低超分倍数时的参数可以共享到更好的放大倍数，因此也增强了非线性映射的学习

### 图像重建`Image Reconstruction`

1. 在第`s`层，利用一个转置卷积来进行2倍的放大。利用`blinear kernel`来对转置卷积进行初始化，并使它随着网络的学习参数共同优化。
2. 上采样后的图像利用主元素相加的方式与预测的残差进行求和，从而得到一个高分辨输出，利用该输入作为下一级放大的输入，得到第`s+1`层

## Loss Function

在第![](https://latex.codecogs.com/png.latex?s)层，假设网络重建后的图像是![](https://latex.codecogs.com/png.latex?x_s)，特征提取后的残差为![](https://latex.codecogs.com/png.latex?r_s)，所以网络的输出为![](https://latex.codecogs.com/png.latex?y_s=x_s+r_s)，与输出进行损失计算的`GT`为![](https://latex.codecogs.com/png.latex?\hat{y_s})
$$
L(\hat{y},y;\theta) = \frac{1}{N}\sum_{i=1}^{N}\sum_{s=1}^{L}\rho(\hat{y}_{s}^{(i)}-y_{s}^{(i)}) \\
= \frac{1}{N}\sum_{i=1}^{N}\sum_{s=1}^{L}\rho((\hat{y}_{s}^{(i)}-x_{s}^{(i)})-r_s^{(i)})
$$
其中![](https://latex.codecogs.com/png.latex?\rho=\sqrt{x^2+\varepsilon^2})是`Charbonnier Loss`的惩罚函数。![](https://latex.codecogs.com/png.latex?N)是训练样本的`batches`，![](https://latex.codecogs.com/png.latex?L)是金字塔的层数。根据经验，设置![](https://latex.codecogs.com/png.latex?\varepsilon)为![](https://latex.codecogs.com/png.latex?1e-3)。

在每一级放大中，都有每一级自己的`loss function`和不同大小的相应`Ground Truth`用于计算，所以`LapSRN`是**多损失函数**的结构。

## Implementation and training details

1. 对于每一个卷积层采用64个`3*3`的卷积核
2. 利用[`He Kaiming`初始化方法](https://www.cnblogs.com/zgqcn/p/14067327.html#kaiming%E5%88%9D%E5%A7%8B%E5%8C%96)对卷积核进行初始化
3. 采用`4*4`的转置卷积滤波器，并利用`bliinear filter`初始化
4. 所有层的输出都利用负斜率为0.2的`leaky rectified linear unit`进行激活
5. 利用`padding`在图像边缘补零，实现`the same`计算
6. 虽然`3*3`卷积核的空间支持比较小，但是可以通过增加网络深度来实现更好的非线性映射

7. 数据集：
   - 91张来自Yang et al.
   - 200张BSDS
8. 训练参数：
   - 使用`128*128pixel`的64 patches
   - 每epoch进行1000次的反向传播计算
   - 数据扩增：比例缩放参数`[0.5, 1.0]`，旋转{90°，180°，270°}，随机翻转{水平，垂直}
   - 利用`bicubic`来获得LR图像
   - 利用`MatConvNet toolbox`训练
   - `momentum parameter = 0.9`, `weight dacay = 1e-4`
   - 初始`lr = 1e-5`, 每50 epochs减半

# Experiment Results

### Model analysis

> 为了证明`LapSRN`网络结构及其提出loss的正确性和准确性，作者提出了多个对照实验来证明，如图3

1. 去除重建分支（无残差学习）后，训练波动大，性能比完整的低

2. 将Loss改为L2 Loss，要训练更多的epoch才能超过`SRCNN`，且会产生伪影
3. 去除金字塔结构同时改为相应的卷积层，PSNR上更低了
4. 加深网络，采用更多的卷积层能提升效果，但也增加了计算量，所以还是使用10个来达到平衡

![Snipaste_2020-12-06_10-29-12](https://tva2.sinaimg.cn/large/005tpOh1ly1gldxgx1aokj30yf0b0juj.jpg)

![Snipaste_2020-12-06_10-29-54](https://tvax3.sinaimg.cn/large/005tpOh1ly1gldxh4gg2hj30zu08ftbk.jpg)

<center><font color='green'><b>图3 对照实验</b></font></center>

### Comparisons with the state-of-arts

使用5个数据集对多个不同网络进行比较，数据集为：SET5，SET14，BSDS100，URBAN100，MANGA109。基本超过了目前模型，如图4

<center><img src="https://tvax2.sinaimg.cn/large/005tpOh1ly1gldxmw5wxyj30pk0o2qaf.jpg" alt="Snipaste_2020-12-06_10-35-41" width="920" data-width="920" data-height="866"></center>

![Snipaste_2020-12-06_10-36-37](https://tvax4.sinaimg.cn/large/005tpOh1ly1gldxnxrubdj30qe0jz13t.jpg)

<center><font color='green'><b>图4 视觉比较及量化比较</b></font></center>

1. 将低层模型的训练到最小错误时，再去训练更高层可以提升效果。
2. `LapSRN`在网络、条纹上超分有优势。那些利用先上采样后超分辨的方法，会产生伪影
3. `SelfExSR`和`SCN`也可以逐级放大，但是`LapSRN`速度更快

Execution time

> 在同样的情况下进行训练对比，除了略低于`FSRCNN`，比其他的网络快，如图5

<center><img src="https://tvax1.sinaimg.cn/large/005tpOh1ly1gldxw7pyv1j30jh0eagnw.jpg" alt="Snipaste_2020-12-06_10-44-38" width="701" data-width="701" data-height="514"></center>

<center><font color='green'><b>图5 训练速度比较</b></font></center>

### Super-resolving real-world photos

![Snipaste_2020-12-06_10-47-23](https://tva1.sinaimg.cn/large/005tpOh1ly1gldxz35dbxj31490arwiq.jpg)

<center><font color='green'><b>图6 真实图像对比</b></font></center>

### Super-resolving video sequences
<center><img src="https://tvax1.sinaimg.cn/large/005tpOh1ly1gldy07930qj30iz0b5wg3.jpg" alt="Snipaste_2020-12-06_10-48-29" width="683" data-width="683" data-height="401"></center>

<center><font color='green'><b>图7 视频超分辨对比</b></font></center>

1. 更清晰
2. 实现`real-time performance`

### Limitations

1. 对精细结构超分辨：除了`selfExSR`，其他都很难实现这种的精细结构超分辨，如图8
2. `LapSRN`的网络规模比较大，为了减少计算量，所以也减少了卷积层数量，

<center><img src="https://tvax4.sinaimg.cn/large/005tpOh1ly1gldy3ujne4j30jj0aw76f.jpg" alt="Snipaste_2020-12-06_10-51-10" width="703" data-width="703" data-height="392"></center>

<center><font color='green'><b>图8 LapSRN不足之处</b></font></center>

# Conclusion

1. 提出的网络实现了更快更准确的单一图像超分辨
2. 通过`coarse-to-fine`的方式逐级计算高分辨图像的残差
3. 使用转置卷积代替`pre-upsampling`
4. 提出了`Charbonnier Loss`，减轻了超分辨伪影
5. 在主观指标和量化指标上，超越了目前的`state-of-the-art`



> **Write by Gqq**