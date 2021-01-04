# Pytorch断开后继续训练

> 在训练过程中，往往会遇到中断，如在`Colab`和`Kaggle`中，由于网络不稳定，很容易就断开了连接。然而，即使可以稳定训练，但是训练的时长往往是有上限的，此时我们的网络参数训练的可能还未收敛仍然需要训练，所以，应该加载原训练基础上再进行训练是十分很重要的。
>
> > 比如，要训练1000代才能收敛，但是目前只训练的100代就中断了，所以要加载第100代训练的模型参数，然后训练接下来的900代



## 1️⃣`pytorch`模型的保存机制

👉[模型保存的两种机制](https://www.cnblogs.com/zgqcn/p/14015720.html)

## 2️⃣修改训练代码

中断的训练代码最简单的修改方式便是复制一份训练的代码，然后在其基础上进行修改，涉及到最重要的部分就是**模型的保存与加载**

🅰若优化器`optimizer`不需要随着训练的修改，那么直接加载模型、优化器，之后进行训练即可

🅱若优化器需要训练，那么可以进行一下修改：

```python
 if epoch == epochs_g + 1:
            optimizer_r.load_state_dict(checkpoint_r['optimizer'])
            optimizer_g.load_state_dict(checkpoint_g['optimizer'])
            lr_r = checkpoint_r['lr']
            lr_g = checkpoint_g['lr']    
        else:
            optimizer_r = optim.Adagrad(model_r.parameters(), lr = lr_r, weight_decay = 1e-5)
            optimizer_g = optim.Adagrad(model_g.parameters(), lr = lr_g, weight_decay = 1e-5)
```

- 继续训练的第一次是利用模型保存下来的，而之后则是修改的优化器

如：我的模型**每训练50次**进行`learning rate`减半，初始学习率为`0.001`，而我的模型训练到第40代中断，所以加载第40代模型继续进行训练

```shell
python "train_continue.py" --pre_model_r './LapSRN_r_epoch_40.pt' --pre_model_g './LapSRN_g_epoch_40.pt' --nEpochs 60 --cuda  --batchSize 1 --dataset "../../DataSet_test/"
```

可以看看优化器的变化如下：

```shell
Namespace(batchSize=1, cuda=True, dataset='../../DataSet_test/', lr=0.001, nEpochs=60, pre_model_g='./LapSRN_g_epoch_40.pt', pre_model_r='./LapSRN_r_epoch_40.pt', save_models='./', save_train_csv='./train.csv', save_val_csv='/val.csv', seed=123, valBatchSize=1)
===> Loading datasets
===> Loading pre_train model and Building model
Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.001
    lr_decay: 0
    weight_decay: 1e-05
)
===> Epoch 41 Complete: Avg. Loss: 0.0381
===> Avg. PSNR1: 26.2686 dB
===> Avg. PSNR2: 25.1278 dB
Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.001
    lr_decay: 0
    weight_decay: 1e-05
)
===> Epoch 42 Complete: Avg. Loss: 0.0789
===> Avg. PSNR1: 13.8764 dB
===> Avg. PSNR2: 16.7824 dB

.........省略部分..........

Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.001
    lr_decay: 0
    weight_decay: 1e-05
)
===> Epoch 49 Complete: Avg. Loss: 0.0749
===> Avg. PSNR1: 25.5121 dB
===> Avg. PSNR2: 25.1218 dB
Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.001
    lr_decay: 0
    weight_decay: 1e-05
)
===> Epoch 50 Complete: Avg. Loss: 0.0877
===> Avg. PSNR1: 28.2393 dB
===> Avg. PSNR2: 26.6869 dB
Checkpoint saved to ./LapSRN_r_epoch_50.pt and ./LapSRN_g_epoch_50.pt
Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.0005
    lr_decay: 0
    weight_decay: 1e-05
)
===> Epoch 51 Complete: Avg. Loss: 0.2914
===> Avg. PSNR1: 27.3521 dB
===> Avg. PSNR2: 25.3298 dB
Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.0005
    lr_decay: 0
    weight_decay: 1e-05
)
===> Epoch 52 Complete: Avg. Loss: 0.0505
===> Avg. PSNR1: 21.9110 dB
===> Avg. PSNR2: 21.8041 dB
Adagrad (
Parameter Group 0
    eps: 1e-10
    initial_accumulator_value: 0
    lr: 0.0005
    lr_decay: 0
    weight_decay: 1e-05
)
```



## 3️⃣样例学习

中断训练`train_continue.py`代码如下，可供参考学习：

```python
from __future__ import print_function
import argparse
from math import log10
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader
from model import LapSRN_r, LapSRN_g
from data import get_training_set, get_val_set
from loss import Loss
import pandas as pd
from os.path import join
import os

results = {'Avg. Loss': [], 'Avg. PSNR1': [], 'Avg. PSNR2': []}

# Training settings 
parser = argparse.ArgumentParser(description='PyTorch LapSRN')
parser.add_argument('--batchSize', type=int, default=1, help='training batch size')
parser.add_argument('--valBatchSize', type=int, default=1, help='val batch size')
parser.add_argument('--nEpochs', type=int, default=200, help='number of epochs to train for')
parser.add_argument('--lr', type=float, default=1e-3, help='Learning Rate. Default=1e-3')
parser.add_argument('--cuda', action='store_true', help='use cuda?')
parser.add_argument('--seed', type=int, default=123, help='random seed to use. Default=123')
parser.add_argument('--dataset', type=str, default="/content/drive/My Drive/app/DataSet/", help='root of DataSet')
parser.add_argument('--save_train_csv', type=str, default='./train.csv',help='the root of saving the train results')
parser.add_argument('--save_val_csv', type=str, default='/val.csv', help='the root of saving the val results')
parser.add_argument('--save_models', type=str, default='./', help='the root of saving the models')
parser.add_argument('--pre_model_r', type=str, default='./r.pt', help='continue train model : load r model')
parser.add_argument('--pre_model_g', type=str, default='./g.pt', help='continue train model : load g model')
opt = parser.parse_args()
print(opt)

cuda = opt.cuda
if cuda and not torch.cuda.is_available():
    raise Exception("No GPU found, please run without --cuda")

torch.manual_seed(opt.seed)
if cuda:
    torch.cuda.manual_seed(opt.seed)
device = torch.device("cuda" if opt.cuda else "cpu")

print('===> Loading datasets')
train_set = get_training_set(opt.dataset)
val_set = get_val_set(opt.dataset)
training_data_loader = DataLoader(dataset=train_set, batch_size=opt.batchSize, shuffle=True)
val_data_loader = DataLoader(dataset=val_set, batch_size=opt.valBatchSize, shuffle=False)


print('===> Loading pre_train model and Building model')
model_r = LapSRN_r().to(device)
model_g = LapSRN_g().to(device)
Loss = Loss()
criterion = nn.MSELoss()
if cuda:
    Loss = Loss.cuda()
    criterion = criterion.cuda()


def train(epoch):
        epoch_loss = 0
        for iteration, batch in enumerate(training_data_loader, 1):
            LR_r, LR_g, HR_2_target, HR_4_target = batch[0].to(device), batch[1].to(device), batch[2].to(device), batch[3].to(device)

            optimizer_r.zero_grad()
            optimizer_g.zero_grad()

            HR_2_r, HR_4_r = model_r(LR_r)
            HR_2_g, HR_4_g = model_g(LR_g)

            black_2 = torch.zeros(1, HR_2_target[0].shape[1], HR_2_target[0].shape[2]).unsqueeze(0).to(device)
            HR_2 = torch.cat((HR_2_r.squeeze(0), HR_2_g.squeeze(0), black_2.squeeze(0))).unsqueeze(0)

            black_4 = torch.zeros(1, HR_4_target[0].shape[1], HR_4_target[0].shape[2]).unsqueeze(0).to(device)
            HR_4 = torch.cat((HR_4_r.squeeze(0), HR_4_g.squeeze(0), black_4.squeeze(0))).unsqueeze(0)

            loss1 = Loss(HR_2, HR_2_target)
            loss2 = Loss(HR_4, HR_4_target)
            loss = loss1 + loss2

            epoch_loss += loss.item()
            loss.backward()
            optimizer_r.step()
            optimizer_g.step()

        print("===> Epoch {} Complete: Avg. Loss: {:.4f}".format(epoch, epoch_loss / len(training_data_loader)))
        results['Avg. Loss'].append(float('%.4f'%(epoch_loss / len(training_data_loader))))


def val():
    avg_psnr1 = 0
    avg_psnr2 = 0
    with torch.no_grad():
        for batch in val_data_loader:
            LR_r, LR_g, HR_2_target, HR_4_target = batch[0].to(device), batch[1].to(device), batch[2].to(device), batch[3].to(device)
            HR_2_r, HR_4_r = model_r(LR_r)
            HR_2_g, HR_4_g = model_r(LR_g)

            black_2 = torch.zeros(1, HR_2_target[0].shape[1], HR_2_target[0].shape[2]).unsqueeze(0).to(device)
            HR_2 = torch.cat((HR_2_r.squeeze(0), HR_2_g.squeeze(0), black_2.squeeze(0))).unsqueeze(0)

            black_4 = torch.zeros(1, HR_4_target[0].shape[1], HR_4_target[0].shape[2]).unsqueeze(0).to(device)
            HR_4 = torch.cat((HR_4_r.squeeze(0), HR_4_g.squeeze(0), black_4.squeeze(0))).unsqueeze(0)

            mse1 = criterion(HR_2, HR_2_target)
            mse2 = criterion(HR_4, HR_4_target)
            psnr1 = 10 * log10(1 / mse1.item())
            psnr2 = 10 * log10(1 / mse2.item())
            avg_psnr1 += psnr1
            avg_psnr2 += psnr2
        print("===> Avg. PSNR1: {:.4f} dB".format(avg_psnr1 / len(val_data_loader)))
        print("===> Avg. PSNR2: {:.4f} dB".format(avg_psnr2 / len(val_data_loader)))
        results['Avg. PSNR1'].append(float('%.2f'%(avg_psnr1 / len(val_data_loader))))
        results['Avg. PSNR2'].append(float('%.2f'%(avg_psnr2 / len(val_data_loader))))


def checkpoint(epoch):
    model_out_r_path = join(opt.save_models, 'LapSRN_r_epoch_{}.pt'.format(epoch))
    model_out_g_path = join(opt.save_models, 'LapSRN_g_epoch_{}.pt'.format(epoch))
    state_r = {'model': model_r.state_dict(), 'optimizer': optimizer_r.state_dict(), 'epoch':epoch, 'lr':lr_r}
    state_g = {'model': model_g.state_dict(), 'optimizer': optimizer_g.state_dict(), 'epoch':epoch, 'lr':lr_g}
    torch.save(state_r, model_out_r_path, _use_new_zipfile_serialization=False)
    torch.save(state_g, model_out_g_path, _use_new_zipfile_serialization=False)
    print("Checkpoint saved to {} and {}".format(model_out_r_path, model_out_g_path))



if os.path.exists(opt.pre_model_r) and os.path.exists(opt.pre_model_g):
    model_r = LapSRN_r().cuda()
    model_g = LapSRN_g().cuda()

    checkpoint_r = torch.load(opt.pre_model_r)
    model_r.load_state_dict(checkpoint_r['model'])
    model_r.train()
    epochs_r = checkpoint_r['epoch']
    

    checkpoint_g = torch.load(opt.pre_model_g)
    model_g.load_state_dict(checkpoint_g['model'])
    model_g.train()
    epochs_g = checkpoint_g['epoch']
    
    optimizer_r = optim.Adagrad(model_r.parameters())
    optimizer_g = optim.Adagrad(model_g.parameters())

    for epoch in range(epochs_g + 1, opt.nEpochs + 1):

        if epoch == epochs_g + 1:
            optimizer_r.load_state_dict(checkpoint_r['optimizer'])
            optimizer_g.load_state_dict(checkpoint_g['optimizer'])
            lr_r = checkpoint_r['lr']
            lr_g = checkpoint_g['lr']    
        else:
            optimizer_r = optim.Adagrad(model_r.parameters(), lr = lr_r, weight_decay = 1e-5)
            optimizer_g = optim.Adagrad(model_g.parameters(), lr = lr_g, weight_decay = 1e-5)
            
            
        train(epoch)
        val()

        if epoch % 10 ==0:
            checkpoint(epoch)
        if epoch % 50 ==0:
            lr_r = lr_r / 2
            lr_g = lr_g / 2

        data_frame = pd.DataFrame(
        data={ 'Avg. Loss': results['Avg. Loss'],
                'Avg. PSNR1': results['Avg. PSNR1'],
                'Avg. PSNR2': results['Avg. PSNR2']},
        index=range(epochs_g + 1, epoch + 1)
        )
        data_frame.to_csv('./result-z.csv', index_label='Epoch')
```

