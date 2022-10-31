##  创建 Numpy Array

```python
import numpy as np
np_array = np.array([1, 2, 3])
print(type(np_array))  #  <class 'numpy.ndarray'
print(np_array.shape)  #  (3,)

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

# np_array的维度(axis轴)和大小
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
# ravel返回一个一维数组
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

```python
array = np.arange(10)
print(array)
# 第三个元素到 第五个元素
print(array[2: 5])
# 第一个元素到第五个元素
print(array[:5])
# 从第一个到倒数第4个
print(array[: -3])
# 第4个元素到倒数第2个元素
print(array[3: -1])
# 颠倒numpy数组
print(array[::-1])
# 从第3个元素到第7个元素
print(array[2: 7: 3])
# 从倒数第3个元素,到第5个元素（4）回跳一个位置)
print(array[-3: 3: -1])
```

### 二维数组索引

```python
# 二维矩阵的索引
matrix_1 = np.arange(20).reshape(4, 5)
print(matrix_1)
print(matrix_1[0, 1])
# 取第三行
print(matrix_1[2, :])
# 取第三列
print(matrix_1[:, 2])
# row: 1~2, columns: 2~3
print(matrix_1[1: 3, 2: 4])

print(matrix_1)
# 获取特定位置的多个元素并生成一个一维数组
print(matrix_1[[0, 2, 2], [0, 1, 0]])
print(matrix_1)
# row: 0 and 3   columns: 2~4
print(matrix_1[(0, 3), 2: 5])
```

### 高维数组

```python
# 数组中有两个5行8列的数组
matrix_3d = np.arange(80).reshape(2, 5, 8)

# ellipsis...未显性定义的所有维度不变
print(matrix_3d[1, ...])

```

