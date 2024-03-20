---
title: PyTorch
date: 2023-08-01 00:00:00
categories: [Pytorch]
---

使用Image读取图片
```python
from PIL import Image
# 读取图片
img_path = 'data/1.jpg'
img = Image.open(img_path)
# 显示图片
img.show()
```
png格式是四个通道，RGB+alpha，alpha表示透明度，alpha=0表示完全透明，alpha=255表示完全不透明。所以，需要调用image = image.convert('RGB')方法将图片转换为RGB格式，保留其颜色通道。

读取一个目录的文件
```python
import os
dir_path = 'data/train'
# 读取该目录下的所有文件名称
path_list = os.listdir(dir_path)
# 将文件名称和路径拼接
path = os.path.join(dir_path, path_list[0])
img = Image.open(path)
img.show()
```

## TensorBoard：数据可视化工具 + transforms
```python
from torch.utils.tensorboard import SummaryWriter
# 创建一个writer对象
writer = SummaryWriter('logs')
writer.add_image('example', img)
writer.add_scalar('test', 0.5, 1)
# 绘制网络结构，输入为网络和输入数据，输出为tensorboard的可视化网络结构
writer.add_graph(net, input)
writer.close()
```
创建完writer对象后，需要在命令行中输入以下命令
```shell
tensorboard --logdir=logs --port=6008
```
输入完之后会出现一个本地地址，默认端口号为6006，为了防止端口冲突，可以指定其他端口号。

writer.add_image()方法读入图片的类型为tensor，ndarray，string类型
```python
from PIL import Image
from torch.utils.tensorboard import SummaryWriter
img = Image.open('data/1.jpg')
print(type(img))
# 将PIL转换为ndarray
import numpy as np
img_array = np.array(img)
# 将PIL转换为tensor
from torchvision import transforms
tensor_trans = transforms.ToTensor()
img_tensor = tensor_trans(img)
# Normalization：输入为tensor，输出为tensor
trans_norm = transforms.Normalize([0.5, 0.5, 0.5], [0.5, 0.5, 0.5])
img_norm = trans_norm(img_tensor)
# Resize:输入为PIL，输出为PIL
trans_resize = transforms.Resize((256, 256))
img_resize = trans_resize(img)
# Compose:输入transform列表
trans_compose = transforms.Compose([transforms.Resize((256, 256)), transforms.ToTensor()])
img_compose = trans_compose(img)
# RandomCrop:输入为PIL，输出为PIL
trans_crop = transforms.RandomCrop(256)
img_crop = trans_crop(img)
# 输出到tensorboard
writer = SummaryWriter('logs')
writer.add_image('example', img_array, 1, dataformats='HWC')
```

## 加载数据
读取数据：Dataset, DataLoader
Dataset:提供一种方式去获取数据及其label
1. 如何获取每一个数据及其label
2. 总共有多少数据
```python
# torchvision中的数据集
import torchvision
train_data = torchvision.datasets.CIFAR10(root='data', train=True, download=True)
```
DataLoader:为网络提供不同的数据形式
```python
import torchvision
from torch.utils.data import DataLoader
test_data = torchvision.datasets.CIFAR10(root='data', train=False, download=True)
# batch_size:将batach_size个数据打包成一个batch
test_loader = DataLoader(test_data, batch_size=64, shuffle=True, num_workers=2, drop_last=True)
# 取出第一个batch
img, label = next(iter(test_loader))
```

## 神经网络搭建
### nn.Module:base class for all neural network modules
```python
import torch
class MyModel(torch.nn.Module):
    def __init__(self):
        super(MyModel, self).__init__()
        self.conv1 = torch.nn.Conv2d(3, 32, 3, 1, 1)
        self.conv2 = torch.nn.Conv2d(32, 64, 3, 1, 1)
        self.fc1 = torch.nn.Linear(64*8*8, 128)
        self.fc2 = torch.nn.Linear(128, 10)
    def forward(self, x):
        x = torch.nn.functional.relu(self.conv1(x))
        x = torch.nn.functional.max_pool2d(x, 2, 2)
        x = torch.nn.functional.relu(self.conv2(x))
        x = torch.nn.functional.max_pool2d(x, 2, 2)
        x = x.view(-1, 64*8*8)
        x = torch.nn.functional.relu(self.fc1(x))
        x = self.fc2(x)
        return x
# 初始化类
net = MyModel()
x = torch.randn(1, 3, 32, 32)
# 调用forward方法
out = net(x)
```
## 卷积层
```python
import torch
# 输入数据
x = torch.randn(1, 3, 32, 32)
# 卷积层
conv1 = torch.nn.Conv2d(3, 32, 3, 1, 1)
out = conv1(x)
print(out.shape)
# 卷积层参数
Conv2d(in_channels, out_channels, kernel_size, stride, padding, dilation, groups, bias=True, padding_mode='zeros')
```
1. 常用参数
in_channels:输入数据的通道数
out_channels:输出数据的通道数
kernel_size:卷积核的大小
stride:步长
padding:填充
2. 其他参数
dilation:扩张
groups:分组卷积
bias:是否使用偏置
padding_mode:填充模式

## 池化层：最大池化层
```python
import torch
# 输入数据
x = torch.randn(1, 3, 32, 32)
# 最大池化层
pool1 = torch.nn.MaxPool2d(2, 2)
out = pool1(x)
print(out.shape)
# 池化层参数
MaxPool2d(kernel_size, stride, padding, dilation, return_indices, ceil_mode)
```
1. 常用参数
kernel_size:池化核的大小
stride:步长
padding:填充
2. 其他参数
dilation:扩张
return_indices:是否返回最大值的索引
ceil_mode:是否向上取整

## 非线性激活层
```python
import torch
# 输入数据
x = torch.randn(1, 3, 32, 32)
# 非线性激活层ReLU、Sigmoid、Tanh
relu = torch.nn.ReLU()
out = relu(x)
print(out.shape)
```

## 线性层/全连接层
```python
import torch
# 输入数据
x = torch.randn(1, 3, 32, 32)
# 线性层
fc = torch.nn.Linear(3*32*32, 10)
out = fc(x)
print(out.shape)
```

## Dropout层
```python
import torch
# 输入数据
x = torch.randn(1, 3, 32, 32)
# Dropout层
drop = torch.nn.Dropout(0.5)
out = drop(x)
print(out.shape)
```

## nn.Sequential
```python
import torch
# 输入数据
x = torch.randn(1, 3, 32, 32)
# 线性层
fc = torch.nn.Linear(3*32*32, 10)
# 非线性激活层ReLU
relu = torch.nn.ReLU()
# 池化层
pool = torch.nn.MaxPool2d(2, 2)
# 卷积层
conv = torch.nn.Conv2d(3, 32, 3, 1, 1)
# Sequential
model = torch.nn.Sequential(
    conv,
    relu,
    pool,
    conv,
    relu,
    pool,
    fc
)
out = model(x)
print(out.shape)
```

## 损失函数
### L1Loss
L1Loss是计算预测值和真实值之间的平均绝对值误差/绝对值误差之和
### MSELoss
MSELoss是计算预测值和真实值之间的均方误差/平方误差之和
### CrossEntropyLoss
CrossEntropyLoss是计算预测值和真实值之间的交叉熵损失，常用于分类问题
### 方向传播
```python
loss = torch.nn.CrossEntropyLoss()
result_loss = loss(input, label)
result_loss.backward()
```

## 优化器
```python
# 参数分别为网络参数，学习率和动量
optimizer = torch.optim.SGD(model.parameters(), lr=0.01, momentum=0.9)
loss = torch.nn.CrossEntropyLoss()
result_loss = loss(input, label)
print(result_loss.item())
# 梯度清零
optimizer.zero_grad()
# 反向传播，让模型存储参数的梯度
result_loss.backward()
# 更新参数
optimizer.step()
```

## 现有模型的参数
```python
# pretrained=True表示模型的参数已经训练好，pretrained=False表示模型的参数未训练
vgg16 = torchvision.models.vgg16(pretrained=True)
# 在现有的模型上添加新的层
vgg16.classifier.add_module('fc', torch.nn.Linear(1000, 10))
# 或者这样添加
vgg16.add_module('fc', torch.nn.Linear(1000, 10))
# 在现有的模型上修改层
vgg16.classifier[6] = torch.nn.Linear(4096, 10)
# 保存和加载模型
# 保存方式1 模型参数+网络结构
torch.save(vgg16, 'vgg16.pth')
# 加载方式1，使用加载方式1时，需要先定义模型的结构，要不然会加载失败
#例如，加载参数前先定义模型的结构
#class Mymodule(nn.Module):
#    def __init__(self):
#        
model = torch.load('vgg16.pth')

# 保存方式2 模型参数
torch.save(vgg16.state_dict(), 'vgg16.pth')
# 加载方式2
## 获得网络模型
vgg16 = torchvision.models.vgg16(pretrained=False)
## 加载参数
vgg16.load_state_dict(torch.load('vgg16.pth'))
```

## 分类问题计算准确率
```python
output = torch.tensor([[0.1,0.2],
[0.3,0.4]])
print(output.argmax(dim=1))
pred = output.argmax(dim=1)
target = torch.tensor([0,1])
print((pred == target).sum().item())
```

## train和eval
```python
# 训练模式
# 对于一些网络层，比如Dropout和BatchNorm，训练和测试时的表现是不一样的
model.train()
# 测试模式
# 对于一些网络层，比如Dropout和BatchNorm，训练和测试时的表现是不一样的
model.eval()
```

## 使用time模块计算训练时间
```python
import time
start = time.time()
# 训练代码
end = time.time()
print('训练时间为：', end-start)
```

## 使用GPU
1. 找到网络模型、数据、损失函数，然后调用.cuda()方法
2. 找到网络模型、数据、损失函数，然后调用.to(device)方法——常用
```python
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
loss.to(device)
model.to(device)
input = input.to(device)
```

## autograd
反向传播查看梯度
```python
loss.backward()
print(w.grad)
print(b.grad)

z = torch.matmul(x, w) + b
print(z.requires_grad) # True
with torch.no_grad(): # 不会记录梯度，内存占用小
    z = torch.matmul(x, w) + b
print(z.requires_grad) # False

z = torch.matmul(x, w) + b
z_det = z.detach() # 返回一个新的tensor，但是不会记录梯度
print(z_det.requires_grad) # False
```
对于backward()方法，如果是标量，可以不传入参数，如果是向量，需要传入参数，参数大小和输出大小一致
```python
x = torch.tensor([1.0, 2.0, 3.0, 4.0], requires_grad=True)
y = 2*x
z = y.view(2, 2)
v = torch.tensor([[1.0, 0.1], [0.01, 0.001]], dtype=torch.float)
z.backward(v)
print(x.grad)
```


