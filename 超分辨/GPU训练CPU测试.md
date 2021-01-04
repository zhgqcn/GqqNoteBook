# Pytorch中利用GPU训练，CPU测试

要先利用GPU训练，CPU测试，那么在模型**训练**时候，是能**保存模型的参数**而不能保存整个模型，可见**[Pytorch模型保存机制](https://www.cnblogs.com/zgqcn/p/14015720.html)**便可以学会模型的***保存、加载、测试***

💥这里主要讲一点重要的，即在<font color='red'>**`pytorch 1.6`**</font>的版本中训练模型保存时，**不能**直接使用

```python
torch.save(state_r, model_out_r_path)
```

否则，在CPU测试时，由于版本的不兼容会导致出错，**正确使用**如下：

```python
torch.save(state_g, model_out_g_path, _use_new_zipfile_serialization=False)
```

而后，在模型加载时候，要限定`map_location = cpu`的加载：

```python
checkpoint_r = torch.load(opt.model_r,map_location='cpu')
model_rt.load_state_dict(checkpoint_r['model'])
model_rt.eval()
optimizer_rt.load_state_dict(checkpoint_r['optimizer'])
epochs_rt = checkpoint_r['epoch']
```

GPU下训练的模型即可方便的在CPU环境中测试了

![Snipaste_2020-11-21_15-12-23](https://tvax1.sinaimg.cn/large/005tpOh1ly1gkwtcrf273j313y04eabu.jpg)



若模型已经训练保存，但是有没有使用`_use_new_zipfile_serialization=False`来进行约束，那么，可以在**`pytorch 1.6`**中直接加载模型，然后再次使用`torch.save`进行保存为**非zip格式**：

```python
#在torch 1.6版本中重新加载一下网络参数

model = MyModel().cuda() # 先预加载模型

model.load_state_dict(torch.load(pre_model))  #加载模型参数，pre_model为直接没有设置_use_new_zipfile_serialization=False而模型的参数模型

#重新保存网络参数
torch.save(model.state_dict(), pre_model,_use_new_zipfile_serialization=False)
```



另外，网络模型的参数一般较大，在CPU中测试时，可能会导致内存爆满而使得程序无法运行，此时，可以将模型的输入进行修改。

以下，是我将`input`的图像设置为`512*512`，导致了内存的爆满

![Snipaste_2020-11-21_15-21-42](https://tva2.sinaimg.cn/large/005tpOh1ly1gkwtm2ye84j30sf07qwfy.jpg)

之后，我将`input image`裁剪成`128 * 128`进行CPU测试，便可大功告成。



具体的`CPU`测试代码如下，可供参考与理解：

```python
from __future__ import print_function
import argparse
import torch
from PIL import Image
from torchvision.transforms import Compose, ToTensor, CenterCrop
import torchvision
from model import LapSRN_r, LapSRN_g
import torch.optim as optim
# Argument settingsss
parser = argparse.ArgumentParser(description='PyTorch LapSRN')
parser.add_argument('--input', type=str, required=False, default='/content/drive/My Drive/TestImg/val/44-512pix-speed7-ave1.tif', help='input image to use')
parser.add_argument('--model_r', type=str, default='LapSRN_r_epoch_10.pth', help='model file to use')
parser.add_argument('--model_g', type=str, default='LapSRN_g_epoch_10.pth', help='model file to use')
parser.add_argument('--outputHR2', type=str, default='./73_LapSRN_R_epochs100_HR2.tif', help='where to save the output image')
parser.add_argument('--outputHR4', type=str, default='./73_LapSRN_R_epochs100_HR4.tif', help='where to save the output image')
parser.add_argument('--cuda', action='store_true', help='use cuda')
opt = parser.parse_args()
print(opt)

model_rt = LapSRN_r()
model_gt = LapSRN_g()
optimizer_rt = optim.Adagrad(model_rt.parameters(), lr=1e-3, weight_decay=1e-5)
optimizer_gt = optim.Adagrad(model_gt.parameters(), lr=1e-3, weight_decay=1e-5)

checkpoint_r = torch.load(opt.model_r,map_location='cpu')
model_rt.load_state_dict(checkpoint_r['model'])
model_rt.eval()
optimizer_rt.load_state_dict(checkpoint_r['optimizer'])
epochs_rt = checkpoint_r['epoch']

checkpoint_g = torch.load(opt.model_g,map_location='cpu')
model_gt.load_state_dict(checkpoint_g['model'])
model_rt.eval()
optimizer_gt.load_state_dict(checkpoint_g['optimizer'])
epochs_gt = checkpoint_g['epoch']

transform = Compose(
    [
        ToTensor(),
    ])

img = Image.open(opt.input).convert('RGB')
r, g, _ = img.split()

r = transform(r)
r = r.unsqueeze(0)

g = transform(g)
g = g.unsqueeze(0)

HR_2_r, HR_4_r = model_rt(r)
HR_2_g, HR_4_g = model_gt(g)

black_2 = torch.zeros(1, 128 * 2, 128 * 2).unsqueeze(0)
HR_2 = torch.cat((HR_2_r.squeeze(0), HR_2_g.squeeze(0), black_2.squeeze(0))).unsqueeze(0)
black_4 = torch.zeros(1, 128 * 4, 128 * 4).unsqueeze(0)
HR_4 = torch.cat((HR_4_r.squeeze(0), HR_4_g.squeeze(0), black_4.squeeze(0))).unsqueeze(0)
torchvision.utils.save_image(HR_2, opt.outputHR2, padding=0) 
torchvision.utils.save_image(HR_4, opt.outputHR4, padding=0)
print("saved !")
```

