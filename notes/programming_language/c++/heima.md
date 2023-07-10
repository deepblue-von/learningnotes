# 命名空间

```c++
// 定义一个命名空间
namespace spaceA {
    int g_a = 10;
}

// 方式2
using spaceA::g_a;

// 方式3
using namespace spaceA

int main()
{	
    // 方式1
    cout << spaceA::g_a <<endl:
    return 0
}
```



# 变量

+ 未初始化的变量都被分配到bss段，初始化为0；

+ c++语言不允许重定义，而c语言不进行检测
+ c语言创建结构体变量要写struct关键字 ,  c++可以不用加

# 关键字

![image-20230422202026283](heima.assets/image-20230422202026283.png)

# 数据类型

## 整型

![image-20230519185730003](heima.assets/image-20230519185730003.png)

## bool运算符增强

+ true  1

+ false 0

+ 占用一个字节

## sizeof关键字



# 三目运算符增强

左值：可以放在赋值运算符左边

右值：放在赋值运算符右边

+ 在c语言中三目运算符只能做右值
+ 在c++中三目运算符可以做左值



# const增强

+ c语言中`const` 修饰的变量可以通过指针修改

+ c++中`const` 修饰的变量没有地址，如果对其取地址会临时开辟一个空，变将指针指向这个临时空间

  c++在常量区开辟一个符号表，在编译阶段将常量a替换为10（编译器处理）

+ 宏定义常量是预处理器处理的

![image-20230706094137150](heima.assets/image-20230706094137150.png)

```c++
const int a = 10;
int *p = (int *)&a;
*p = 20;
cout << a << endl;    // c 语言中a 的值被改变了，c++中没有改变
cout << *p << endl;
```



# 枚举的增强

## c

```c
enum season
{
    SPR = 0;
    SUM,
    AUT,
    WIN,
    WIN1,
    WIN2,
    ...
};
void test()
{
    enum season s = 2;
}
```

## c++

```c++
enum season
{
    SPR = 0;
    SUM,
    AUT,
    WIN
};

void test()
{
    enum season s = 3;  // invalid
    enum season s = WIN;  // valid
}
```





# 内存分区

+ 代码区：存放函数体的二进制代码，有操作系统进行管理
+ 全局区：存放全局变量和静态变量以及常量
+ 栈区：由编译器自动分配释放，存放函数的参数值，局部变量等
+ 堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收

## 程序运行前

**代码区**

+ 存放CPU执行的机器指令
+ 代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可
+ 代码区是只读的，使其只读的原因是防止程序意外地修改了它的指令

**全局区**

+ 全局变量和静态变量存放在此
+ 全局区还包含了常量区，字符常量和其他常量也存放在此
+ 该区域的数据在程序结束后由操作系统释放

## 程序运行后

**栈区**

+ 由编译器自动分配释放，存放函数的参数值，局部变量等
+ 注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

```cpp
int *func()
{
    int a = 10;
    return &a;
}

int main()
{
    int *p = func();
    cout << *p << endl; // 第一次可以打印正确的数字，是因为编译器做了保留
    cout << *p << endl; // 第二次这个数据就不再保留了
    
    system("pause");
}
```

**堆区**

+ 由程序员分配释放，若程序员不释放，程序结束时由操作系统回收
+ 在c++中主要利用new在堆区开辟内存

```cpp
int *func()
{
    int *p = new int(10);
    return p;
}

int main()
{
    int *p = func();
    cout << *p << endl; 
    cout << *p << endl;
     
    system("pause");
}
```

<img src="heima.assets/image-20230521150435122.png" alt="image-20230521150435122" style="zoom:50%;" />

## new操作符

![image-20230521150613594](heima.assets/image-20230521150613594.png)

**基本语法**

```cpp
int *func()
{
    int *p = new int(10);
    return p;
}

int main()
{
    int *p = func();
    cout << *p << endl; 
    delete p;
    
    cout << *p << endl;  // 读取访问冲突(非法操作)
     
    system("pause");
}
```

**在堆区开辟数组**

```cpp
void test01()
{
    int *arr = new int[10];
    for(int i = 0; i < 10; i++)
    {
        arr[i] = i;
    }
    for(int i = 0; i < 10; i++)
    {
        cout << arr[i] <<endl;
    }
    
    
    // 释放内存
    delete[] arr;
}
```

# 引用

1. 引用没有定义，是一种关系型声明，声明它和原有某一变量的关系。故而类型与原类型保持一致，且不分配内存，与被引用的变量有相同的地址

2. <span style="color:red">声明的时候必须初始化，一经声明，不可变更</span>>

   `int &b;`  错误

3. 可对引用再次引用。多次引用的结果，是某一变量具有多个别名

4. &符号前有数据类型时，是引用。其它皆为取地址

```c++
int a = 10;
int &re = a;
re = 50; // re是a的引用，可以对a进行修改，re就是a的一个别名
```

## 引用作为函数参数

```cpp
void swap(int &a, int &b)
{
    int temp = a;
    a = b;
    b = temp;
}

int main()
{
    int a = 10; 
    int b = 20;
    
    swap(a, b);
    
    return 0;
}
```

## 引用作为函数返回值

+ 不要返回局部变量的引用
+ 函数的调用可以作为左值

```cpp
// 不要返回局部变量的引用
int &test01()
{
    int a= 10;
    return a;
}

int main()
{
    int &ref = tes01();
    cout << ref << endl;
    cout << ref << endl;
}


// 函数的调用可以作为左值
int &test01() {
    static int a = 10;
    return a;
}

int main() {
    int &ref = test01();
    cout << ref << endl;
    
    test01() = 1000;
    cout << ref << endl;    
}
```

## 引用的本质

==指针常量==

```cpp
int &ref = a;  // 自动转换为int *const ref = &a;

ref = 20; // 内部发现ref是引用，自动转换为*ref = 20;
```

## 指针引用

```cpp
#include <iostream>
#include <cstring>
#include <cstdlib>
using namespace std;

struct teacher
{
    int id;
    char name[64];
};


int get_member(struct teacher **tp)
{
    (*tp) = (teacher *)malloc(sizeof(teacher));
    if (*tp == nullptr)
    {
        return -1;
    }
    (*tp) -> id = 100;
    strcpy((*tp)->name, "zhang");
    return 1;
}

teacher * get_member(int id, const char * name)
{
    teacher *tp;
    tp = (teacher *)malloc(sizeof(teacher));
    if (tp == nullptr)
    {
        cout << "malloc is failed..." << endl;
        return tp;
    }
    tp -> id = id;
    strcpy(tp->name, name);
    return tp;
}

int get_member(struct teacher * t)
{
    t->id = 100;
    strcpy(t->name, "zhangsan");
    return 1;
}

// 指针引用
int get_member1(struct teacher * & t)
{
    t = (teacher *) malloc(sizeof(teacher));
    if (t == nullptr)
    {
        return -1;
    }
    t -> id = 10;
    strcpy(t->name, "lisi");
    return 1;
}

void teacher_free(teacher **t)
{
    if (*t != nullptr)
    {
        free(*t);
        *t = nullptr;
        cout << "Memory is freed..." << endl;
    }
    else
        return;
}

// 指针引用
void free_teacher(teacher * &t)
{
    if (t != nullptr)
    {
        free(t);
        t = nullptr;
        cout << "Memory is freed..." << endl;
    }
    else
        return;
}

int main()
{
    
    // 动态内存分配
    struct teacher *tp = nullptr;
    tp = get_member(100, "ZhangSan");
    cout << "id: " << tp->id << ", name: " << tp->name << endl;
    free_teacher(tp);

    get_member(&tp);
    cout << "id: " << tp->id << ", name: " << tp->name << endl;
    teacher_free(&tp);
    cout << tp << endl;
    get_member1(tp);
    cout << "id: " << tp->id << ", name: " << tp->name << endl;
    free_teacher(tp);
    cout << tp << endl;

    // 内存开辟在栈上
    teacher teacher1{};
    tp = &teacher1;
    get_member(tp);
    cout << teacher1.id << " " << teacher1.name << endl;

    exit(0);
}
```



## 常量引用

<span style="color:red">非常量可以赋给常量引用，常量不可以赋给非常量引用</span>

**错误**

```cpp
const int a = 10;
int &ref = a;  // 错误 

int &ref = 10;  // 错误
```

**修改**

```cpp
// 引用必须引一块合法的内存空间
const int &ref = 10;  // int temp = 10; const int &ref = temp;
```

**修饰形参防止误操作**

```cpp
void showValue(const int& val)
{
    // 在函数体中，不能对实参进行修改  val = 1000;
    cout << val << endl;
}
```



# 指针



# 函数

## 内联函数

+ 内联函数声明时inline关键字必须和函数定义结合在一起，否则编译器会直接忽略内联请求

+ <span style="color:red">c++编译器直接将函数体插入在函数调用的地方</span>

+ 内联函数没有普通函数调用时的额外开销（压栈，跳转，返回）

+ 内联函数是一种特殊的函数，具有普通函数的特征（参数检查，返回类型等）

+ <span style = "color:#9333FF">内联函数由编译器处理</span>，直接将编译后的函数体插入调用的地方，

  <span style="color:#9333FF">宏代买片段由预处理器处理</span>，进行简单的文本替换，没有任何编译过程

+ c++中内联编译的限制：

  ​	不能存在任何形式的循环语句

  ​	不能存在过多的条件判断语句

  ​	函数体不能过于庞大

  ​	不能对函数进行取值操作

  ​	函数内联声明必须在调用语句之前

+ 编译器对于内联函数的限制并不是绝对的，内联函数相对于普通函数的优势只是省去了函数调用时压栈，跳转和返回的开销。因此<span style="color:#9333FF">当函数体的执行开销源大于压栈，跳转和返回所用的开销时，那么内联函数将无意义。</span>



```cpp
inline printAB(int a, int b);

int main()
{
    
    int a = 10;
    int b = 20;
    
    for (int i = 0; i < 1000; i++)\
    {
        a++;
        b++;
        printAB(a, b);
    }
    
    exit(0);
}

inline printAB(int a, int b);
{
    cout << "a = " << a << ", b = " << b << endl;
}
```



## 默认参数

如果函数声明有默认参数，函数实现就不能有默认参数

```cpp
int fun(int a = 10, b = 20);

int fun(int a, int b)
{
    return 0;
}
```

## 占位参数

占位参数可以有默认参数，（暂时无用，只有亚元使用）

```cpp
void func(int a, int)  // 第二个形参就是占位参数
{
    
}
```

# 函数重载

## 函数重载满足条件

+ 同一个作用域下
+ 函数名称相同
+ 函数参数类型不同， 或者个数不同，  或者顺序不同

**注意**：函数的返回值不可以作为函数重载的条件

```cpp
void func()
{
    cout << "func()" <<endl;
}

void func(int a)
{
	cout << "func(int a)" << endl;
}
```

## 注意事项

1. 引用作为重载的条件

```cpp
void func(int &a)
{
    cout << "func(int &a)" << endl;
}

void func(const int &a)
{
    cout << "func(const int &a)" << endl;
}

int main()
{
    int a = 10;
   	func(a);
    
    func(10);
}
```



2. 函数重载时碰到默认参数

```cpp
int fun(int a, b = 20)
{
    return 0;
}

int fun(int a)
{
    return 0;
}

int main()
{
    func(10);  // 出错
}
```

## 重载底层实现

c++利用name magling (倾轧) 技术，来改变函数名，区分参数不同的同名函数

实现原理：用vcifld表示void char int float long double 及其引用

```cpp
void func(char a);                   // func_c(char a)
void func(char a, int b, double c);  // func_cid(char a, int b, double c)
```



## 函数重载和函数指针

函数指针只能指向确定类型的函数，不能使用重载之后的函数

```cpp
int func(int a, int b);

typedef int(MY_FUNC)(int, int);
typedef int(* MY_FUNC_P)(int, int);

int main()
{
    int a = 10;
    int b = 20;
    // 方法一
    MY_FUNC *f1 = func;
    f1(a, b);
    // 方法二 不建议用，因为使用时看不出是指针，不直观
    MY_FUNC_P f2 = func;
    f2(a, b);
    // 方法三
    int (*f3)(int, int) = func;
    f3(a, b);


    exit(0);
}


int func(int a, int b)
{
    cout << "func" << endl;
    return 0;
}
```





# 类和对象

+ c++面向对象的三大特性为：封装，继承，多态

## 封装

### 封装的意义

+ 将属性和行为作为一个整体，表现生活中的事物
+ 将属性和行为加以权限控制

```cpp
#include <iostream>
#include <string>
using namespace std;

class Student
{
// 公共权限
public:
    // 成员变量
    string m_Name;
    int m_Id;

	// 成员方法
    void showStudent()
    {
        cout << "name: " << m_Name << "m_Id: " << m_Id << endl;
    }

    void setName(string name)
    {
        m_Name = name;
    }
};

int main()
{
    Student s1;
    s1.setName("celery");
    s1.m_Id = 1;
    s1.showStudent();
}
```

### 访问权限

+ 公共权限	public    类内可以访问	类外可以访问
+ 保护权限    protected    类内可以访问    类外不可以访问
+ 私有权限    private    类内可以访问    类外不可以访问

### class 和 struct

**唯一区别**

class 默认权限是私有权限

struct 默认权限是公有权限



## 构造函数



>  对象的初始化和清理是两个非常重要的安全问题
>
>  一个对象或者变量没有初始状态，对其使用后果是未知
>
>  同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题



c++利用了构造函数和析构函数解决上述问题，这两个函数将会被编译器自动调用，完成对象初始化和清理工作。对象的初始化和清理工作是编译器强制要我们做的事情，因此如果我们不提供构造和析构，编译器会提供。

<span style="color:red">**构造函数：**</span>

+ 主要作用在于创建对象时为对象的成员属性赋值

+ 构造函数没有返回值也不写void
+ 函数名称与类名相同
+ 构造函数可以有参数，因此可以发生重载
+ 程序在调用对象的时候会自动调用构造，无须手动调用，而且只会调用一次

### 分类及调用

+ 默认构造函数：只要显示的提供构造函数（显示无参构造，显示有参，显示拷贝构造），默认构造函数都会失效
+ 有参构造函数
+ 拷贝构造函数：默认写法：`Test(const Test & another) {}`       <span style="color:red">参数必须为`const 类名 &`</span>
+ 默认拷贝构造函数：当没有显示的拷贝构造时失效

```cpp
#include <iostream>

using namespace std;


class Person {
public:
    // 默认构造
    Person(){
        cout << "Calling default constructor..." << endl;
    }

    // 有参构造
    Person(int n){
        cout << "Calling parameter constructor..." << endl;
        age = n;
    }

    // 拷贝构造
    Person(const Person &p){
        age = p.age;
        cout << "Calling copy constructor..." << endl;
    }
    int getAge(){
        return age;
    }

private:
    int age;
};


// 调用
int main() {

#if 0
    // 括号法
    Person p1;  // 默认构造不要写()
    Person p2(18);
    Person p3(p2);
#endif

    // 显示法
    Person p1;
    Person p2 = Person(10);
    Person p3 = Person(p2);

    Person(10); // 匿名对象  特点：当前执行结束后，系统会立即回收掉匿名对象

    // 注意事项2
    // 不要利用拷贝构造函数初始化匿名对象  编译器会认为Person (p3)  ===  Person p3;

    // 隐式转换法
    Person p4 = 10;
    Person p5 = p4;
    cout << "p3's age is " << p3.getAge() << endl;
    exit(0);
}
```

### 拷贝构造函数调用时机

```cpp
#include <iostream>
using namespace std;

class Person
{
public:
    Person()
    {
        cout << "Person 默认构造函数调用..." << endl;
    }

    Person(int age)
    {
        m_Age = age;
    }
    Person(const Person &p)
    {
        cout << "Person 拷贝构造函数调用..." << endl;
    }
    int getAge()
    {
        return m_Age;
    }
    ~Person()
    {
        cout << "Person 析构函数调用..." << endl;
    }
private:
    int m_Age;
};

// 使用一个已经创建完毕的对象来初始化一个新对象
void test01()
{
    Person p1(20);
    Person p2(p1);
    cout << "p2的年龄为" << p2.getAge() << endl;
}

// 值传递的方式给函数参数传值
void doWork(Person p) {}

void test02()
{
    Person p;
    doWork(p);
}

// 值方式返回局部对象
Person doWork2()
{
    Person p1;
    return p1;
}
void test03()
{
    Person p = doWork2();
}
```

**细节说明：**

```c++
Person p1;
Person p2(p1); // 调用拷贝构造函数
Person p3 = p2; // 调用拷贝构造函数
Person p4;
p4 = p2; // 并非调用拷贝构造函数，而是操作符重载 （因为这里已经不是初始化，而是赋值）
```



### 构造函数调用规则

+ 创建一个类，c++编译器会给每个类都添加至少三个函数（默认构造函数，析构函数，拷贝构造函数）
+ 如果用户定义有参构造函数，c++不再提供默认无参构造函数，但是会提供默认拷贝构造函数
+ 如果用户定义拷贝构造函数，c++不会再提供其他构造函数

## 析构函数

<span style="color:red">**析构函数：**</span>

+ 主要作用在于对象销毁前系统自动调用，并执行一些清理工作
+ 析构函数没有返回值也不写void
+ 函数名称与类名相同，在名称前加上符号~
+ 析构函数不可以有参数，因此不可以发生重载
+ 程序在对象销毁前会自动调用析构，无需手动调用，而且只会调用一次

+ **默认析构函数：**当有显示析构函数时，默认析构函数失效

```cpp
class Person
{
public:
	Person(){}   
    ~Person(){}
};
```

+ **<span style="color:red">析构函数并不是用来销毁对象的，因为创建一个对象时，空间开辟在栈上，使用完毕后，由操作系统释放。析构函数是用来释放对象开辟在堆上的内存的</span>**

+  返回一个匿名对象，当一个函数返回一个匿名对象的时候，函数外部没有任何变量去接收它，这个匿名对象将不会再被使用，编译器会直接将这个匿名对象会收掉，而不是等待整个函数执行完毕再回收

## 浅拷贝与深拷贝

**浅拷贝**

<img src="heima.assets/image-20230523134950212.png" alt="image-20230523134950212" style="zoom: 67%;" />

```cpp
#include <iostream>
using namespace std;

class Person
{
public:
    Person()
    {
        cout << "Person 默认构造函数调用..." << endl;
    }

    Person(int age, int height)
    {
        m_Age = age;
        m_Height = new int(height);
    }

    int getAge()
    {
        return m_Age;
    }
    int *getHeight()
    {
        return m_Height;
    }
    ~Person()
    {
        // 析构代码，将堆区开辟数据做释放操作
        cout << "Person 析构函数调用..." << endl;
        if(m_Height != NULL)
        {
            delete m_Height;
            m_Height = NULL;
        }
    }

private:
    int m_Age;
    int *m_Height;
};

void test04()
{
    Person p1 = Person(20, 175);
    Person p2 = Person(p1);

    cout << "the age of p1 is: " << p1.getAge() << " the height of p1 is: " << *p1.getHeight() << endl;
    cout << "the age of p2 is: " << p2.getAge() << " the height of p2 is: " << *p2.getHeight()<< endl;
}
int main()
{
    test04();
    exit(0);
}
```

==问题==：重复释放堆区内存。p1为指针，指向的数据存放在堆区；执行浅拷贝后，两个指针指向同一块内存；函数调用结束后，p2先释放，此时p1再释放，为非法操作。



**深拷贝**

<img src="heima.assets/image-20230523135659367.png" alt="image-20230523135659367" style="zoom:67%;" />

```cpp
#include <iostream>
using namespace std;

class Person
{
public:
    Person()
    {
        cout << "Person 默认构造函数调用..." << endl;
    }

    Person(int age, int height)
    {
        m_Age = age;
        m_Height = new int(height);
    }
    Person(const Person &p)
    {
        cout << "Person 拷贝构造函数调用..." << endl;
        m_Age = p.m_Age;
//        m_Height = p.m_Height;    编译器默认实现就是这行代码
        // 深拷贝操作
        m_Height = new int(*p.m_Height);
    }
    int getAge()
    {
        return m_Age;
    }
    int *getHeight()
    {
        return m_Height;
    }
    ~Person()
    {
        // 析构代码，将堆区开辟数据做释放操作
        cout << "Person 析构函数调用..." << endl;
        if(m_Height != NULL)
        {
            delete m_Height;
            m_Height = NULL;
        }
    }

private:
    int m_Age;
    int *m_Height;
};


void test04()
{
    Person p1 = Person(20, 175);
    Person p2 = Person(p1);

    cout << "the age of p1 is: " << p1.getAge() << " the height of p1 is: " << *p1.getHeight() << endl;
    cout << "the age of p2 is: " << p2.getAge() << " the height of p2 is: " << *p2.getHeight()<< endl;
}
int main()
{
    test04();

    exit(0);
}
```

## 初始化列表

对类中的属性进行初始化

```cpp
class Person
{
public:
	Person(int a, int b): m_A(a), m_B(b) {}
private:
    m_A;
    m_B;
};


int main()
{
    Person p(12, 13);
    exit(0);
}
```

## 类对象作为类成员

当其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序与构造相反

## new 和delete

**区别：**

+ malloc free是函数，标准库，stdio.h

+ new在对上初始化一个对象的时候，会触发对象的构造函数，malloc不能
+ free并不能触发一个对象的析构函数



```cpp
// c语言
void test01()
{
    int * p = (int *) malloc(sizeof(int));
    *p = 10;
    free(p);
    p = nullptr;


    // 数组
    int *q;
    q = (int *)malloc(sizeof(int) * 10);
    for (int i = 0; i < 10; i++)
    {
        q[i] = i;
    }

    for (int i = 0; i < 10; i++)
    {
        cout << "q[" << i << "] = " << i << endl;
    }
    free(q);
    q = nullptr;
}

// c++
void test02()
{
    int * p = new int;
    *p = 10;
    delete p;
    p = nullptr;

    // 数组
    int *q;
    q = new int[10];
    for (int i = 0; i < 10; i++)
    {
        q[i] = i;
    }

    for (int i = 0; i < 10; i++)
    {
        cout << "q[" << i << "] = " << i << endl;
    }
    delete [] q;
    q = nullptr;
} 
```





## 静态成员

静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员

静态成员分为：

+ 静态成员变量

  + 所有对象共享同一份数据

  ```cpp
  void test02(){
      // 静态成员变量 不属于某个对象，所有对象都共享同一份数据
      // 因此静态成员变量有两种访问方式
  
      // 1.通过对象进行访问
      Person p;
      cout << p.m_A << endl;
  
      // 2.通过类名进行访问
      cout << Person::m_A << endl;
  
  }
  ```

  + 在编译阶段分配内存，且内存分配在静态区，求类的大小是并不包含在内
  + 类内声明，类外初始化

  ```cpp
  class Person
  {
  public:
      static int m_A;
  };
  
  int Person::m_A = 100;
  ```

+ 静态成员函数
  + 所有对象共享同一个函数
  + <span style="color:red">静态成员函数只能访问静态成员变量</span>

## c++对象模型和this指针

### 成员变量和成员函数分开存储

c++编译器会给每一个空对象也分配一个字节空间，是为了区分空对象占内存的位置

每个空对象也应该有一个个独一无二的内存地址

```c
#include <iostream>
using namespace std;


// 成员变量和成员函数分开存储
class Person
{
    int m_Age; // 非静态成员变量  // 属于类的对象上
    static int m_B; // 静态成员变量  不属于类对象上
    void func() {} // 非静态成员函数  不属于类对象上
    static void func2() {}
};


void test01()
{
    cout << "空对象Person所占字节数：" << sizeof(Person) << endl;
}
int main()
{
    test01();


    exit(0);
}
```

+ c++中成员变量和成员函数是分开存储的
+ 每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会共用一块代码

### this指针

+ c++通过提供特殊的对象指针，this指针，来解决同一个类型对象共用一块代码（这块代码如何区分是那个对象调用的自己）**this指针指向被调用的成员函数所属的对象**
+ <span style="color:red">this指针的本质是指针常量，指向不可以修改</span>

==用途==

+ 当形参和成员变量同名时，可用this指针来区分
+ 在类的非静态成员函数中返回对象本身，可使用`return *this`

```cpp
#include <iostream>
using namespace std;

class Person
{
public:
    Person(int age)
    {
        this->age = age;
    }

    int age;
};

void test01()
{
    Person p1(18);
    cout << "p1的年龄为：" << p1.age;
}

int main()
{
    test01();

    exit(0);
}
```

### 空指针访问成员函数

```cpp
#include <iostream>

using namespace std;

class Person {
public:
    void showClassName() {
        cout << "this is Person class" << endl;
    }

    void showPersonAge() {
        if (this == NULL) {
            return;
        }
        cout << "the age is: " << this->m_Age << endl;
    }

    int m_Age;
};

void test01() {
    Person *p = NULL;

    p->showClassName();
    p->showPersonAge();
}

int main() {
    test01();
    
    exit(0);
}
```

### `const`修饰成员函数

**常函数：**

+ 成员函数后加`const`后我们称这个函数为常函数
+ 常函数内不可以修改成员属性
+ 成员属性声明时加关键字`mutable`后，在常函数中依然可以修改

```cpp
class Person
{
public:
    // this 指针的本质是指针常量，指针的指向是不可以修改的
    // 在成员函数后面加const, 修饰的是this的指向，让指针指向的值也不可以修改
    void showPerson() const
    {
        this -> m_B = 100;
    }
    int m_A;
    mutable int m_B;
};
void test01()
{
    Person p;
    p.showPerson();
}
```



**常对象：**

+ 声明对象前加`const`称该对象为常对象
+ 常对象只能调用常函数

```cpp
#include <iostream>
using namespace std;

class Person
{
public:
    Person(){}
    // this 指针的本质是指针常量，指针的指向是不可以修改的
    // 在成员函数后面加const, 修饰的是this的指向，让指针指向的值也不可以修改
    void showPerson() const
    {
        this -> m_B = 100;
    }
    void func(){}
    int m_A;
    mutable int m_B;
};

void test02()
{
    const Person p;
//    p.m_A = 10;
    p.m_B = 123;  // m_B是特殊值，在常对象下也可以修改
//    p.func();  //  常对象只能调用常函数
}
```

## 友元

### 全局函数做友元

```cpp
#include <iostream>
using namespace std;

class Building
{
    // goodFriend全局函数是Building的好朋友，可以访问Building中的私有成员
    friend void goodFriend(Building *building);
public:
    Building()
    {
        m_SittingRoom = "客厅";
        m_BedRoom = "卧室";
    }

public:
    string m_SittingRoom;
private:
    string m_BedRoom;
};


// 全局函数
void goodFriend(Building *building)
{
    cout << "好朋友全局函数正在访问..." << building->m_SittingRoom << endl;
    cout << "好朋友全局函数正在访问..." << building->m_BedRoom << endl;
}

void test01()
{
    Building building;
    goodFriend(&building);
}

int main()
{
    test01();

    exit(0);
}
```

### 类做友元

```cpp
#include <iostream>
using namespace std;

class Building;

class GoodGay
{
public:
    void visit();  // 参观函数  访问Builing中的属性
    GoodGay();

public:
    Building *building;
};

class Building
{
    // GoodGay类是本类的好朋友，可以访问奔雷中私有成员
    friend class GoodGay;
public:
    Building();
public:
    string m_SittingRoom; // 客厅

private:
    string m_BedRoom;  // 卧室
};

// 类外写成员函数
Building::Building()
{
    m_SittingRoom = "客厅";
    m_BedRoom = "卧室";
}

GoodGay::GoodGay() {
    // 创建建筑物对象
    building = new Building;
}

void GoodGay::visit() {
    cout << "GoodGay is visiting ..." << building->m_SittingRoom << endl;
    cout << "GoodGay is visiting ..." << building->m_BedRoom<< endl;
}

void tesst01()
{
    GoodGay gg;
    gg.visit();
}

int main()
{
    tesst01();

    exit(0);
}
```

### 成员函数作为友元

```cpp
#include <iostream>
using namespace std;

class Building;

class GoodGay
{
public:
    GoodGay();

    void visit();
    void visit2();

    Building *building;
};

class Building
{
    friend void GoodGay::visit();
public:
    Building();
public:
    string m_SittingRoom;
private:
    string m_BedRoom;

};
Building::Building() {
    m_SittingRoom = "客厅";
    m_BedRoom = "卧室";
}
GoodGay::GoodGay() {
    building = new Building;
}

void GoodGay::visit() {
    cout << "visit function is visiting :" << building->m_SittingRoom << endl;
    cout << "visit function is visiting :" << building->m_BedRoom << endl;
}
void GoodGay::visit2() {
    cout << "visit function is visiting :" << building->m_SittingRoom << endl;
}

void test01(){
    GoodGay gg;
    gg.visit();
    gg.visit2();
}

int main()
{
    test01();

    exit(0);
}
```

## 操作符重载

### 加号运算符重载

```cpp
class Person
{
public:
    int m_A;
    int m_B;

    //成员函数运算符重载
//    Person operator+(Person &p)
//    {
//        Person temp;
//        temp.m_A = this->m_A + p.m_A;
//        temp.m_B = this->m_B + p.m_B;
//        return temp;
//    }
};


// 全局函数重载
Person operator+(Person &p1, Person &p2)
{
    Person temp;
    temp.m_A = p1.m_A + p2.m_A;
    temp.m_B = p1.m_B + p2.m_B;
    return temp;
}

// 运算符重载也可以发生函数重载
Person operator+(Person &p, int num){
    Person temp;
    temp.m_A = p.m_A + num;
    temp.m_B = p.m_B + num;
    return temp;
}

```

### 左移运算符重载

```cpp
#include <iostream>
using namespace std;

class Person
{
public:
    int m_A;
    int m_B;

    // 左移运算符不能通过成员函数重载，重载结果为 p << cout
};

ostream &operator<<(ostream &cout, Person &p)
{
    cout << "p.m_A = " << p.m_A << " p.m_B = " << p.m_B;
    return cout;
}

void test01()
{
    Person p{};
    p.m_A = 5;
    p.m_B = 6;
    cout << p << endl;
}

int main()
{
    test01();

    exit(0);
}
```

### 仿函数

+ 重载（）运算符
+ 由于重载后使用的方式非常像函数的调用，因此称为仿函数
+ 仿函数没有固定的写法，非常灵活

```cpp
#include <iostream>
using namespace std;

class MyPrint{

public:
    void operator()(string test)
    {
        cout << test << endl;
    }

};

void test01()
{
    MyPrint myprint;
    myprint("hello world");
    // 匿名函数对象
    MyPrint()("Good");
}

int main()
{
    test01();

    exit(0);
}
```

## 继承

### 继承基本语法

```cpp
#include <iostream>
using namespace std;

class BasePage
{
public:
    void header()
    {
        cout << "首页、公开课、登录、注册...（公共头部）" << endl;
    }

    void footer()
    {
        cout << "帮助中心、交流合作、站内地图...（公共底部）" << endl;
    }

    void left()
    {
        cout << "Java, Python, C++...（公共分类列表）" << endl;
    }
};

class Java : public BasePage
{
public:
    void content()
    {
        cout << "Java学科视频" << endl;
    }
};

class Python : public BasePage
{
public:
    void content()
    {
        cout << "Python学科视频" << endl;
    }
};

void test01()
{
    Java ja;
    cout << "java页面信息" << endl;
    ja.header();
    ja.footer();
    ja.left();
    ja.content();
}

void test02()
{
    Python py;
    cout << "Python页面信息" << endl;
    py.header();
    py.footer();
    py.left();
    py.content();
}

int main()
{
    test01();
    cout << "-----------" << endl;
    test02();
    exit(0);
}
```

### 继承方式

![image-20230529150818353](heima.assets/image-20230529150818353.png)

### 构造和析构的顺序

![image-20230531103025717](heima.assets/image-20230531103025717.png)

### 继承同名成员处理方式

+ 子类对象可以直接访问到子类中同名成员
+ 子类对象加作用域可以访问到父类同名成员
+ 当子类与父类拥有同名成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类中同名函数

```cpp
#include <iostream>
using namespace std;

class Base
{
public:
    void func()
    {
        cout << "Base-func 调用..." << endl;
    }
    void func(int a)
    {
        cout << "Base-func(int a) 调用..." << endl;
    }
};


class Son : public Base
{
public:
    void func()
    {
        cout << "Son-func 调用..." << endl;
    }
};

void test01()
{
    s1.func();

    // 如果子类中出现和父类同名的成员函数，子类的同名成员会隐藏掉父类中所有同名成员函数
    // 如果想访问父类中被隐藏的同名函数，应该加作用域
    s1.Base::func(100);
    s1.Base::func();
}

int main()
{
    test01();

    exit(0);
}
```

### 同名静态成员处理

```cpp
#include <iostream>

using namespace std;

class Base {
public:
    static int m_A;

    static void func() {
        cout << "function of Base recalled..." << endl;
    }
};

int Base::m_A = 100;

class Son : public Base {
public:
    static int m_A;

    static void func() {
        cout << "function of Son recalled..." << endl;
    }
};

int Son::m_A = 200;

int test01() {
    Son s;
    cout << "visiting by object..." << endl;
    cout << "s.m_S = " << s.m_A << endl;
    cout << "s.Base::m_A = " << s.Base::m_A << endl;
    cout << endl;
    cout << "visiting by class..." << endl;
    cout << "Son::m_A = " << Son::m_A << endl;
    cout << "Base::m_A = " << Son::Base::m_A << endl;
    return 0;
}

int main() {
    test01();

    exit(0);
}
```

### 多继承

语法：class 子类 ：继承方式  父类1， 继承方式  父类2

### 菱形继承

继承前加`virtual`关键字后，变为<span style="color:red">虚继承</span>, 使相同的数据只保留一份

```cpp
#include <iostream>
using namespace std;

class Animal
{
public:
    int m_Age;
};

class Sheep : virtual public Animal {};

class Tuo : virtual public Animal {};

class SheepTuo : public Sheep, public Tuo {};

void test01()
{
    SheepTuo st;
    st.m_Age = 18;
    cout << "st.Sheep::m_Age = " << st.Sheep::m_Age << endl;
    cout << "st.Tuo::m_Age = " << st.Tuo::m_Age << endl;
}

int main()
{
    test01();

    exit(0);
}
```

## 多态

### 多态的基本概念

**多态是c++面向对象三大特征之一**

多态分为两类

+ 静态多态：函数重载和运算符重载数据静态多态，复用函数名
+ 动态多态：派生类和虚函数实现运行时多态

静态多态和动态多态的区别

+ 静态多态的函数地址早绑定 - 编译阶段确定函数地址
+ 动态多态的函数地址晚绑定 - 运行阶段确定函数地址

```cpp
#include <iostream>

class Animal
{
public:
    // 虚函数
    virtual void speak()
    {
        std::cout << "Animal is speaking." << std::endl;
    }
};

class Cat : public Animal
{
public:
    // 重写
    void speak() override
    {
        std::cout << "Cat is speaking." << std::endl;
    }
};

// 地址早绑定，地址在编译阶段确定
// 动态多态满足条件
// 1.有继承关系
// 2.子类重写父类的虚函数

// 动态多态使用
// 父类指针或者引用 指向子类对象
void doSpeak(Animal &animal)
{
    animal.speak();
}
void test01()
{
    Cat cat;
    doSpeak(cat);
}

int main()
{
//    using namespace std;
    test01();

    exit(0);
}
```

### 多态的原理剖析

<img src="heima.assets/image-20230603221545638.png" alt="image-20230603221545638" style="zoom: 80%;" />

**重写**

<img src="heima.assets/image-20230603221708514.png" alt="image-20230603221708514" style="zoom:80%;" />

### 多态实现计算机

<span style = "color:red">对扩展进行开放，对修改进行关闭</span>

```cpp
class AbstractCalculator
{
public:
    virtual int getResult()
    {
        return 0;
    }

    int m_Num1{};
    int m_Num2{};
};

class AddCalculator : public AbstractCalculator
{
public:
    int getResult() override
    {
        return m_Num1 + m_Num2;
    }
};

class SubCalculator : public AbstractCalculator
{
public:
    int getResult() override
    {
        return m_Num1 - m_Num2;
    }
};
class MulCalculator : public AbstractCalculator
{
public:
    int getResult() override
    {
        return m_Num1 * m_Num2;
    }
};

void test01()
{
    AbstractCalculator *abc = new AddCalculator;
    abc->m_Num1 = 15;
    abc->m_Num2 = 5;
    cout << abc->m_Num1 << "+" << abc->m_Num2 << "=" << abc->getResult() << endl;
    delete abc;
    abc = new SubCalculator;
    abc->m_Num1 = 15;
    abc->m_Num2 = 5;
    cout << abc->m_Num1 << "-" << abc->m_Num2 << "=" << abc->getResult() << endl;
    delete abc;
}
int main()
{
    test01();
    exit(0);
}
```

### 纯虚函数和抽象类

<span style="color:red">纯虚函数语法</span>：`virtual 返回值类型 函数名 （参数列表）= 0`

当类中有了纯虚函数，这个类也称为抽象类

**抽象类特点**：

+ 无法实例化对象
+ 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```cpp
#include <iostream>
using namespace std;

class AbstractDrinking
{
public:
    // 煮水
    virtual void boil() = 0;
    // 冲泡
    virtual void brew() = 0;
    // 倒入杯中
    virtual void pourInCup() = 0;
    // 加入辅料
    virtual void putSomething() = 0;

    void makeDrinking()
    {
        boil();
        brew();
        pourInCup();
        putSomething();
    }
};

class Tea : public AbstractDrinking
{
public:
    // 煮水
    void boil() override
    {
        cout << "boil water..." << endl;
    }
    // 冲泡
    void brew() override
    {
        cout << "brew tea..." << endl;
    }
    // 倒入杯中
    void pourInCup() override
    {
        cout << "pour in the cup..." << endl;
    }
    // 加入辅料
    void putSomething() override
    {
        cout << "put some lemon..." << endl;
    }
};

class Coffee : public AbstractDrinking
{
public:
    // 煮水
    void boil() override
    {
        cout << "boil water..." << endl;
    }
    // 冲泡
    void brew() override
    {
        cout << "brew coffee..." << endl;
    }
    // 倒入杯中
    void pourInCup() override
    {
        cout << "pour in the cup..." << endl;
    }
    // 加入辅料
    void putSomething() override
    {
        cout << "put milk and sugar..." << endl;
    }
};
void doWork(AbstractDrinking *ad)
{
    ad->makeDrinking();
    delete ad;
}

void test01()
{
    doWork(new Coffee);
    cout << "-------------" << endl;
    doWork((new Tea));
}
```

### 虚析构和纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方式：将父类中的析构函数改为**虚析构**或者**纯虚析构**



虚析构和纯虚析构共性：

+ 都可以解决父类指针释放子类对象
+ 都需要有具体的函数实现

虚析构和纯虚析构区别：

+ 如果时纯虚析构，该类属于抽象类，无法实例化对象



**虚析构**

```cpp
#include <iostream>
using namespace std;

class Animal
{
public:
    Animal()
    {
        cout << "Animal's constructor is called..." << endl;
    }
    // 利用虚析构可以解决父类指针释放对象时不干净的问题
    virtual ~Animal()
    {
        cout << "Animal's destructor is called..." << endl;
    }

    // 纯虚函数
    virtual void speak() = 0;
};

class Cat : public Animal
{
public:
    explicit Cat(string name)
    {
        m_Name = new string(std::move(name));
    }
    void speak() override
    {
        cout << *m_Name << " Cat says miao miao miao" << endl;
    }
    ~Cat() override
    {
        if(m_Name != nullptr)
        {
            cout << "Cat's destructor is called..." << endl;
            delete m_Name;
            m_Name = nullptr;
        }
    }

    string *m_Name;
};

void test01()
{
    Animal *animal = new Cat("Tom");
    animal->speak();
    delete animal;
}
```

**纯虚析构**

如果纯虚析构只写了声明没写实现，则会报（无法解析的外部命令错误 [链接阶段的错误]）

```cpp
#include <iostream>
using namespace std;

class Animal
{
public:
    Animal()
    {
        cout << "Animal's constructor is called..." << endl;
    }

    // 纯虚析构
    // 有了纯虚析构之后，这个类也属于抽象类，无法实例化对象
    virtual ~Animal() = 0;

    // 纯虚函数
    virtual void speak() = 0;
};
Animal::~Animal()
{
    cout << "Animal trivial destructor is called..." << endl;
}


class Cat : public Animal
{
public:
    explicit Cat(string name)
    {
        m_Name = new string(std::move(name));
    }
    void speak() override
    {
        cout << *m_Name << " Cat says miao miao miao" << endl;
    }
    ~Cat() override
    {
        if(m_Name != nullptr)
        {
            cout << "Cat's destructor is called..." << endl;
            delete m_Name;
            m_Name = nullptr;
        }
    }

    string *m_Name;
};

void test01()
{
    Animal *animal = new Cat("Tom");
    animal->speak();
    delete animal;
}
```

# 文件操作

![image-20230606093539525](heima.assets/image-20230606093539525.png)

## 文本文件

### 写文件

写文件步骤：

1. 包含头文件
2. 创建流对象
3. 打开文件
4. 写数据
5. 关闭文件



文件打开方式

![image-20230606112745980](heima.assets/image-20230606112745980.png)

```cpp
// 1. 包含头文件
#include <fstream>

// 文本文件 写文件

void test01()
{
    // 2. 创建输出流对象
    ofstream ofs;
    // 3. 打开文件
    ofs.open("./test.txt", ios::out);
    // 4. 写入数据
    ofs << "name: lili" << endl;
    ofs << "性别： 女" << endl;
    ofs << "age: 18" << endl;

    // 5. 关闭文件
    ofs.close();
}
```

### 写文件

读文件步骤：

1. 包含头文件
2. 创建流对象
3. 打开文件并判断文件是否打开成功
4. 读数据
5. 关闭文件

```cpp
#include <fstream>

void test01()
{
    ifstream ifs;
    ifs.open("./test.txt", ios::in);

    if (!ifs.is_open())
    {
        cout << "file open failed!" << endl;
        return;
    }

    // 4.读数据
#if 0
    // 方法一
    char buf[1024] = {0};
    while (ifs >> buf)
    {
        cout << buf << endl;
    }
	// 方法二
    char buf[1024] = { 0 };
    while (ifs.getline(buf, sizeof buf))
    {
        cout << buf << endl;
    }
	// 方法三
    string buf;
    while(getline(ifs, buf))
    {
        cout << buf << endl;
    }
#endif
	// 方法四
    char c;
    while ( (c = char(ifs.get())) != EOF)
    {
        cout << c;
    }

    ifs.close();

}
```

## 二进制文件

以二进制的方式进行读写操作

打开方式要指定为`ios::binary`

### 写文件

二进制方式写文件主要利用流对象调用成员函数write

函数原型：`ostream& write(const char *buffer, int len);`

参数解释：字符指针buffer指向内存中一段存储空间，len是读写字节数

```cpp
#include <fstream>
using namespace std;

#include <fstream>
#include <string>

class Person
{
public:
    char m_Name[64];
    int m_age;
};

void test01()
{
    // 创建输出流对象
    ofstream ofs("./person.txt", ios::out | ios::binary);

    Person p = {"zhangsan", 28};


    ofs.write((const char *)&p, sizeof p);
}
```

### 读文件

二进制方式读文件主要利用流对象调用成员函数read

函数原型：`istream& read(char *buffer, int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写字节数

```cpp
#include <iostream>
using namespace std;
#include <fstream>

class Person{
public:

    char m_Name[64];
    int m_Age;
};

void test01()
{
    ifstream  ifs;
    ifs.open("person.txt", ios::in | ios::binary);
    if (!ifs.is_open())
    {
        cout << "the file open failed!";
        return ;
    }

    Person p;
    ifs.read((char *)&p, sizeof(Person));

    cout << "name: " << p.m_Name << " age: " << p.m_Age << endl;

    ifs.close();

}
```

6