# 图像分块大小对于训练的影响

在进行训练时，将图像原原本本地作为input进行训练是难以实现的，所以往往有以下两种处理方案：

1. [Centercrop](https://pytorch.org/docs/stable/torchvision/transforms.html#torchvision.transforms.CenterCrop)
2. <font color='red'>分块</font>



## 分块处理

对于分块，我们可以进行**不同的等分**，以下对`512 * 512 pixel`的图像及其所对应的`target`进行两种尝试：

- `4 * 4` 分块，即生成的input是`128 * 128 pixel`的
- `8 * 8` 分块，即生成的input是`64 * 64 pixel`的

![Snipaste_2020-11-17_23-25-57](https://tvax2.sinaimg.cn/large/005tpOh1ly1gksl4juw46j30rc0itadw.jpg)

![Snipaste_2020-11-17_23-24-40](https://tvax3.sinaimg.cn/large/005tpOh1ly1gksl39zd40j30w60nbgs2.jpg)



## 训练结果比较



![QQ图片20201117232751](https://tva2.sinaimg.cn/large/005tpOh1ly1gksl6pwym7j317q0gomyy.jpg)



可以看出，通过`128*128`进行分块的数据集可以在网络的训练上表现出更好地效果，因为更大的输入使得网络能有<font color='red'>更大的感受野</font>，有益于网络参数的训练。

所以，在算力允许的情况下，应该尽量的用大的图像进行输入训练。