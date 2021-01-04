[TOC]

>深度学习越加火热，但是，很多实验室并没有配套的硬件设备，让贫穷的学生党头大😔
>经过网上大量的搜罗，我整理了适合学生党的深度学习解决方案。利用**Colab + Kaggle**两大免费的GPU环境，让深度学习变得简单。

# Colab
## Colab基础使用
Google Colab提供了免费K80的GPU，通过Google Drive就可以很好的白嫖一波了 👉[Colab 实用教程](https://www.cnblogs.com/zgqcn/p/11186406.html)
## Colab进阶使用
通过下载[**Google备份与同步**](https://www.google.com/drive/download/)到本地端，就可以实现数据的同步，保证了较大量的数据集在云端和本地端直接的无差错传输

<center><img src="https://tvax4.sinaimg.cn/large/005tpOh1gy1ghmunxq0opj30dk0jnjta.jpg" alt="Snipaste_2020-08-11_14-21-27" style="zoom:80%;" /></center>

通过这一同步，在本地端修改代码，可以在1分钟之内就同步到云端，方便与训练与修改。

❌但是，K80 GPU的算力较弱，且colab的连接不稳定，**适用于对代码进行调试，不适于长时间的训练。**



# Kaggle

Kaggle提供免费访问内核中的Nvidia Tesla P100，相比于K80，算力提升了太多了。我在colab训练10小时最好80epochs，在kaggle上，训练不到1小时就可以80epochs，真是太感动了😂，配置如下：

## 创建账号

[Kaggle.com](https://www.kaggle.com/)

## 创建NoteBook

![Snipaste_2020-08-11_14-35-13](https://tva3.sinaimg.cn/large/005tpOh1gy1ghmv25udfsj31hc0im410.jpg)

![Snipaste_2020-08-11_14-36-58](https://tvax3.sinaimg.cn/large/005tpOh1gy1ghmv3ysfiqj31h50p3mzv.jpg)

**建议**：创建时可以不选GPU，直接None就行了，否则，创建后就开始计时了，GPU一周可以使用39小时（当然可以多多创建账号）

## 配置notebook

![Snipaste_2020-08-11_14-41-30](https://tva2.sinaimg.cn/large/005tpOh1gy1ghmv8px62tj31hc0j7439.jpg)

1. 上传代码及数据集要求压缩包上传，上传后Add进去就好了

<center><img src="https://tvax3.sinaimg.cn/large/005tpOh1gy1ghmvag993mj30mw0g5q3w.jpg" alt="Snipaste_2020-08-11_14-43-09" style="zoom:70%;" /></center>

2. 写控制命令时，只需要复制路径就可以获得train.py的位置

<center><img src="https://tva1.sinaimg.cn/large/005tpOh1gy1ghmvd09d8pj309807omxe.jpg" alt="Snipaste_2020-08-11_14-45-36" style="zoom:80%;" /></center>

3. 训练结果保存的位置应该为`/kaggle/working/...`

<center><img src="https://tvax4.sinaimg.cn/large/005tpOh1gy1ghmvervhyaj30mq03h3yv.jpg" alt="Snipaste_2020-08-11_14-47-20" style="zoom:80%;" /></center>

4. 数据集与代码管理

<center><img src="https://tvax4.sinaimg.cn/large/005tpOh1gy1ghmvk4uhq9j31h00o0dk4.jpg" alt="Snipaste_2020-08-11_14-52-21" style="zoom:70%;" /></center>

# Kaggle进阶使用
**[Kaggle：不怕断开连接，睡觉起来看结果](https://www.cnblogs.com/zgqcn/p/14160093.html)**

通过这两大平台的结合使用，学生党可以方便的进行深度学习了，thanks for Google.

> **write by Gqq**