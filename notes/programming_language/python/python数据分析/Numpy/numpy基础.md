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



### 一维数组

**1. 一维数组创建**

```python
arr = np.arange(10)
print(arr)
arr[5:8] = 12
print((arr[5:8]))
arr_slice = arr[5:8]

```
**2.将一个标量值赋值给一个切片时（如arr[5:8]=12），该值会自动传播**
```python
print(arr_slice)  # [12 12 12]
arr_slice[1] = 12345
```
**3.修改arr_slice中的值，变动也会体现在原始数组arr中**
```python
print(arr_slice)  # [   12 12345    12]
print(arr)  # [    0     1     2     3     4    12 12345    12     8     9]
```
**4.切片[:]会给数组中所有值赋值**
```python
arr_slice[:] = 64
print(arr)
```

### 二维数组

**1.  二维数组创建**
==在二维数组中，各索引位置上的元素不再是标量，而是一维数组==

```python
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr2d[2]) # [7 8 9]
```
**2.获取数组中的单个元素**
```python
arr2d[0][2] # 3
aee2d[0, 2] # 3
```

**3.  用arr[].copy可以将数组中的元素复制出来**

```python
arrc = arr2d[0].copy()
print(arrc)
```

### 三维数组

**1.  创建三维数组**

```python
# 下面创建一个有两个两行三列的三维数组
arr3d = np.array([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
print(arr3d)

# 创建一个有两个五行八列的三维数组
matrix_3d = np.arange(80).reshape(2, 5, 8)
print(matrix_3d)
```

**2. 获取元素**

==在多维数组中，如果省略了后面的索引，则返回对象是一个维度低一点的ndarray==

```python
print(arr3d[0]) # [[1 2 3]
                # [4 5 6]]
```

## 切片索引

### 一维数组

```python
print(arr[1: 6])
```

### 二维数组

**1. 沿着第零轴切片**

```python
print(arr2d[: 2])
```

**2.  拿到前两行和后两列**

```python
print(arr2d[:2, 1:]) # [[2 3]
                     # [5 6]]

arr2d[:2, 1:] = 0  # [[1 0 0]
                   # [4 0 0]
                   # [7 8 9]]
```

==对切片的赋值也会被扩散到整个选取==

## 布尔型索引

```python
names = np.array(["Bob", "joe", "Will", "Bob", "will", "Joe", "joe"])
print(names)

# 产生一个七行四列的服从正太分布的数据
data = np.random.randn(7, 4)
print(data)
```

跟算术运算一样，数组的比较运算（如\==）也是矢量化的。因此，对names和字符串"Bob"的比较运算将会产生一个布尔型数组==这个布尔型数组可用于数组索引：==

```python
print(names == "Bob")  # [ True False False  True False False False]

# 该布尔型数组可用于数组索引
print(data[names == "Bob"])
```

要选择除"Bob"以外的其他值，==既可以使用不等于符号（!=），也可以通过~对条件进行否定==

```python
bool_array = names != "Bob"
print(bool_array)
print(data[bool_array])
print(data[~(names == "Bob")])
```

> ==：Python关键字and和or在布尔型数组中无效。要使用&与|。==

## 花式索引

## 函数

### max()

+ np.max和np.amax是同名函数

```python
amax(a, axis=None, out=None, keepdims=<no value>, initial=<no value>, where=<no value>)
```

### argmax()

**功能：返回最大值的索引**

#### 一维数组

```python
import numpy as np
numbers = np.arange(5)
print(numbers)
numbers.argmax()
```

```python
numbers.argmax()  # 4
```



#### 二维数组

```python
import numpy as np

numbers = np.arange(6).reshape(2,3)
numbers
```



**axis=0, 在列中比较，选出最大的行索引**

```python
numbers.argmax(axis=0)  # [1, 1, 1]
```

**axis=1,在行中比较，选出最大的列索引**

## 模块

### random

1. 打乱原数组的顺序

```python
np.random.shuffle(numbers)
```



