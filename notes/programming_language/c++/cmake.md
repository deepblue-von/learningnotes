# make基础使用

![image-20231025152030460](cmake.assets/image-20231025152030460.png)



![image-20231025152049282](cmake.assets/image-20231025152049282.png)

## 生成项目构建系统

以当前目录 . 作为项目源目录，mybuild作为构建目录

```c++
cmake -S . -B mybuild
```

## 构建项目

```c++
cmake --build mybuild
```

## 运行可执行文件

```c++
mybuild/cmake_learn
```



## cmake基础函数

![image-20231127205616775](cmake.assets/image-20231127205616775.png)

