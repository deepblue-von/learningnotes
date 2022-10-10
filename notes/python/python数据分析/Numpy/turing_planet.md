##  创建 Numpy Array

```python
import numpy as np
np_array = np.array([1, 2, 3])
print(type(np_array))  # (3,)
print(np_array.shape)  # [1 2 3]

# 创建一个全0的array
zero_array = np.zeros((5))
print(zero_array)  # [0. 0. 0. 0. 0.]
ones_array = np.ones((4, 4))
print(ones_array)  # [[1. 1. 1. 1.]
                   # [1. 1. 1. 1.]
                   # [1. 1. 1. 1.]
                   # [1. 1. 1. 1.]]
full_array = np.full((3, 3), 7)  
print(full_array)  # [[7 7 7]
                   # [7 7 7]
                   # [7 7 7]]
#对角线全为1
eye_array = np.eye(4)
print(eye_array)
empty_array = np.empty((2, 3))
print(empty_array)

# np_array的维度和大小
dimension = ones_array.ndim
print(dimension)  # 2
size = ones_array.size
print(size)  # 16

# np_array的数据类型
int_array = np.array([1, 2])
print(int_array.dtype)  # int32
float_array = np.array([1.0, 2.0])
print(float_array.dtype)  # float64
# 占多少字节
print(float_array.itemsize)  # 8

float32_array = np.array((1, 2), dtype=np.float32)
print(float32_array.dtype)
print(float32_array.itemsize)


# 将二维数组变成一维
x.shape = (6,)
print(x)

# reshape之后，仍然指向相同的数据
y = np.reshape(x, (3, 2))
print(y)
y[0, 0] = 1000
print(x)
# ravel返回一个以为数组
z = y.ravel()
print(z)
z[0] = 2000
print(y)

# copy复制原来的数据
array1 = np.arange(4)
print(array1)
array2 = array1
array2[0] = 1000
print(array1)

array3 = array1.copy()
array3[0] = 2000
print(array3, array1)
```

## 常用Functions

```
# 产生0-1之间的随机数
random_array = np.random.random((5, 5))
print(random_array)
# 产生0-100之间的随机数
rand1 = np.random.randint(100, size=(5, 5))
print(rand1)
rand2 = np.random.rand(3, 4)
print(rand2)
# 创建的数据服从正态分布
rand3 = np.random.randn(10)
print(rand3)

# arange创建一个等差的数组
print(np.arange(1, 5))
print(np.arange(1.0, 5.0))
# 可以设置步长
print(np.arange(1, 5, 0.5))
print(np.linspace(0, 5, 10))
```

## 创建数组索引

