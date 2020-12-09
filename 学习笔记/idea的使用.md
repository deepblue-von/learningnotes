### idea的使用
创建项目-创建模块-创建包（在scr下创建）-类（代码）
**删除模块**  在idea中只能删除模块的记录，要想彻底删除模块要进入到项目的文件夹中进行删除
**删除项目**  File-close
**新建项目 ** create a new project

#### 常用快捷键
Alt + /   功能辅助键（代码提示）配置快捷键 file-setting-code
sout  快速生成打印语句
psvm  快速生成主方法
Ctrl + /  快速添加单行注释B
Ctrl + shift + /  快速添加多行注释
Ctrl + Alt + L  格式化代码
Ctrl + D  快速复制代码
Alt + enter  导包
Alt + F12  打开terminal
shift + F6  修改名称（类名、包名、文件名、变//量名）

#### 运算符

三目运算符
变量 = 关系表达式？表达式A:表达式B

#### 键盘录入

```java
//1.导包（必须写在class类的上面）
import java.util.Scanner;

public class Demo1{
    public static void main(String[] args){
        //2.创建Scanner对象 //对象就是具备一定功能的东西
        Scanner sc = new Scanner(Sytem.in)//固定格式
        //3.使用Scanner键盘录入整数的功能
        int num = sc.nextInt();//阻塞等待你键盘录入
        double b = sc.nextDouble();
        System.out.println(num);
    }
}
```
