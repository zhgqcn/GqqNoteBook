# `Lightweight Pyramid Networks for Image Deraining`

> 作者：厦门大学信通系`SmartDSP`实验室，[项目官网](https://xueyangfu.github.io/projects/LPNet.html)
>
> 题目：用于图像去雨的轻量级金字塔网络

# 摘要

1. 目前存在的去雨网络存在<font color='red'>大量的训练参数</font>，不利于移动端的落地使用
2. 利用<font color='red'>特殊领域</font>的知识简化学习过程：
   - 引入<font color='red'>高斯拉普拉斯图像金字塔分解技术</font>，所以在每一级上的学习任务可以简化为参数量较少的浅层网络
   - 利用<font color='red'>递归</font>和<font color='red'>残差</font>网络模块，所以在参数量<font color='blue'>少于8000</font>时也能实现目前最好的去雨效果

# 引言

雨天是常见的天气系统，不仅影响人们的视觉，也影响着计算机视觉系统，如：<font color='red'>自动驾驶、监控系统</font>等。因为<font color='red'>光的折射和散射</font>，图像中的物体容易<b>被雨丝模糊和遮挡</b>。当遇到大雨时，稠密的雨丝将使得这种现象更为严重。因为目前的计算机视觉系统的输入设定是干净清晰度的图像，所以模型的精准度在雨天时将很容易退化。所以，设计高效且有用的去雨算法对于许多应用的落地是十分重要的。

## 相关的工作

1. <font color='red'>视频去雨</font>：可以利用相邻帧时间的时空信息，如利用相邻帧之间的平均密度来从静态背景中去雨。有以下方法：
   
   - 在傅里叶域进行，使用高斯混合模型、低秩逼近、通过矩阵完成
   - 将雨丝划分为稀疏区和密集区，然后利用基于矩阵分解的算法
- 基于补丁的高斯混合矩阵
   
2. <font color='red'>单帧图片去雨</font>：单帧去雨无法利用相邻帧信息，所以比视频去雨更加困难。有以下方法：

   - 使用内核、低秩逼近、字典学习
   - 核回归和非局部均值滤波器
   - 将图像分为高频和低频部分，对高频部分使用基于稀疏编码的字典学习来分离和去雨
   - 从高频部分进行自学习去雨
   - 区分性编码：通过迫使雨层的稀疏向量稀疏化，目标函数可以解决将背景和雨丝分离的问题
   - 混合模型和局部梯度下降
   - 高斯混合模型`GMMs`：背景层从自然图像中学习，雨层从带雨图像中学习
   - 可变方向乘法器`ADMM`
   - <font color='green'><b>基于卷积神经网络的深度学习方法</b></font>

   

## 本文贡献

- 提出<font color='blue'>`LPNet`模型</font>，该模型<font color='red'>参数量少于8k</font>，相较于以往的深层神经网络，更有利于在移动端的应用落地。
- 利用<font color='red'>特殊领域</font>的知识简化学习过程
- 首次采用<font color='red'>拉普拉斯金字塔</font>模块来分解降级图片和有雨图片到不同的网络层次
- 使用<font color='red'>递归模块</font>和<font color='red'>残差模块</font>建立子网络来重建去雨图像的每层级的<font color='red'>高斯金字塔</font>
- 根据不同层级的物理特性采用<font color='red'>特殊`Loss`函数</font>
- 实现的<font color='red'>目前最好</font>的去雨效果，在其他计算机视觉领域也有很强的可应用性



# <font color='red'>LPNet网络模型</font>

![Snipaste_2020-11-27_10-50-39](https://tvax1.sinaimg.cn/large/005tpOh1ly1gl3jhrm4v4j317w0kn433.jpg)

<center><b><font color='green'>LPnet网络及组成模块</font></b></center>

## Motivation

1. 雨丝会被物体边缘和背景遮挡，所以很难在图像域进行学习，但是可以在<font color='red'>高频信息</font>上学习，因为高频部分主要包含了雨丝和物体边缘信息而不带有图像背景的干扰
2. 此前为了实现以上操作，利用[导引滤波器](https://en.wikipedia.org/wiki/Guided_filter)来获取高频信息作为网络输入，然后进行去雨在进行背景融合，但是在<font color='red'>很细的雨丝上</font>很难提取出高频信息
3. 基于以上分解的想法 ，提出了<font color='red'>轻量级金字塔神经网络</font>简化训练过程，同时减少参数量

## Stage I : 拉普拉斯金字塔

拉普拉斯金字塔的获取要通过高斯金字塔的运算，就是通过<u><b><font color='orange'>高斯下采样后再通过拉普拉斯上采样，在于原图相减来获得高频残差</font></b></u>	👉《[拉普拉斯金字塔与高斯金字塔的关系](https://zhuanlan.zhihu.com/p/94014493)》
$$
L_n(X) = G_n(X) - upsample(G_{n+1}(X))
$$

$$
G_n(X)是高斯金字塔 \qquad L_n(X)是拉普拉斯金字塔
$$

选择传统拉普拉斯金字塔的<font color='red'>优点</font>：

1. 背景去除，所以网络只需要处理<font color='blue'>高频信息</font>
2. 利用每层学习的<font color='blue'>稀疏性</font>
3. 使每层学习更像是<font color='blue'>标识映射`identity mapping`</font>
4. 在`GPU`上容易进行计算

## Stage II : 子网络结构

对于每一层级，都是相同的网络结构，只是卷积核`kernel`数量会根据不同层次特征来进行数量上的调整，主要采用`residual learning`和`recursive blocks`

1. <font color='red'>特征提取</font>

$$
H_{n,0} = \sigma(W_{n}^{0}*L_n(X)+b_{n}^{0})
$$

2. <font color='red'>递归模块</font>`recursive blocks`:利用参数共享到其他几个`recursive blocks`来实现减少训练参数量，主要应用了每一个`block`采用三个卷积如下：

$$
F_{n,t}^{1}=σ(W_{n}^{1}*H_{n,t-1}+b_{n}^{1}), \\
F_{n,t}^{2}=σ(W_{n}^{2}*F_{n,t}^{1}+b_{n}^{2}), \\
F_{n,t}^{3}=W_{n}^{3}*F_{n,t}^{2}+b_{n}^{3},
$$

$$
F^{\{1,2,3\}}是中间特征，W^{\{1,2,3\}}和b^{\{1,2,3\}}是在多个block中共享的参数
$$

​		为了便于前向传播和反向求导就计算，利用<font color='red'>`残差模块`</font>来将输入与递归模块输出相加：
$$
H_{n,t}=σ(F_{n,t}^{3}+H_{n,0})
$$

3. <font color='red'>高斯金字塔重建</font>：

$$
L_n(Y) = (W_{n}^{4}*H_{n,T}+b_{n}^4)+L_n(X),
$$

​		以上公式为<font color='red'>拉普拉斯金字塔的输出</font>，则相应的高斯金字塔重建结果为：
$$
G_N(Y)=max(0,L_N(Y)),\\
G_n(Y)=max(0,L_n(Y)+upsample(G_{n+1}(Y))),
$$
​		因为每层的输出应该`≥0`，所以应有`x=max(0,x)`即`ReLU`函数

​		最终输出的去雨图像为
$$
G_1(Y)
$$

## Loss Function

1. `MSE(L2)`：由于平方惩罚在图像边缘上的训练较差，生成的图像会<font color='red'>过于平滑</font>
2. 所以，对不同的网络层级采用不同的`Loss`来适应不同的网络特征
3. `L1 + SSIM`：因为更好的图像细节和雨丝存在于更低层的金字塔网络中(如Fig 3)，所以使用`SSIM`来训练相应的子网络来<font color='red'>保留更多的高频信息</font>

4. `L1`：因为在更高层的金字塔网络中存在更大的物体结构和平滑背景区域，因此仅使用`L1 Loss`来更新网络参数

$$
L = \frac{1}{M}\sum_{i=1}^{M}\{\sum_{n=1}^{N}L^{l1}(G_n(Y^i),G_n(Y_{GT}^{i}))+
\sum_{n=1}^{2}L^{SSIM}(G_n(Y^i),G_n(Y_{GT}^i))\},
$$



![Snipaste_2020-11-27_15-13-31](https://tva1.sinaimg.cn/large/005tpOh1ly1gl3r386fadj30lf0dtq54.jpg)

<center><font color='green'><b>Fig 3</b></font></center>

## 去除`Batch Normalization`层

1. 加入`BN`层是为了使得训练的特征映射服从<font color='red'>高斯分布`(正态分布)`</font>
2. 但是拉普拉斯金字塔低层数据是<font color='red'>稀疏的</font>`(如Fig3直方图)`，本身比较容易处理，所以不需要BN层约束
3. 去除`BN层`后使得模型更加灵活且参数量更少

## 参数设置

1. 固定的平滑内核`[0.0625,0.25,0.375,0.25,0.0625]`用于拉普拉斯金字塔的构建与高斯金字塔的重建

2. ![Snipaste_2020-11-28_11-05-11](https://tva2.sinaimg.cn/large/005tpOh1ly1gl4pjn356pj30md01zdg7.jpg)
   
3. 如`Fig3`，雨丝在更低层级，高层级更接近于特征映射，所以更高层只需要更少的训练参数即可，故从低层级到高层级，网络核数递减`[16, 8, 4, 2, 1]`。在最高一层，图像是很小且平滑的，雨丝仍然存在于高频部分，但是其学习更像是简单的<font color='red'>全局对比自适应学习</font>。

4. 如`Fig5`，通过对于`Rainy img`和`Our Result`的每一级，可以看得到：雨丝仍然在低层级，然而高层级几乎相同。可见，在更高层去除多余的核是必要的

![Snipaste_2020-11-27_15-58-48](https://tva2.sinaimg.cn/large/005tpOh1ly1gl3seccw9bj313e0ijwj1.jpg)

## 实验参数

- `dataset`：人工合成1800张大雨图像 + 200张小雨图像
- `optimizer`：Adam
- `mini-batch size` : 10
- `learning rate`：0.001
- `epochs`：3

# 实验结果

通过对多个模型在<font color='red'>人工合成雨图[大雨、小雨]、真实世界雨图、去雾方面</font>进行对比，主要比较标准：主观图像输出的视觉比较、量化比较(主观评价分、PSNR、SSIM)

![Snipaste_2020-11-27_16-11-21](https://tva3.sinaimg.cn/large/005tpOh1ly1gl3ssnpgpej315j0byq8t.jpg)

![Snipaste_2020-11-27_16-11-29](https://tva2.sinaimg.cn/large/005tpOh1ly1gl3ssrrixmj314z0bjn1v.jpg)

![Snipaste_2020-11-27_16-11-45](https://tvax2.sinaimg.cn/large/005tpOh1ly1gl3sswdos2j313h0at78q.jpg)

![Snipaste_2020-11-27_16-12-30](https://tvax1.sinaimg.cn/large/005tpOh1ly1gl3st13n0qj30zb06g0vb.jpg)

![Snipaste_2020-11-27_16-13-46](https://tvax1.sinaimg.cn/large/005tpOh1ly1gl3stwmafej30rd0hsn48.jpg)

![Snipaste_2020-11-27_16-15-51](https://tva4.sinaimg.cn/large/005tpOh1ly1gl3sw5e6qlj30na09ijt3.jpg)

## 论文其他内容

由于论文中心思想就是上面部分，虽然其他思想也有提及，但不作为核心

1. `LPNet`在使用更多的网络参数时，可以表现出更佳效果。但为了轻量级应用实施，所以没加
2. `skip connection`有利于前向传播及反向求导
3. `SSIM Loss`基于局部图像特征：局部对比度、亮度、细节。更加符合<font color='red'>人眼系统</font>。
4. `LPNet`也适用于其他计算机视觉任务，如去噪、去雾等领域
5. 由于网络是轻量级的，所以可作为其他计算机视觉任务的预处理：在下雨天的目标检测上先使用`LPNet`去雨网络

![Snipaste_2020-11-27_16-23-44](https://tva2.sinaimg.cn/large/005tpOh1ly1gl3t5ln17bj30wl0b7q6c.jpg)

![Snipaste_2020-11-27_16-23-49](https://tva2.sinaimg.cn/large/005tpOh1ly1gl3t5oysvfj31490bl78y.jpg)

![Snipaste_2020-11-27_16-24-54](https://tva2.sinaimg.cn/large/005tpOh1ly1gl3t5vkz0oj31270g0jy0.jpg)