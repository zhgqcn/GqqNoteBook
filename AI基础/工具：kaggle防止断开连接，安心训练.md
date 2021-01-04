# Kaggle：不怕断开连接，睡觉起来看结果

> 无论利用`Colab`训练还是用`kaggle`训练，直接在其`Jupyter Notebook`上训练，在网络不稳定和长时间训练时容易断开连接，导致训练无法顺心如意进行。本想着睡前跑一跑，睡后看结果，但是醒了却满满心塞。
>
> 为了解决这一问题，在`kaggle`上训练时，做好设置便可不怕断开连接，睡觉起来看结果，提高学习效率

# `kaggle`的基础使用

🛑 **了解`kaggle`的基础使用：[免费的深度学习GPU环境Colab和Kaggle搭配使用](https://www.cnblogs.com/zgqcn/p/13475630.html#kaggle)**



# `kaggle`的进阶使用

1️⃣	**通过学习了基础使用后，对按照以下进行配置**

- 打开GPU
- 导入数据集和代码
- 写好训练代码语句并执行，若能成功执行进行下一步，否则要调试通过后再继续
- 点击`Save Version`

![Snipaste_2020-12-19_16-39-54](https://tva2.sinaimg.cn/large/005tpOh1ly1glt9yddey9j31h60u0470.jpg)



2️⃣	**点击`save version`后出现以下弹框**

- version name ：自己命名好记就行，可以[模型名称+版本+时间]，便于自己查看和记录
- 一定要选择`save & Run All(Commit)`模型，否则将导致训练结果不保存
- 点击 save

![Snipaste_2020-12-19_16-40-54](https://tvax1.sinaimg.cn/large/005tpOh1ly1glta221h6ij31hc0u0tfq.jpg)



3️⃣	**save之后便开始训练，可以在左下角的框框中看到部分情况**

- 此时，可以关闭浏览器或者保留在此页面
  - 💥若要留在此页面，记得要**把GPU关闭**，否则也会扣除时长，加上你训练的服务器，等于扣除了2倍
- 可以点击右上角的数字到详情页面进行日志查看

![Snipaste_2020-12-19_16-43-32](https://tvax3.sinaimg.cn/large/005tpOh1ly1glta4jckd3j31hc0u0wna.jpg)



4️⃣	**点击数字后跳转到此页面**

- view logs ：可查看训练的全部细节
- Go to Viewer ：查看训练结果

![Snipaste_2020-12-19_16-44-12](https://tva3.sinaimg.cn/large/005tpOh1ly1glta888qlyj31hc0u07a4.jpg)



5️⃣	**点击`Go to Viewer`即可查看训练的结果啦，之后可以进行下载到本地进行测试**

![Snipaste_2020-12-19_16-45-10](https://tvax2.sinaimg.cn/large/005tpOh1ly1glta9sfnohj31hc0rjwjj.jpg)



**有了这一套操作 ，训练不再怕，安心睡觉等结果。**



> Write by Gqq