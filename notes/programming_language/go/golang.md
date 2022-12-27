# 0.windows下搭建Go开发环境

## 0.1 SDK  Software Development Kit

官网下载安装(新建一个目录安装就行)

cmd  看到下面的提示信息，表示安装成功

![image-20221027214843600](golang.assets/image-20221027214843600.png)

   ## 0.2配置环境变量

![image-20221027215858563](golang.assets/image-20221027215858563.png)



![image-20221027220012581](golang.assets/image-20221027220012581.png)

==新建一个GOPATH,保存go项目地址==

![image-20221027221008503](golang.assets/image-20221027221008503.png)



==**说明**==

**golang环境变量配置的作用**

GOROOT: 指定go sdk安装目录

Path:指定sdk\bin 目录： go.exe  godoc.exe  gofmt.exe

GOPATH：就是golang的工作目录：我们项目的源码都放在这个目录下

**go程序编写、编译、运行**

golang编写就是写源码

编译就是生成一个二进制可执行文件

运行：对可执行文件运行



# 1. 开始

## 1.1 第一个go程序



```go
// 要求开发一个hello.go程序，可以输出 "hello,wi=orld!"

package main
import "fmt"
func main() {
	fmt.Println("hello,world!")
}
```

==代码说明==

1. go文件的后缀是.go

2. package main

   表示hello.go 文件所在的包是main，在go中，每个文件都必须归属于一个包

3. import "fmt"

   表示：引入一个包，包名为fmt,引入该包后，就可以使用fmt包的函数，比如：fmt.Println

4. func main() {

   }

   func 是一个关键字，表示一个函数

   main是函数名，是一个主函数，即我们的程序入口

5. fmt.Println("hello")

   表示调用fmt包的函数Println输出"hello,world!"

6. 使用go build命令对该go文件进行编译，生成exe文件

   ![image-20221027223957170](golang.assets/image-20221027223957170.png)

7. 运行hello.exe文件即可

   ![image-20221027224130128](golang.assets/image-20221027224130128.png)

 

8. go run命令直接编译加运行

## 1.2Vscode 下创建新项目的步骤

1. 创建项目并初始化

   `新建文件夹 --->cmd----> code . --->打开终端--->go mod init 文件夹名`

2. 创建包，创建模块相互调用

## 1.3 go 代码组织

1. go应用包和模块来组织代码

2. 包： 文件夹

3. 模块： .go 文件

4. 一个包中有多个模块或多个子包

## 1.4 go语言语法要求和注意事项

1. go应用程序的执行入口时main函数
2. go语言严格区分大小写
3. go方法由一条条语句构成，每个语句后不需要分号(go语言会自动在每行后自动加分号)
4. go编译器是一行一行编译的，所以一行只写一条语句
5. ==go语言定义的变量，或者import引入的包没有用到，代码不能通过==
6. 大括号必须成对出现

## 1.5 转义字符

和其他语言一样

## 1.6 go语言的注释

// 行注释

/* */块注释

## 1.7 go 标识符、关键字、命名规则

### 标识符

1. 标识符由数字、字母和下划线组成

2. 只能以字母和下划线开头

3. 标识符区分大小写

4. 包名，保持package的名字和目录保持一致，尽量采取有意义的，简短，不要和标准库冲突

5. 变量名，函数名，常量名：采用驼峰命名法

6. 如果变量名，函数名，常量名首字母大写，则可以被其他的包访问；

   如果首字母小写，则只能在本包中使用

   （注：可以简单理解成首字母大写是公开的public,首字母小写是私有的private）

### 关键字

![image-20221130124312079](golang.assets/image-20221130124312079.png)



## 1.7 规范的代码风格

1. 推荐使用行注释
2. 要有正确的缩进 gofmt -w main.go 格式化代码(在命令行执行)
3. 运算符两边习惯加空格
4. go语言代码风格{}大括号行尾风格

## 1.8 golangAPI

Aplication Programming Interface   应用程序编程接口提供编程所用的各种函数

# 2. 变量

## 变量使用

```go
func main(){
	var i int
	// 第一种：指定变量类型，声明后若不赋值，使用默认值
	fmt.Println("i=", i)

	// 第二种，根据值自行判定变量类型(类型推导)
	var num = 10.11
	fmt.Println("num",num)


	// 第三种， 省略var，注意 := 左侧的变量不应该是已声明过的，狗则会导致编译错误
	// 下面的方式等价 var nam string  name = "tom"
	name := "tom"
}
```

## 多变量声明

```go
// 多变量声明
var n1, n2, n3 int
fmt.Println("n1=",n1, "n2=",n2, "n3=",n3)

var n1, name, n3 = 100, "tom", 888
fmt.Println("n1=", n1, "name=", name, "n3=", n3)
```

## 定义多个全局变量

```go
var (
	n3 = 300
	n4 = 900
	name2 = "mary"
)
```

## 变量的格式化输出

```go
package main

import (
	"fmt"
	"project01/user"
)

func main() {

	s := user.Hello()
	fmt.Printf("s: %v\n", s)
	// var name string = "Wednesday"
	// var age int = 20
	// var email string = "1750218633@qq.com"
	var (
		name  string = "Wednesday"
		age   int    = 20
		email string = "1750218633@qq.com"
	)

	fmt.Printf("name=%v, age=%d, email=%v", name, age, email)

}
```

### 查看变量的数据类型

```go
fmt.printf("n1= %T", n1)
```

### 查看占用字节大小

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	n1 := 20
	fmt.Printf("n1占用字节的大小为:%d", unsafe.Sizeof(n1))
}
```



## 变量的数据类型



## 注意事项

1. 变量在某一个区域内，数据值可以在同一个类型内不断变化
2. 变量在一个作用域得不能重名
3. 变量 = 变量名 + 值 + 数据类型
4. 变量如果没有赋初始值，golang会自动赋初值

## 查看变量类型和占用字节数

```go
package main
import (
	"fmt"
	"unsafe"
	"reflect"
)

func main(){
	var i int = 10
	i = 10
	i = 50
	fmt.Println("i=", i)
	// i = 1.2
	fmt.Println("i 的类型是：", reflect.TypeOf(i), "i 占用的字节数是", unsafe.Sizeof(i))
}
```

## 整型变量

1. go的整型默认为int型(占8个字节，64位，和int64一样，但type是int )
2. 保小不保大

## 浮点型

1. 默认是float64

2. 支持科学计数法

   `num := 5.1234e2`

## 字符类型

1. 用byte（占1个字节，8位，等价于uint8）来保存
2. go的字符串是由单个字节连接起来的，也就是对于传统的字符串是由字符构成的，而go的不同，它是由字节组成的
3. %c输出码值对应的字符

```go
package main

import (
	"fmt"
)

func main() {
	var c1 byte = 'a'
	var c2 byte = '0'
	fmt.Printf("c1: %c\n", c1)
	fmt.Printf("c2: %c\n", c2)
}
```

## bool类型

1.bool类型只能是true和false

## string 类型

1. 字符串一旦赋值，字符串就不能改变了

2. 双引号会识别转义字符
3. 使用反引号，可以将内容原样输出
4. 当拼接操作很长时，可以分行写，但是将+号保存到上一行

## 基本数据类型的默认值

![image-20221201150640889](golang.assets/image-20221201150640889.png)

## 基本数据类型的相互转换

==不同类型的变量之间赋值时需要显式转换==

```go
package main

import "fmt"

func main() {
	var i int8 = 32
	var n1 float32 = float32(i)
	var n2 = int32(i)
	fmt.Printf("i= %v, n1= %v, n2= %v", i, n1, n2)
}
```



## 基本数据类型转字符串

### **方法一**`fmt.Sprintf`

```go
package main

import "fmt"

func main() {
	var num1 int = 99
	var num2 float64 = 23.456
	var b bool = true
	var mychar byte = 'h'
	var str string

	// 第一种方法
	str = fmt.Sprintf("%d", num1)
	fmt.Printf("str type %T str=%v\n", str, str)
	str = fmt.Sprintf("%f", num2)
	fmt.Printf("str type %T str=%v\n", str, str)
	str = fmt.Sprintf("%t", b)
	fmt.Printf("str type %T str=%v\n", str, str)
	str = fmt.Sprintf("%c", mychar)
	fmt.Printf("str type %T str=%v\n", str, str)

}
```

### **方法二**`strconv`

```go
	var num3 int = 99
	var num4 float64 = 23.456
	var b2 bool = true
	str = strconv.FormatInt(int64(num3), 10)
	fmt.Printf("str type: %T, str=%v\n", str, str)
	str = strconv.FormatFloat(num4, 'f', 10, 64)
	fmt.Printf("str type: %T, str=%v\n", str, str)
	str = strconv.FormatBool(b2)
	fmt.Printf("str type: %T, str=%v\n", str, str)
```

## string类型转基本数据类型

```go
package main

import (
	"fmt"
	"strconv"
	"unsafe"
)

func main() {

	var str string = "true"
	var b bool
	b, _ = strconv.ParseBool(str)
	fmt.Printf("b type is %T, b = %t\n", b, b)

	var str2 string = "123456"
	var n1 int64
	n1, _ = strconv.ParseInt(str2, 10, 0)
	fmt.Printf("n1 type is %T n1 = %v\n", n1, n1)
}
```

## 值类型和引用类型

1. 值类型：基本数据类型 int， float, bool, string, 数组和结构体

   变量直接存储值，内存通常在==栈==中分配

2. 引用类型： 指针， slice切片， map， 管道chan, interface等都是引用类型

   变量存储的是一个地址，这个地址对应的空间才是真正存储数据（值）,内存通常在==堆==上分配，当没有任何变量引用这个地址时，该地址对应的数据空间就成为一个垃圾，这时由GC回收

```go
package main

import "fmt"

func main() {
	var i int = 10
	fmt.Println("i的地址为:", &i)

	var ptr *int = &i
	fmt.Printf("ptr: %v\n", ptr)
	fmt.Printf("ptr存的值为:%v", *ptr)
}
```



# 3. 运算符

## 算数运算符

![image-20221218155919781](golang.assets/image-20221218155919781.png)

==注意事项：==

>  10 % 3 = 10 - 10 / 3 * 3
>
> 自增自减只能当作一个独立的语句使用，不能当作结果赋值，且只能写在变量后面

## 赋值运算符

![image-20221218165934151](golang.assets/image-20221218165934151.png)

## 比较运算符/关系运算符

![image-20221218164555617](golang.assets/image-20221218164555617.png)



## 逻辑运算符

用于连接多个关系运算符，运算结果也是一个bool值

![image-20221218165020974](golang.assets/image-20221218165020974.png)

==注意事项：==

>&& 逻辑与也叫短路与，如若第一个条件为假，则第二段代码不会执行
>
>|| 逻辑或也叫短路或，如果第一个条件为真，则第二段代码不会执行

## 位运算符

![image-20221218171116379](golang.assets/image-20221218171116379.png)

## 取值运算符/取址运算符

![image-20221218171302737](golang.assets/image-20221218171302737.png)

# 4. 输入输出

```go
	// 要求从控制台接收用户信息，[姓名，年龄，薪水， 是否通过考试]
	// 方式一 fmt.Scanfln
	// 先声明需要的变量
	var name string
	var age byte
	var salary float32
	var isPass bool
	fmt.Println("请输入姓名：")
	fmt.Scanln(&name)
	fmt.Println("请输入年龄：")
	fmt.Scanln(&age)
	fmt.Println("请输入薪水：")
	fmt.Scanln(&salary)
	fmt.Println("请输入是否通过考试：")
	fmt.Scanln(&isPass)
	fmt.Printf("%v,年龄：%v,月工资：%v,是否通过考试：%v", name, age, salary, isPass)
```

方法二：

```go
	//方式二 按照指定的格式输入
	fmt.Println("请输入你的姓名，年龄，薪水，是否通过考试，用空格分开")
	fmt.Scanf("%s %d %f %t", &name, &age, &salary, &isPass)
	fmt.Printf("%v,年龄：%v,月工资：%v,是否通过考试：%v", name, age, salary, isPass)
```

# 5. 进制

# 6. 位运算

## 原码，反码，补码

**运算规则：**

1. 二进制的最高位是符号位：**0表示正数，1表示负数**
2. **正数的原码、反码、补码都一样(三码合一)**
3. **负数的反码 = 它的原码符号位不变，其它位取反**
4. **负数的补码 = 它的反码+1，负数的反码 = 负数的补码 - 1**
5. 0的反码，补码都是0
7. ==计算机运算的时候，都是以补码的方式来运算的==
8. ==看运算结果的时候，要看他的原码==

## 按位运算

![image-20221218175841918](golang.assets/image-20221218175841918.png)

## 移位运算

![image-20221218175939155](golang.assets/image-20221218175939155.png)

# 7. 流程控制

## 单分支

```go
if 条件表达式 {
    代码执行块
}
```

==细节说明:==

go的if还有一个强大的地方就是条件判断语句里面允许 声明一个变量，这个变量的作用域只能在该条件逻辑块内，其他地方就不起作用了

```go
if age := 20; age > 18 {
    fmt.Print("你年龄大于18岁，要对自己的行为负责")
}
```

## 双分支

```go
if 条件表达式 {
    代码执行块
} else {
    代码块
}
```

**判断是否是闰年**

```go 
var year int = 2022
if year%4 == 0 && year%100 != 0 || year%400 == 0 {
    fmt.Printf("%v是闰年", year)
} else {
    fmt.Printf("%v不是闰年", year)
}
```

## 多分支

```go 
if 条件表达式1 {
    代码块
} else if 条件表达式2 {
    代码块
} else {
    代码块
}
```

## switch分支

1. switch分支用于基于不同条件执行不同的动作，每一个case分支都是唯一的，从上到下逐一测试，直到匹配为止。
2. 匹配项后面不需要再加break

3. golang的case后的表达式可以有多个，用逗号隔开

4. case后面的各个表达式的值的数据类型，必须和switch的表达式数据类型一致

5. case后面的表达式如果是常量值，则要求不能重复

6. default语句不是必须的

7. switch后面也可以不带表达式，类似于if-else分支来使用

   ```go 
   var age int = 10
   switch {
   case age == 10:
       fmt.Println("age == 10")
   case age == 20:
       fmt.Println("age == 20")
   default :
       fmt.Println("没有匹配到")
   }
   ```

8. switch 后也可以直接声明/定义一个变量， 分号结束

   ```go
   switch score := 80; {
   case score > 90 :
       fmt.Println("成绩优秀")
   case score >= 70 && score <= 90 :
       fmt.Println("成绩良好")
   case score >= 60 && score <= 70 :
       fmt.Println("成绩及格")
   default ：
       fmt.Println("不及格")
   }
   ```

9. switch 穿透-fallthrought

   ```go
   var num int = 10
   switch num {
   case 10:
       fmt.Println("ok1")
       fallthrough
   case 20:
       fmt.Println("ok2")
   }
   ```

10. tyoe switch



# 8. for 循环

1. 基本用法

   ```go
   func main() {
   	for i := 1; i <= 10; i++ {
   		fmt.Println("你好，老韩")
   	}
   }
   ```

2. 用法二

   ```go 
   j := 1
   for j <= 10 {
       fmt.Println("你好")
       j++
   }
   ```

3. 死循环，配合break使用

   ```go
   	k := 1
   	for {
   		if k <= 10 {
   			fmt.Println("ok")
   		} else {
   			break
   		}
   		k++
   	}
   ```

**注意事项：**

1. for循环条件必须是bool值

2. for-range遍历字符串

   ```go
   var str string = "Hello, world!"
   for i := 0; i < len(str); i++ {
       fmt.Printf("%c", str[i])
   }
   
   for index, value := range str {
       fmt.Printf("index:%d, val:%c\n", index, value)
   }
   ```

   ==如果字符串中有中文，那么传统的遍历字符串的方式会出错。原因是传统的对字符传的遍历，是按字节遍历的，而一个汉字占三个字节==

# 9. while

## for 循环实现while

```go
func main() {
	var i int = 1
	for {
		if i > 10 {
			break
		}
		fmt.Println(i)
		i++
	}
```

## for 循环实现do-while

```go 
var j int = 1
for {
    fmt.Println(j)
    if j > 10 {
        break
    }
    j++
}
```

# 10. 跳转控制语句

## break

```go
func main() {

	var count int = 0

	for {
		rand.Seed(time.Now().UnixNano())
		n := rand.Intn(100) + 1
		fmt.Println(n)
		count++
		if n == 99 {
			break
		}
	}
	fmt.Println(count)
}
```

1. break语句出现在多层嵌套的语句块中时，可以通过标签指明要终止的是那一层语句块

   ```go
   tag1:
   	for i := 0; i < 4; i++ {
   		for j := 0; j <= 10; j++ {
   			if j == 2 {
   				break tag1
   			}
   			fmt.Println("j=", j)
   		}
   	}
   ```

## continue

1. 用于结束本次循环，接续执行下一次循环

2. continue语句出现在多层嵌套循环语句体中时，可以通过标签指明要跳过的是那一层循环

## goto

1. 语言的goto语句可以无条件地转移到程序中指定的行。
2. goto语句通常与条件语句配合使用，可用来实现条件转移，跳出循环体等功能
3. 在go程序设计中一半不主张使用goto语句，以免造成程序流程的混乱，使理解和调试程序产生困难

```go
func main() {
	n := 11
	fmt.Println("ok1")
	if n > 10 {
		goto label1
	}
	fmt.Println("ok2")
	fmt.Println("ok3")
	fmt.Println("ok4")
label1:
	fmt.Println("ok5")
}
```

## return

return 使用在方法或者函数中，表示跳出所在的方法或函数

# 11. 函数

## 基本语法

```go 
func 函数名 (形参列表) (返回值列表) {
    执行语句...
    return 返回值列表
}
```

```go
func getSumAndSub(n1 int, n2 int) (int, int) {
	return n1 + n2, n1 - n2
}

func main() {
	var number1 int = 12
	var number2 int = 5
	var sum int
	var sub int
	sum, sub = getSumAndSub(number1, number2)
	fmt.Println("sum:", sum, "sub:", sub)
}
```



1. 如果返回多个值时，在接收时，希望忽略某个返回值，则使用`_`符号表示占位忽略

   ```go
   _, sub2 := getSumAndSub(number1, number2)
   fmt.Println("sub2:", sub2)
   ```

2. 返回值只有一个，返回值类型列表可以不写()

3. 形参列表和返回值列表的数据类型可以是值类型和引用类型

4. 函数命名遵循标识符命名规范，首字母不能是数字，首席木大写函数可以被本包文件和其他套文件使用；首字母小写，只能被本报文件使用

5. 函数中的变量是局部的，函数外不能调用

6. 基本数据类型和数组默认都是值传递，即进行值拷贝。在函数内修改，不会应影响到原来的值

7. 如果希望函数内的变量能修改函数外的变量，可以传入变量的地址&，函数内以指针的方式操作变量

   ```go
   func test(n1 *int) {
   	*n1 = *n1 + 1
   	fmt.Println("test_n1:", *n1)
   }
   
   func main() {
   	var n1 int = 3
   	test(&n1)
   	fmt.Println("main_n1:", n1)
   }
   
   ```

8. go函数不支持重载

9. go中，函数也是一种数据类型，可以赋给一个变量，则该变量就是一个函数类型的变量了，通过该变量可以对函数调用

   ```go
   func getSum(n1 int, n2 int) int {
   	return n1 + n2
   }
   func main() {
   	a := getSum
   	res := a(10, 50)
   	fmt.Println("res:", res)
   }
   ```

   

10. 函数既然是一种数据类型，因此在go中，函数可以作为形参，并且调用

    ```go
    func getSum(n1 int, n2 int) int {
    	return n1 + n2
    }
    func myFunc(funvar func(int, int) int, n1 int, n2 int) int {
    	return funvar(n1, n2)
    }
    func main() {
    	res2 := myFunc(getSum, 10, 50)
    	fmt.Println("res2:", res2)
    }
    ```

    

11. 为了简化数据类型定义，go支持自定义数据类型

    在go中，myInt 和int虽然都是int类型，但是go认为myInt和int是两个类型

    ```go
    type myInt int
    var number myInt
    ```

    案例：

    ```go
    func getSum(n1 int, n2 int) int {
    	return n1 + n2
    }
    func myFunc(funvar func(int, int) int, n1 int, n2 int) int {
    	return funvar(n1, n2)
    }
    
    type myFunType func(int, int) int
    
    func myFunc2(funvar myFunType, n1 int, n2 int) int {
    	return funvar(n1, n2)
    }
    func main() {
    	res3 := myFunc2(getSum, 10, 50)
    	fmt.Println("res3:", res3)
    }
    ```

12. 支持对函数返回值命名

    ```go
    func getSumAndSub(n1 int, n2 int) (sum int, sub int) {
        sum = n1 + n2
        sub = n1 - n2
        return
    }
    
    sum, sub := getSumAndSub(12, 13)
    fmt.Println("sum:", sum, "sub", sub)
    ```

13. 支持可变参数

    ```go
    func sum(n1 int, args ...int) int {
    	sum := n1
    	for i := 0; i < len(args); i++ {
    		sum += args[i]
    	}
    	return sum
    }
    func main() {
    	res := sum(1, 2, 3, 4, 5)
    	fmt.Println("res:", res)
    }
    ```

    

## 调用机制

![image-20221222101541858](golang.assets/image-20221222101541858.png)

1. 在调用一个函数时，会给还函数分配一个新的空间，编译器会通过自身的处理让这个新的空间和其他的栈的空间区分开
2. 在每个函数对应的栈中，数据空间是独立的，不会混淆
3. 当一个函数调用完毕，程序会销毁这个函数对应的栈空间

## 递归

```go
func fibonacci(n int) int {
	if n == 0 {
		return 0
	} else if n == 1 {
		return 1
	} else {
		return fibonacci(n-1) + fibonacci(n-2)
	}
}

func main() {
	var n int = 6
	for i := 1; i <= n; i++ {
		res := fibonacci(i)
		fmt.Println(res)
	}
}
```

## init函数

​         每一个源文件都可以包含一个init函数，该函数会在main函数执行前，被go运行框架调用，也就是说init会在main函数前被调用

1. 通常可以在init函数中进行初始化工作

```go
// 
func init() {
	fmt.Println("init()...")
}
func main() {
	fmt.Println("main()...")
}
```

2. 如果一个文件同时包含全局变量定义，init函数和main函数，则执行流程是，变量定义->init函数->main函数

   ```go
   var age int = test()
   
   func test() int {
   	fmt.Println("test()...")
   	return 90
   }
   
   // 通常可以在init函数中进行初始化工作
   func init() {
   	fmt.Println("init()...")
   }
   func main() {
   	fmt.Println("main()...age=", age)
   }
   ```

3. 执行流程

   ![image-20221222182533097](golang.assets/image-20221222182533097.png)

## 匿名函数

1. 如果一个函数只希望使用一次，可以考虑匿名函数，匿名函数也可以调用多次

2. 调用方式

   + 匿名函数在定义时就直接调用，这种方式的匿名函数只能调用一次

   ```go
   func main() {
   	res1 := func(n1 int, n2 int) int {
   		return n1 + n2
   	}(10, 20)
   	fmt.Println("res1:", res1)
   }
   ```

   + 将匿名函数赋给一个变量，再通过该变量来调用匿名函数

   ```go
   func main() {
   	a := func(n1 int, n2 int) int {
   		return n1 - n2
   	}
   	res2 := a(10, 12)
   	fmt.Println("res2:", res2)
   }
   ```

3. 全局匿名函数

   ```go
   var (
   	fun1 = func(n1 int, n2 int) int {
   		return n1 * n2
   	}
   )
   
   func main() {
   	res3 := fun1(4, 9)
   	fmt.Println("res3:", res3)
   }
   ```

## 闭包

<span style="color:red">闭包就是一个函数和与其相关的引用环境组合的一个整体(实体)</span>

```go
// 累加器
func AddUpper() func(int) int {
	var n int = 10
	return func(x int) int {
		n = n + x
		return n
	}
}
func main() {
	f := AddUpper()
	fmt.Println(f(1))
	fmt.Println(f(2))
	fmt.Println(f(3))
}
```

1. AddUpper是一个函数，返回的数据类型是fun (int) int 

2. 闭包的说明

   ![image-20221222212501575](golang.assets/image-20221222212501575.png)

   返回的是一个匿名函数，但是这个匿名函数引用到函数外的n，因此这个匿名函数就和n形成一个整体，形成闭包

3. 当反复调用f函数时，因为n是只初始化一次，因此每调用一次就进行累计

**案例：**

```go
// 文件名拼接
func makeSuffix(suffix string) func(string) string {
	return func(file string) string {
		if !strings.HasSuffix(file, suffix) {
			return file + suffix
		} else {
			return file
		}
	}
}

func main() {
	var suffix string = ".jpg"
	var file string = "cat.jpg"
	f := makeSuffix(suffix)
	fmt.Println("fileName:", f(file))
	fmt.Println("fileName:", f("dog"))
}
```

1. `strings.HasSuffix(fileName, suffixName)`判断fileName是否有后缀suffixName
2. 如果使用传统的方法，也可以轻松实现这个功能，但是传统方法需要每次都传入一个后缀名，比如：.jpg，而闭包因为可以保留上次引用的某个值，所以我们传图一次就可以反复使用

## defer

**为什么需要defer**

在函数中，程序员经常需要创建资源(比如，数据库连接、文件句柄、锁等)，为了<span style="color:red">在函数执行完毕后，及时的释放资源</span>，go的设计者提供defer(延时机制)

```go
func test() {
    // 关闭文件资源
    file = openfile(文件名)
    defer file.close()
    // 其他代码
}

func test2() {
    // 释放数据库资源
    connect = openDatabase()
    defer connect.close()
    // 其他代码
}
```



1. 当执行到defer时，暂时不执行，会将defer后面的语句压入到独立的栈中(defer栈)
2. 当函数执行完毕后，再从defer栈，按照先入后出的方法出栈，执行

```go
func sum(n1 int, n2 int) int {
	defer fmt.Println("ok1 n1=", n1) // 3
	defer fmt.Println("ok2 n2=", n2) // 2

	res := n1 + n2
	fmt.Println("ok3 res=", res) // 1
	return res
}
func main() {
	res := sum(10, 20)
	fmt.Println("ok4 res=", res) // 4
}
```

3. 在defer将语句放入栈时，也会将相关的值拷贝同时入栈

## 函数参数传递方式

不管是那种传递方式，传递给函数的都是变量的副本，不同的是，值传递是值的拷贝，引用传递的是地址的拷贝，一般来说，地址拷贝效率高，因为数据量小，而值拷贝取决于拷贝的数据大小，数据越大，效率越低

## 字符串常用系统函数

1. 统计字符串长度

   ```go
   str := "hello山西"
   fmt.Println("str len=", len(str))
   ```

2. 字符串遍历，同时处理有中文的问题

   ```go
   r := []rune(str)
   for i := 0; i < len(r); i++ {
       fmt.Printf("字符=%c\n", r[i])
   }
   ```

3. 字符串转整数

   ```go
   n, err := strconv.Atoi("123")
   if err != nil {
       fmt.Println("转换错误", err)
   } else {
       fmt.Printf("n=%v", n)
   }
   ```

4. 整数转字符串

   ```go
   str = strconv.Itoa(12345)
   fmt.Printf("类型是：%T, 转换结果:%v", str, str)
   ```

5. 字符串转[]byte

   ```go
   var bytes = []byte("hello.go")
   fmt.Printf("bytes=%v\n", bytes)
   // bytes=[104 101 108 108 111 46 103 111]
   ```

6. []byte转字符串

   ```go
   str = string([]byte{97, 98, 99})
   fmt.Printf("str= %v\n", str)
   ```

7. 十进制转其他进制

   ```go
   str = strconv.FormatInt(123, 2)
   fmt.Printf("123对应的二进制是：%v", str)
   str = strconv.FormatInt(123, 16)
   fmt.Printf("123对应的十六进制是：%v", str)
   ```

8. 查找字符串是否在指定的字符串中

   ```go
   var tag = strings.Contains("seefood", "foo")
   fmt.Println(tag)
   ```

9. 统计一个字符串中有几个指定的字串

   ```go
   num := strings.Count("cheese", "e")
   fmt.Printf("num: %v\n", num)
   ```

10. 不区分大小写的字符串比较(==是区分字母大小写的):

    ```go
    strings.EqualFold("abc", "Abc")
    ```

11. 返回字串在字符串中第一次出现的index值，如果没有返回-1

    ```go
    index := strings.Index("NLT_abc", "abc")
    ```

12. 返回字符串在字符串中最后一次出现的Index值

    ```go
    index := strinf.LastIndex("NLT_abcabc", "abc")
    ```

13. 将指定的子串替换成另外一个字串

    ```go
    srtings.Replace("go go hello", "go", "go语言"， 1)
    str = strings.Replace("go go hello", "go", "go语言", -1)
    fmt.Printf("str: %v\n", str)
    ```

14. 按照指定的某个字符为分割标识，将一个字符串拆分成字符串数组，

    ```go 
    strArr := strings.Split("hello,world,ok", ",")
    fmt.Printf("strArr: %v\n", strArr)
    ```

15. 将字符串进行大小写的转换

    ```go
    str = "goLang Hello"
    str = strings.ToLower(str)
    fmt.Printf("str: %v\n", str)
    ```

16. 将字符串两边的空格去掉

    ```go
    str = strings.TrimSpace(" tn a lone gopher ntrn   ")
    fmt.Printf("str: %v\n", str)
    ```

17. 将字符串左右两边指定的字符去掉

    ```go
    // 将左右两边的!和空格去掉
    str = strings.Trim("! hello ! ", " !")
    fmt.Printf("str: %q\n", str)
    ```

18. `strings.TrimLeft()`

19. `strings.TrimRight()`

20. 判断字符串是否以指定的字符串开头

    ```go
    flag := strings.HasPrefix("frp://192.168.10.1", "ftp")
    fmt.Printf("flag: %v\n", flag)
    ```

21. 判断字符串是否以指定的字符串结尾

    ```go
    flag = strings.HasSuffix("NLT_abc.jpg", "abc")
    fmt.Printf("flag: %v\n", flag)
    ```

### 时间和日期相关函数

1. time.Time类型用来表示时间

2. 获取当前时间的方法

   ```go
   now := time.Now()
   fmt.Printf("now=%v now type=%T", now, now)
   ```

​	

# 12. 包

## 基本概念

go的每一个文件都是属于一个包的，也就是说go以包的形式来管理文件和项目目录结构的

## 包的作用

1. 区分相同名字的函数、变量等标识符
2. 当程序文件很多时，可以很好的管理项目
3. 控制函数、变量等访问范围，即作用域

## 包的具体使用

1. 打包基本语法

   `package util`

2. 引入包的基本语法

   `import "包的路径"`

![image-20221221234821636](golang.assets/image-20221221234821636.png)

<span style="color:red">**路径的根目录是go.mod所在的文件夹**</span>

```go
package main

import (
	"fmt"
	"project01/packagedemo/utils"
)

func main() {
	var n1 float64 = 32
	var n2 float64 = 4
	var operator byte = '/'
	result := utils.Cal(n1, n2, operator)
	fmt.Println(result)
}
```

3. 包名通常和文件夹名一样(也可以不一样)，一般为小写字母

4. 为了让其他包的文件能够访问到本包的函数，函数名的首字母应该大写

5. 在访问其他包的函数，变量时，其语法是<span style="color:red">包名.函数名</span>

6. 如果包名过长，go支持给包取别名，取别名之后，原来的包名就不能使用了

   ```go
   package main
   
   import (
   	"fmt"
   	util "project01/packagedemo/utils"
   )
   
   func main() {
   	var n1 float64 = 32
   	var n2 float64 = 4
   	var operator byte = '/'
   	result := util.Cal(n1, n2, operator)
   	fmt.Println(result)
   }
   ```

7. 同一个包下的函数名不能重复，否则会报重复定义

8. 如果要编译生成一个可执行文件，就需要将这个报声明为main

   