---
# Colab 实用教程

---

[TOC]

# Colab是什么？

1. Google Colab 是一个免费的云服务并支持免费的 GPU，可以：

- 提高你的 **Python** 语言的编码技能。
- 使用 **Keras**、**TensorFlow**、**PyTorch** 和 **OpenCV** 等流行库开发深度学习应用程序。
- Colab 与其它免费的云服务最重要的区别在于：**Colab** 提供完全免费的 GPU，**对学生党进行AI学习提供便利**。

2. Colab 是Google的， 所以，你的电脑必须能够登陆[goole.com](http://www.google.com/)啊，所以**'学会上网'**很重要。先确定可以上谷歌后，再继续往下看吧！



# Colab的基本配置

1. **登录 [Google Drive](https://drive.google.com/)**
2. **在 Google Drive 上创建文件夹，我创建的是名字为 app 的文件夹**

   ![s](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714224844251-1534150405.png)

3. **创建新的 Colab 笔记（Notebook），通过 右键点击 > More > Colaboratory 步骤创建一个新的笔记**

   ![ss](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714225153623-189155043.png)

   **通过点击文件名来重命名笔记**

![ss0](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714225240840-1044399482.png)

4. **打开 GPU**

   **Edit > Notebook settings** 或者进入 **Runtime > Change runtime type**，然后选择 **GPU** 作为 **Hardware accelerator（硬件加速器）**。

![sss](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714225413657-1785459503.png)

5. **使用 Google Colab 运行基本的 Python 代码**

   这个倒是不常用，使用这个功能类似jupyter notebook，而我们要跑的代码基本是已经编辑好的工程项目。利用colab主要是想通过**GPU加速**更快的训练。

   ![sbs](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714225610556-1791273060.png)

6. **在创建的文件夹页面上传你的整个要跑的文件(包括数据集)，右击选upload fold 或者直接拖拉也行**

![Snipaste_2020-12-19_17-42-19](https://tvax4.sinaimg.cn/large/005tpOh1ly1gltb0w7jk6j31hb0ht41l.jpg)

# Colab 模型训练

1. 加载盘

   ```python
   from google.colab import drive
   drive.mount('/content/drive/')
   ```

   ![fs](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714230932384-1359631752.png)

2. 切换到你要跑的目录下面

   ```python
   # 指定当前的工作文件夹
   import os
   # 此处为google drive中的文件路径,drive为之前指定的工作根目录，要加上
   os.chdir("/content/drive/MyDrive/LapSRN/") 
   ```

3. **安装Pytorch以及torchvision** 

   Colab 一般情况下已经自带了pytorch环境了。若没有可以进行相应的安装：

   ```python
   !pip install torch torchvision  # 在Colab中执行操作语句时，感叹号不能漏
   ```

4. **执行训练命令**

   ![fs](https://img2018.cnblogs.com/blog/1538832/201907/1538832-20190714231518478-1065278260.png)

5. 注意事项

   - 最重要的是**路径问题**，一般在`data.py`或者`dateset.py`文件里面有关于路径的，还有save model时候。可以将路径相关的都改成`parse`的语句，在执行命令时传入防止出错。相关的路径可以直接复制

   ![Snipaste_2020-12-19_17-54-36](https://tva1.sinaimg.cn/large/005tpOh1ly1gltbdqo7ynj31hc0qhgry.jpg)

# 进阶使用

**搭配Kaggle使用，体验更是一层楼**
**🅰[免费的深度学习GPU环境Colab和Kaggle搭配使用](https://www.cnblogs.com/zgqcn/p/13475630.html)**
**🅱[Kaggle：不怕断开连接，睡觉起来看结果](https://www.cnblogs.com/zgqcn/p/14160093.html)**