## 创建的ndarray

```python
import numpy as np
# 创建ndarray
data1 = [6, 7.5, 8, 0, 1]
print(data1)
arr1 = np.array(data1)
print(arr1)

# 嵌套序列
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
print(data2)
arr2 = np.array(data2)
print(arr2)

# ndim返回的是数组的维度，返回的只有一个数
print(arr2.ndim)
# shape返回各维度大小的一个元组
print(arr2.shape)
# 返回数组数组的数据类型
print(arr2.dtype)

# zeros和ones分别可以创建指定长度或形状的全0或全1数组
# empty可以创建一个没有任何具体值的数组
arr3 = np.zeros(10)
print(arr3)
# 创建一个三行六列的数组，每一个元素都是0
np.zeros((3, 6))
# 改变数组的形状
np.reshape((2,9))
np.empty((2, 3, 2))

# arange是Python内置函数range的数组版
np.arange(15)
print(np.arange(15))  # [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14]
print(list(range(10)))  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

```

![img](https://camo.githubusercontent.com/a5bfc7ddb3084e72bcfc9057d81524960a6b490d7309043bc547670ff40c149d/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f373137383639312d373861623131663637653730373761362e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

## ndarray的数据类型

```python
#dtype（数据类型）是一个特殊的对象，它含有ndarray将一块内存解释为特定数据类型所需的信息
arr1 = np.array([1, 2, 3], dtype=np.float64)
print(arr1.dtype)

# 通过ndarray的astype方法明确地将一个数组从一个dtype转换成另一个dtype
arr = np.array([1, 2, 3, 4, 5])
print(arr.dtype)
float_arr = arr.astype(np.float64)
print(float_arr.dtype)
```

## numpy数组运算

+ 大小相等的数组之间的任何算术运算都会将运算应用到元素级
+ 数组与标量的算术运算会将标量值传播到各个元素
+ 大小相同的数组之间的比较会生成布尔值数组

## 基本的索引和切片

```python
arr = np.arange(10)
print(arr)
arr[5:8] = 12
print((arr[5:8]))
arr_slice = arr[5:8]
# 将一个标量值赋值给一个切片时（如arr[5:8]=12），该值会自动传播
print(arr_slice)  # [12 12 12]
arr_slice[1] = 12345
# 修改arr_slice中的值，变动也会体现在原始数组arr中
print(arr_slice)  # [   12 12345    12]
print(arr)  # [    0     1     2     3     4    12 12345    12     8     9]

# 二维数组
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr2d[0][2])
print(arr2d[0, 2])
# 用arr[].copy可以将数组中的元素复制出来
arrc = arr2d[0].copy()
print(arrc)
```

