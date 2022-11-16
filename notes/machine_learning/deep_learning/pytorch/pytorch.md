# 什么是pytorch

是一个基于python的一个科学计算包

+ numpy的替代品，可以利用GPU的性能进行计算
+ 深度学习平台，具有足够的灵活性和速度

# tensor(张量)

## tensor自动微分

**1. 用来保存网络中数据的类，其中grad也是一个tensor,因此要想取得梯度的值需要tensor.data**

<img src="pytorch.assets/image-20221111094258266.png" alt="image-20221111094258266" style="zoom: 50%;" />



```python
import torch 
x_data = [1.0, 2.0, 3.0]
y_data = [2.0, 4.0, 6.0]

w = torch.tensor([1.0])
```

**2. 权重需要计算梯度时，设置为True用来跟踪与w相关的计算,默认是不需要计算梯度的**

```python
w.requires_grad = True
```

+ **.grad**

  张量的梯度将会累积到.grad属性中，可以调用backword()计算所有梯度
  
  ```python 
  # 将梯度保存在w中，然后计算图释放
  loss.backword()
  ```
  
  每次更新完梯度之后将所有梯度清零
  
  ```python
  # 将所有梯度清零
  w.grad.zero()
  ```
  
  

+ .grad_fn



## tersor数据类型转换

+ **数据存储位置转换**

  CPU张量---->GPU张量，使用`data.cuda()`

  GPU张量---->CPU张量，使用`data.cpu()`

+ **与numpy数据类型转换**

  tensor---->numpy, 使用`data.numpy()`，data为tensor变量

  numpy---->tensor, 使用`torch.from_numpy(data)`, data为numpy变量

+ **与python数据类型转换**

  tensor---->单个python数据，使用`data.item()`，data为tensor变量且只能包含单个数据
  
  tensor---->python list,使用`data.tolist()`, data为tensor变量，返回shape相同的可嵌套的list

## tensor创建

```python
from __future__ import print_function
import torch
```

1. 构造一个5*3的矩阵，不初始化

```python
x = torch.empty(5, 3)
print(x)
```

2. 构造一个随机初始化的矩阵

```python
x = torch.rand(5,3)
```

3. 构造一个全零的矩阵

```python
z = torch.zeros(5, 3, dtype=torch.long)
print(z)
```

4. 构造一个张量（直接使用数据）

```python
x = tensor([5.5, 3])
```



## tensor运算

1. 按元素相除时，必须先将数据转换为浮点型，否则计算结果只有整数部分

```python
x.float() / y,float()
```

2. 使用@做矩阵乘法

```python 
# x 与 y的转置做矩阵乘法
x @ y.t()
```

3. 使用条件判别可以生成元素为0或1的新tensor,在numpy中生成的是True和False



# pytorch使用



## 导入torch

**为了避免数据类型错误，将tensor的默认数据类型统一设置为double类型**

```python
import torch
torch.set_default_tensor_type(torch.DoubleTensor)
```



![image-20221111155305971](pytorch.assets/image-20221111155305971.png)

==神经网络的本质是寻找一种非线性的空间变换函数==

 

## prepare dataset





## Epoch

所有训练样本都进行一次前馈和一次反向传播为一个epoch

## Batch-Size

进行一次前馈一次反馈一次 更新所用样本的大小

## Iteration

每个epoch需要多少次迭代

