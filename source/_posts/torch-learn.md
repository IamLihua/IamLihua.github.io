---
title: torch笔记
date: 2023-07-12 16:57:24
tags: 
  - torch
categories: 
  - learn
excerpt: 这个暑假学习了一下深度学习，过了一下常见的流程，于是把教程上代码都记了一下，方便以后回顾
---

# torch笔记

## 参考视频

www.bilibili.com/video/BV1hE411t7RN

## 基本类的使用

### Dataset

作用：数据导入

模块位置

```python
from torch.utils.data import Dataset
```

代码

```python
'''
Dataset需要重写下面这三个函数
__init__:写初始化的准备工作，当然可以空着
__getitem__:如何获取指定元素
__len__:数据集有多大
'''
from torch.utils.data import Dataset
from PIL import Image # python image lib, py自带的库
import os

class MyData(Dataset):
    def __init__(self, root_dir, image_dir, label_dir): # 参数的个数和名字随意，只要有__init__这个函数就行
        self.root_dir = root_dir
        self.image_dir = image_dir
        self.label_dir = label_dir
        self.label_path = os.path.join(self.root_dir, self.label_dir)
        self.image_path = os.path.join(self.root_dir, self.image_dir)
        self.image_list = os.listdir(self.image_path)
        self.label_list = os.listdir(self.label_path)
        # 因为label 和 Image文件名相同，进行一样的排序，可以保证取出的数据和label是一一对应的
        self.image_list.sort()
        self.label_list.sort()

    def __getitem__(self, idx): # idx是序号
        img_name = self.image_list[idx]
        label_name = self.label_list[idx]
        img_item_path = os.path.join(self.root_dir, self.image_dir, img_name)
        label_item_path = os.path.join(self.root_dir, self.label_dir, label_name)
        img = Image.open(img_item_path)

        with open(label_item_path, 'r') as f:
            label = f.readline()

        # img = np.array(img)
        sample = {'img': img, 'label': label}
        return sample

    def __len__(self):
        assert len(self.image_list) == len(self.label_list)
        return len(self.image_list)

if __name__ == '__main__':
    root_dir = "F:\\test\\pytorch-tutorial-master\\dataset\\train"
    image_ants = "ants_image"
    label_ants = "ants_label"
    ants_dataset = MyData(root_dir, image_ants, label_ants)
    ants_dataset[0]['img'].show() # 获得第一张图片
```

### tensorboard.SummaryWriter

日志记录，可以记录标量或者图片的变化

代码

```python
from torch.utils.tensorboard import SummaryWriter
import numpy as np
from PIL import Image

writer = SummaryWriter("logs")
image_path = "F:\\test\\pytorch-tutorial-master\\dataset\\train\\ants_image\\6240329_72c01e663e.jpg"
img_PIL = Image.open(image_path)
img_array = np.array(img_PIL)
print(type(img_array))
print(img_array.shape)

writer.add_image("train", img_array, 1, dataformats='HWC') # dataformats这个参数用来指明数组中通道的意义(长宽高顺序),1是第一张,后面还可以加第2张第3张等
# 如果是加入图片数组的话是使用add_images()方法 显示时是一堆图片拼接在一起
# y = 2x
for i in range(100):
    writer.add_scalar("y=2x", 3*i, i) # 加入标量

writer.close()  # 别忘了关
```

查看

```sh
conda activate torch
tensorboard --logdir=E:\pprogram\test\logs # 这个是代码中指定的路径
```

### transforms

图片处理类

导入

```python
from torchvision import transforms
```

代码

```python
from torch.utils.data import Dataset, DataLoader
from PIL import Image
import os
from torchvision import transforms

class MyData(Dataset):

    def __init__(self, root_dir, image_dir, label_dir, transform=None):
        self.root_dir = root_dir
        self.image_dir = image_dir
        self.label_dir = label_dir
        self.label_path = os.path.join(self.root_dir, self.label_dir)
        self.image_path = os.path.join(self.root_dir, self.image_dir)
        self.image_list = os.listdir(self.image_path)
        self.label_list = os.listdir(self.label_path)
        self.transform = transform
        # 因为label 和 Image文件名相同，进行一样的排序，可以保证取出的数据和label是一一对应的
        self.image_list.sort()
        self.label_list.sort()

    def __getitem__(self, idx):
        img_name = self.image_list[idx]
        label_name = self.label_list[idx]
        img_item_path = os.path.join(self.root_dir, self.image_dir, img_name)
        label_item_path = os.path.join(self.root_dir, self.label_dir, label_name)
        img = Image.open(img_item_path)
        with open(label_item_path, 'r') as f:
            label = f.readline()

        if self.transform:
            img = transform(img)
            '''
            python里面有一个魔法函数叫__call__()
            在实现它后 类的实例可以发挥出类似函数的功能
            e.g.
            p=new Person("lihua") # 这里调用构造函数__init__(str)
            age=p(18) # 这里调用魔法函数__call__(int)
            '''

        return img, label

    def __len__(self):
        assert len(self.image_list) == len(self.label_list)
        return len(self.image_list)

transform = transforms.Compose([transforms.Resize(400), transforms.ToTensor()])
root_dir = "dataset/train"
image_ants = "ants_image"
label_ants = "ants_label"
ants_dataset = MyData(root_dir, image_ants, label_ants, transform=transform)
image_bees = "bees_image"
label_bees = "bees_label"
bees_dataset = MyData(root_dir, image_bees, label_bees, transform=transform)
```

`transform = transforms.ToTensor()`使所有数据转换为`Tensor`，如果不进行转换则返回的是PIL图片。`transforms.ToTensor()`将尺寸为 (H x W x C) 且数据位于[0, 255]的PIL图片或者数据类型为`np.uint8`的NumPy数组转换为尺寸为(C x H x W)且数据类型为`torch.float32`且位于[0.0, 1.0]的`Tensor`。

### dataloader

数据分组

```python
import torchvision

# 准备的测试数据集
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

test_data = torchvision.datasets.CIFAR10("./dataset", train=False, transform=torchvision.transforms.ToTensor())

'''
batch_size:一组有多少的数据
shuffle:是否打乱
num_workers:多线程装载,windows下最好写0
drop_last:是否丢弃最后一组不满一组的
它的装载方式是这样的:
例如如果dataset中的每一项是x,y;batch_size=4
那test_loader中的每一项是(Xs,Ys),其中Xs和Ys都是4元组
'''
test_loader = DataLoader(dataset=test_data, batch_size=64, shuffle=True, num_workers=0, drop_last=True)

# 测试数据集中第一张图片及target
img, target = test_data[0]
print(img.shape)
print(target)

writer = SummaryWriter("dataloader")
for epoch in range(2):
    step = 0
    for data in test_loader:
        # 由于shuffle=True,所以两次for循环中test_loader返回的东西不一样
        imgs, targets = data
        # print(imgs.shape)
        # print(targets)
        writer.add_images("Epoch: {}".format(epoch), imgs, step)
        step = step + 1

writer.close()
```

### 一些注意

需要多看看官方文档，一方面要看看输入输出的数据类型是什么，另一方面不清楚返回值的话要试试`print(type(x))`或者关注一下`print(x.shape)`的值

## 使用torch自带的数据集

```python
import torchvision
from torch.utils.tensorboard import SummaryWriter

dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

train_set = torchvision.datasets.CIFAR10(root="./dataset", train=True, transform=dataset_transform, download=True) # 用CIFAR10数据集,download=True代表要从网上在线下载,如果已经有的话,就不会再下载了
test_set = torchvision.datasets.CIFAR10(root="./dataset", train=False, transform=dataset_transform, download=True)

# print(test_set[0])
# print(test_set.classes)
#
# img, target = test_set[0]
# print(img)
# print(target)
# print(test_set.classes[target])
# img.show()
#
# print(test_set[0])

writer = SummaryWriter("p10")
for i in range(10):
    img, target = test_set[i]
    writer.add_image("test_set", img, i)

writer.close()
```

## 网络

### nn.module

torch.nn.Module is Base class for all neural network modules.

```python
import torch
from torch import nn

class Tudui(nn.Module):
    def __init__(self):
        super().__init__()

    def forward(self, input):
        # 前趋,需要实现
        output = input + 1
        return output

tudui = Tudui()
x = torch.tensor(1.0)
output = tudui(x) # 感觉这里有点像魔法函数__call__
print(output)
```

### 卷积

F.conv2d

导入

```python
import torch.nn.functional as F
```

代码

```python
'''
torch.nn.functional.conv2d(input, weight, bias=None, stride=1, padding=0, dilation=1, groups=1) → Tensor
input:(minibatch , in_channels , iH , iW) 最小批量 通道数 高 宽
weight:({out_channels} , \frac{\text{in\_channels}}{\text{groups}} , kH , kW) 卷积核
bias: 偏置量 默认没有
stride:每步长度 默认为1
padding:外侧空白填充的行或者列数 默认为0
'''
import torch
import torch.nn.functional as F

input = torch.tensor([[1, 2, 0, 3, 1],
                      [0, 1, 2, 3, 1],
                      [1, 2, 1, 0, 0],
                      [5, 2, 3, 1, 1],
                      [2, 1, 0, 1, 1]])

kernel = torch.tensor([[1, 2, 1],
                       [0, 1, 0],
                       [2, 1, 0]])

input = torch.reshape(input, (1, 1, 5, 5))
kernel = torch.reshape(kernel, (1, 1, 3, 3))

print(input.shape)
print(kernel.shape)

output = F.conv2d(input, kernel, stride=1)
print(output)

output2 = F.conv2d(input, kernel, stride=2)
print(output2)

output3 = F.conv2d(input, kernel, stride=1, padding=1)
print(output3)
```

### 卷积层

Conv2d

用于选取特征

```python
from torch.nn import Conv2d
```

代码

```python
import torch
import torchvision
from torch import nn
from torch.nn import Conv2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

dataset = torchvision.datasets.CIFAR10("dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                       download=True)
dataloader = DataLoader(dataset, batch_size=64)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        # 和之前的代码不同，这里的只需要指定输入输出通道数，卷积核大小，卷积核内的权重是会自己随机初始化的
        self.conv1 = Conv2d(in_channels=3, out_channels=6, kernel_size=3, stride=1, padding=0)

    def forward(self, x):
        x = self.conv1(x)
        return x

tudui = Tudui()

writer = SummaryWriter("./logs")

step = 0
for data in dataloader:
    imgs, targets = data
    output = tudui(imgs)
    print(imgs.shape)
    print(output.shape)
    # torch.Size([64, 3, 32, 32])
    writer.add_images("input", imgs, step)
    # torch.Size([64, 6, 30, 30])  -> [xxx, 3, 30, 30]

    output = torch.reshape(output, (-1, 3, 30, 30))  # 6通道无法正常显示 所以reshape
    writer.add_images("output", output, step)

    step = step + 1

writer.close()
```

### 池化层

用于减少数据量

```python
from torch.nn import MaxPool2d
```

代码

```python
import torchvision
from torch import nn
from torch.nn import MaxPool2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

dataset = torchvision.datasets.CIFAR10("dataset", train=False, download=True,
                                       transform=torchvision.transforms.ToTensor())

dataloader = DataLoader(dataset, batch_size=64)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.maxpool1 = MaxPool2d(kernel_size=3, ceil_mode=False)  # 多出来的部分的处理方式 为False为不保留

    def forward(self, input):
        output = self.maxpool1(input)
        return output

tudui = Tudui()

writer = SummaryWriter("logs_maxpool")
step = 0

for data in dataloader:
    imgs, targets = data
    writer.add_images("input", imgs, step)
    output = tudui(imgs)
    writer.add_images("output", output, step)
    step = step + 1

writer.close()
```

### 非线性激活

拟合特征

```python
import torch
import torchvision
from torch import nn
from torch.nn import ReLU, Sigmoid
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

input = torch.tensor([[1, -0.5],
                      [-1, 3]])

input = torch.reshape(input, (-1, 1, 2, 2))
print(input.shape)

dataset = torchvision.datasets.CIFAR10("dataset", train=False, download=True,
                                       transform=torchvision.transforms.ToTensor())

dataloader = DataLoader(dataset, batch_size=64)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.relu1 = ReLU()
        self.sigmoid1 = Sigmoid()

    def forward(self, input):
        output = self.sigmoid1(input)
        return output

tudui = Tudui()

writer = SummaryWriter("logs_relu")
step = 0
for data in dataloader:
    imgs, targets = data
    writer.add_images("input", imgs, global_step=step)
    output = tudui(imgs)
    writer.add_images("output", output, step)
    step += 1

writer.close()
```

### 线性层

![img](https://img-blog.csdnimg.cn/b7bc507721f3420f9ae5202676331b80.png)

图中每个箭头都是一个线性计算 $$y=ax+b$$ 具体是矩阵乘法

代码

```python
import torch
import torchvision
from torch import nn
from torch.nn import Linear
from torch.utils.data import DataLoader

dataset = torchvision.datasets.CIFAR10("dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                       download=True)

dataloader = DataLoader(dataset, batch_size=64, drop_last=True)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.linear1 = Linear(196608, 10)  # 将196608维输入变为10层的输出

    def forward(self, input):
        output = self.linear1(input)
        return output

tudui = Tudui()

for data in dataloader:
    imgs, targets = data
    print(imgs.shape)
    output = torch.flatten(imgs)  # torch.flatten可以将Tenor展开成一行(1,1,1,...,n)这种形式
    print(output.shape)
    output = tudui(output)
    print(output.shape)
```

### seq

用于简化表达

```python
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.tensorboard import SummaryWriter


class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model1 = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x

tudui = Tudui()
print(tudui)
input = torch.ones((64, 3, 32, 32))  # 用来检验网络参数是否正确
output = tudui(input)
print(output.shape)

writer = SummaryWriter("logs_seq")
writer.add_graph(tudui, input)  # 画流程图
writer.close()

```

`net[0]`这样根据下标访问子模块的写法只有当`net`是个`ModuleList`或者`Sequential`实例时才可以

### 损失

#### loss

```python
import torch
from torch.nn import L1Loss
from torch import nn

inputs = torch.tensor([1, 2, 3], dtype=torch.float32)
targets = torch.tensor([1, 2, 5], dtype=torch.float32)

inputs = torch.reshape(inputs, (1, 1, 1, 3))
targets = torch.reshape(targets, (1, 1, 1, 3))

loss = L1Loss(reduction='sum')  # 作差然后绝对值
result = loss(inputs, targets)

loss_mse = nn.MSELoss()  # 平方差
result_mse = loss_mse(inputs, targets)

print(result)
print(result_mse)


x = torch.tensor([0.1, 0.2, 0.3])
y = torch.tensor([1])
x = torch.reshape(x, (1, 3))
loss_cross = nn.CrossEntropyLoss()
result_cross = loss_cross(x, y)
print(result_cross)
```

#### backward

```python
import torchvision
from torch import nn
from torch.nn import Sequential, Conv2d, MaxPool2d, Flatten, Linear
from torch.utils.data import DataLoader

dataset = torchvision.datasets.CIFAR10("dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                       download=True)

dataloader = DataLoader(dataset, batch_size=1)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model1 = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x


loss = nn.CrossEntropyLoss()
tudui = Tudui()
for data in dataloader:
    imgs, targets = data
    outputs = tudui(imgs)
    result_loss = loss(outputs, targets)
    result_loss.backward()  # 和forward对应
    print("ok")
```

### 优化

```python
import torch
import torchvision
from torch import nn
from torch.nn import Sequential, Conv2d, MaxPool2d, Flatten, Linear
from torch.utils.data import DataLoader

dataset = torchvision.datasets.CIFAR10("dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                       download=True)

dataloader = DataLoader(dataset, batch_size=1)

class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model1 = Sequential(
            Conv2d(3, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 32, 5, padding=2),
            MaxPool2d(2),
            Conv2d(32, 64, 5, padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024, 64),
            Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x


loss = nn.CrossEntropyLoss()
tudui = Tudui()
optim = torch.optim.SGD(tudui.parameters(), lr=0.01)  # 用指定的网络初始化它的参数
for epoch in range(20):
    # 训练20次
    running_loss = 0.0
    for data in dataloader:
        imgs, targets = data
        outputs = tudui(imgs)  # 前向计算
        result_loss = loss(outputs, targets)  # 损失计算
        optim.zero_grad()  # 梯度置0
        result_loss.backward()  # 反向传播
        optim.step()  # 优化
        running_loss = running_loss + result_loss
    print(running_loss)  # 看每次的loss总和

```

### 打印参数及初始化参数

```python
import torch
from torch import nn
from torch.nn import init

net = nn.Sequential(nn.Linear(4, 3), nn.ReLU(), nn.Linear(3, 1))  # pytorch已进行默认初始化
print(net)
print(type(net.named_parameters()))
for name, param in net.named_parameters():
    # 返回的名字自动加上了层数的索引作为前缀。 
    print(name, param.size())
# 我们再来访问net中单层的参数。对于使用Sequential类构造的神经网络，我们可以通过方括号[]来访问网络的任一层。索引0表示隐藏层为Sequential实例最先添加的层。
for name, param in net[0].named_parameters():
    print(name, param.size(), type(param))

# 将权重参数初始化成均值为0、标准差为0.01的正态分布随机数，并依然将偏差参数清零
for name, param in net.named_parameters():
    if 'weight' in name:
        init.normal_(param, mean=0, std=0.01)
        print(name, param.data)
        
# 使用常数来初始化权重参数
for name, param in net.named_parameters():
    if 'bias' in name:
        init.constant_(param, val=0)
        print(name, param.data)
```



## 使用预下载好的模型

```python
import torchvision

# train_data = torchvision.datasets.ImageNet("data_image_net", split='train', download=True,
#                                            transform=torchvision.transforms.ToTensor())  # 这个数据集太大了
from torch import nn

vgg16_false = torchvision.models.vgg16(pretrained=False)  # 只是初始化 没有训练好参数
vgg16_true = torchvision.models.vgg16(pretrained=True) # 已经预训练好参数

print(vgg16_true)

train_data = torchvision.datasets.CIFAR10('dataset', train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)  # 训练集

# 由于vgg16的最后一层会输出1000维 也就是1000类的分类 有时候我们只需要10分类 所以可以修改为10维的输出
vgg16_true.classifier.add_module('add_linear', nn.Linear(1000, 10))  # 在网络中的classifier层的最后加一层
print(vgg16_true)

print(vgg16_false)
vgg16_false.classifier[-1] = nn.Linear(4096, 10)  # 修改网络中的classifier层的最后层
print(vgg16_false)
```

## 测试网络参数是否正确

使用`torch.zeros`

```python
from torch import nn
import torch.nn.functional as F
import torch


class Net(nn.Module):
    # 自定义网络
    def __init__(self):
        # 调用Net的父类的构造函数，初始化神经网络对象
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = nn.Conv2d(10, 20, kernel_size=5)
        # 对卷积层的输出进行dropout操作，随机丢弃一些神经元，防止过拟合
        self.conv2_drop = nn.Dropout2d()
        self.fc1 = nn.Linear(320, 50)
        self.fc2 = nn.Linear(50, 10)

    # 定义了给定的层之间的连接方式
    def forward(self, x):
        # 卷积，池化核大小为2的最大池化操作，对池化结果进行激活操作，池化核大小是一个超参数，常取2/3
        x = F.relu(F.max_pool2d(self.conv1(x), 2))
        # x = F.relu(F.max_pool2d(self.conv2_drop(self.conv2(x)), 2))
        x = F.relu(F.max_pool2d(self.conv2(x), 2))
        # 重塑张量x，-1表示根据其他维度推断（行数），320列数
        x = x.view(-1, 320)
        x = F.relu(self.fc1(x))
        # dropout函数用于全连接层，Dropout2d用于卷积层
        # x = F.dropout(x, training=self.training)
        x = self.fc2(x)
        return F.log_softmax(x, dim=-1)


net = Net()
print(net)
# 这是1张图,但直接用会
# RuntimeError: Expected 3D (unbatched) or 4D (batched) input to conv2d, but got input of size: [28, 28]
test_in_1 = torch.zeros([28, 28], dtype=torch.float32)  # 需要用float32,否则报错
test_in_2 = torch.zeros([28, 28], dtype=torch.float32)  # 需要用float32,否则报错
# test_in_2 = torch.zeros([2, 1, 28, 28], dtype=torch.float32)  # 也可以直接这样
test_in_1 = test_in_1.view(-1, 1, 28, 28)  # 要加等号,这里的conv1需要单通道的输入
test_in_2 = torch.reshape(test_in_2, (-1, 1, 28, 28))  # 要加等号,这里的conv1需要单通道的输入
test_out_1 = net(test_in_1)
test_out_2 = net(test_in_2)
print(test_out_1)
print(test_out_2)
```



## 保存和使用已有模型的参数

保存参数 两种方法

```python
import torch
import torchvision
from torch import nn

vgg16 = torchvision.models.vgg16(pretrained=False)
# 保存方式1,模型结构+模型参数
torch.save(vgg16, "vgg16_method1.pth")

# 保存方式2，模型参数（官方推荐）
torch.save(vgg16.state_dict(), "vgg16_method2.pth")

# 陷阱
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.conv1 = nn.Conv2d(3, 64, kernel_size=3)

    def forward(self, x):
        x = self.conv1(x)
        return x

tudui = Tudui()
torch.save(tudui, "tudui_method1.pth")
```

读取参数 与之对应的两种方法，这两种方法就相当于加载好了实例了

```python
import torch
# from model_save import *
# 方式1-》保存方式1，加载模型
import torchvision
from torch import nn

model = torch.load("vgg16_method1.pth")
# print(model)

# 方式2，加载模型
vgg16 = torchvision.models.vgg16(pretrained=False)
vgg16.load_state_dict(torch.load("vgg16_method2.pth"))
# model = torch.load("vgg16_method2.pth")
# print(vgg16)

# 陷阱1 需要导入其中所有的包
# class Tudui(nn.Module):
#     def __init__(self):
#         super(Tudui, self).__init__()
#         self.conv1 = nn.Conv2d(3, 64, kernel_size=3)
#
#     def forward(self, x):
#         x = self.conv1(x)
#         return x

model = torch.load('tudui_method1.pth')
print(model)
```

## 训练过程

```python
import torchvision
from torch.utils.tensorboard import SummaryWriter

from model import *
# 准备数据集
from torch import nn
from torch.utils.data import DataLoader

train_data = torchvision.datasets.CIFAR10(root="../data", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10(root="../data", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)

# length 长度
train_data_size = len(train_data)
test_data_size = len(test_data)
# 如果train_data_size=10, 训练数据集的长度为：10
print("训练数据集的长度为：{}".format(train_data_size))
print("测试数据集的长度为：{}".format(test_data_size))


# 利用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 创建网络模型
tudui = Tudui()

# 损失函数
loss_fn = nn.CrossEntropyLoss()

# 优化器
# learning_rate = 0.01
# 1e-2=1 x (10)^(-2) = 1 /100 = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(tudui.parameters(), lr=learning_rate)

# 设置训练网络的一些参数
# 记录训练的次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

# 添加tensorboard
writer = SummaryWriter("../logs_train")

for i in range(epoch):
    print("-------第 {} 轮训练开始-------".format(i+1))

    # 训练步骤开始
    tudui.train()  # 这步在一般情况下可以没有 训练状态
    for data in train_dataloader:
        imgs, targets = data
        outputs = tudui(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:
            print("训练次数：{}, Loss: {}".format(total_train_step, loss.item()))
            writer.add_scalar("train_loss", loss.item(), total_train_step)

    # 测试步骤开始
    tudui.eval()  # 这步在一般情况下可以没有 评估状态
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():  # 评估的时候不用梯度
        for data in test_dataloader:
            imgs, targets = data
            outputs = tudui(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            # argmax显示最大的参数是第几个 argmax(1)代表看每行的 argmax(0)代表看每列的
            # 列表==列表 会返回一个列表 其中的每一项都是对应项比较后的True或者False
            # 列表.sum()会计算列表中True的数量
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy = total_accuracy + accuracy

    print("整体测试集上的Loss: {}".format(total_test_loss))
    print("整体测试集上的正确率: {}".format(total_accuracy/test_data_size))
    writer.add_scalar("test_loss", total_test_loss, total_test_step)
    writer.add_scalar("test_accuracy", total_accuracy/test_data_size, total_test_step)
    total_test_step = total_test_step + 1

    torch.save(tudui, "tudui_{}.pth".format(i))
    print("模型已保存")

writer.close()
```

## GPU加速
需要在三个地方进行设置
1. 网络
2. 数据集
3. 损失函数
### 方法一
用.cuda()的方法

```python
import torch
import torchvision
from torch.utils.tensorboard import SummaryWriter
from torch import nn
from torch.utils.data import DataLoader

train_data = torchvision.datasets.CIFAR10(root="dataset", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10(root="dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)

# length 长度
train_data_size = len(train_data)
test_data_size = len(test_data)
# 如果train_data_size=10, 训练数据集的长度为：10
print("训练数据集的长度为：{}".format(train_data_size))
print("测试数据集的长度为：{}".format(test_data_size))


# 利用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 创建网络模型
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model = nn.Sequential(
            nn.Conv2d(3, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model(x)
        return x


tudui = Tudui()
if torch.cuda.is_available():
    tudui = tudui.cuda()
    print('正在使用gpu')

# 损失函数
loss_fn = nn.CrossEntropyLoss()
if torch.cuda.is_available():
    loss_fn = loss_fn.cuda()
# 优化器
# learning_rate = 0.01
# 1e-2=1 x (10)^(-2) = 1 /100 = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(tudui.parameters(), lr=learning_rate)

# 设置训练网络的一些参数
# 记录训练的次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

# 添加tensorboard
writer = SummaryWriter("logs_train")

for i in range(epoch):
    print("-------第 {} 轮训练开始-------".format(i+1))

    # 训练步骤开始
    tudui.train()
    for data in train_dataloader:
        imgs, targets = data
        if torch.cuda.is_available():
            imgs = imgs.cuda()
            targets = targets.cuda()
        outputs = tudui(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:
            print("训练次数：{}, Loss: {}".format(total_train_step, loss.item()))
            writer.add_scalar("train_loss", loss.item(), total_train_step)

    # 测试步骤开始
    tudui.eval()
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs, targets = data
            if torch.cuda.is_available():
                imgs = imgs.cuda()
                targets = targets.cuda()
            outputs = tudui(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy = total_accuracy + accuracy

    print("整体测试集上的Loss: {}".format(total_test_loss))
    print("整体测试集上的正确率: {}".format(total_accuracy/test_data_size))
    writer.add_scalar("test_loss", total_test_loss, total_test_step)
    writer.add_scalar("test_accuracy", total_accuracy/test_data_size, total_test_step)
    total_test_step = total_test_step + 1

    torch.save(tudui, "pth/tudui_{}.pth".format(i))
    print("模型已保存")

writer.close()
```

### 方法二

用to(device)的方法

```python
import torch
import torchvision
from torch.utils.tensorboard import SummaryWriter
from torch import nn
from torch.utils.data import DataLoader

# 定义训练的设备
device = torch.device("cuda")  # 也可以写成"cuda:0"表示第1块gpu,如果是cpu的话就直接写'cpu'

train_data = torchvision.datasets.CIFAR10(root="dataset", train=True, transform=torchvision.transforms.ToTensor(),
                                          download=True)
test_data = torchvision.datasets.CIFAR10(root="dataset", train=False, transform=torchvision.transforms.ToTensor(),
                                         download=True)

# length 长度
train_data_size = len(train_data)
test_data_size = len(test_data)
# 如果train_data_size=10, 训练数据集的长度为：10
print("训练数据集的长度为：{}".format(train_data_size))
print("测试数据集的长度为：{}".format(test_data_size))


# 利用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 创建网络模型
class Tudui(nn.Module):
    def __init__(self):
        super(Tudui, self).__init__()
        self.model = nn.Sequential(
            nn.Conv2d(3, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model(x)
        return x
tudui = Tudui()
tudui = tudui.to(device)  # 模型网络和损失函数这里可以直接.to(device),不用赋值,但是在后面数据那里还是需要赋值的

# 损失函数
loss_fn = nn.CrossEntropyLoss()
loss_fn = loss_fn.to(device)  # 模型网络和损失函数这里可以直接.to(device),不用赋值,但是在后面数据那里还是需要赋值的
# 优化器
# learning_rate = 0.01
# 1e-2=1 x (10)^(-2) = 1 /100 = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(tudui.parameters(), lr=learning_rate)

# 设置训练网络的一些参数
# 记录训练的次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

# 添加tensorboard
writer = SummaryWriter("logs_train")

for i in range(epoch):
    print("-------第 {} 轮训练开始-------".format(i+1))

    # 训练步骤开始
    tudui.train()
    for data in train_dataloader:
        imgs, targets = data
        imgs = imgs.to(device)
        targets = targets.to(device)
        outputs = tudui(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:
            print("训练次数：{}, Loss: {}".format(total_train_step, loss.item()))
            writer.add_scalar("train_loss", loss.item(), total_train_step)

    # 测试步骤开始
    tudui.eval()
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs, targets = data
            imgs = imgs.to(device)
            targets = targets.to(device)
            outputs = tudui(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            accuracy = (outputs.argmax(1) == targets).sum()
            total_accuracy = total_accuracy + accuracy

    print("整体测试集上的Loss: {}".format(total_test_loss))
    print("整体测试集上的正确率: {}".format(total_accuracy/test_data_size))
    writer.add_scalar("test_loss", total_test_loss, total_test_step)
    writer.add_scalar("test_accuracy", total_accuracy/test_data_size, total_test_step)
    total_test_step = total_test_step + 1

    torch.save(tudui, "tudui_{}.pth".format(i))
    print("模型已保存")

writer.close()
```
