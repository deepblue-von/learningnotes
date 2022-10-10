## 变量

```python
# python是弱类型语言
y = 3
y = "hello"

x = y = 1
ptint(x, y)

x, y = 2, 3
print(x, y)

# 交换两个变量的值
x, y = y, x
print(x, y)

# 删除变量
num = 60
del num
```

## input and output

==input接受到的统一为字符串==

~~~python
print(int(float("5.2")))
print(str(10) + "年")
~~~

## 占位符

```python
name = "渣渣辉"
age = 50
# s = "你好，渣渣辉，年龄50，在贪玩蓝月中等你来砍我"

# %s %d %f
s = "你好，%s,年龄%d,在贪玩蓝月中等你来砍我" % (name, age)
print(s)

# format
s = "你好，{},年龄{},在贪玩蓝月中等你来砍我".format(name, age)
s = "你好，{n},年龄{a},在贪玩蓝月中等你来砍我".format(n=name, a=age)
print(s)

# b、d、o、x 分别是二进制、十进制、八进制、十六进制。
# 十进制的数一般是可以不带D的，默认就是十进制
str.format("{0:1}*{1:1}={2:<3}", i, j, i*j)

# f-string
s = f"你好，{name},年龄{age},在贪玩蓝月中等你来砍我"
print(s)
```

