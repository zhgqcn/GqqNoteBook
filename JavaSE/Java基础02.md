---
title: Java基础-day02
date: 2020-02-07 
categories: JavaSE
tags: Java
id: Gqq
---

## 代码
```java
import java.util.Scanner;
public class ComputerArea{
    public static void main(String[] args) {
        final double PI = 3.14159;
        Scanner input = new Scanner(System.in);
        System.out.print("输入半径：");
        double radius = input.nextDouble();
        double area = radius * radius * PI;
        System.out.println("半径为" + radius + "的圆形的面积为" + area);
    }
}
```

<!--more-->

### import导入相应的包

> 可以导入所有包，也可导入指定包  
>
> 如：

```java
import java.util.Scanner;   // 只导入Scanner包
import java.util.*;         // 导入全部
```

### final定义常量
```
final 数据类型 变量名 = 初始值
```


### 键盘输入
> 利用Scanner对象，步骤如下：  

- 导入Scanner包  
- new创建Scanner对象  
- 输入  

### 输出
利用`+`号连接输出字符串，冒号之间不可阶段输出，如：
```java
System.out.println("我" + "喜欢" + "吃烤
山药");                                     // wrong
System.out.println("我" + "喜欢" + 
"吃烤山药");                                // right
```

### `println`与`print`的区别
```java
print       // 输出不换行  
println     // 输出换一行
```



### 命名习惯

1. 变量命名
    * 字母
    * 下划线
    * 美元符号$
    * 数字
只有前三个可以作为开头使用，区分大小写
2. 使用小写字母命名`变量`和`方法`。如：
    * 变量radius和area
    * 方法print
3. `类名`中的每个单词的首字母大写。如：
    * 类名ComputerArea和System
4. 大写`常量`中的所有字母，连两个单词间用下划线连接。如：
	* 常量PI
	* 常量MAX_VALUE

##  数值数据类型和操作

### 数值类型

#### 整型

| 类型名 |   存储大小   |
| :----: | :----------: |
|  byte  | 8位带符号数  |
| short  | 16位带符号数 |
|  int   | 32位带符号数 |
|  long  | 64位带符号数 |

#### 浮点型

| 类型名 |     存储大小      |
| :----: | :---------------: |
| float  | 32位，标准IEEE754 |
| double | 64位，标准IEEE754 |

#### 数值操作符

`+` `-` `×` `/` `%` 即 加减乘数取余  

求余应用，计算 x 秒 = y 分 z 秒（z < 60)  

```java
import java.util.Scanner;
public class DisplayTime {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.print("Input time :");
        int times = input.nextInt();

        int seconds = times / 60;
        int minutes = times % 60;

        System.out.println(times + "秒 = " + seconds + "分" +
                minutes + "秒");
    }
}
```

#### 幂运算

使用方法`Math.pow(a, b)` 来计算$a^b$。

### 直接量

#### 数值型直接量

```java
int nuberOfYears = 34;
double weight = 0.322;
```

#### 整型直接量

```java
System.out.println(0B1111); // 显示15 	二进制
System.out.println(07777);	// 显示4095 	八进制
System.out.println(0XFFFF);	// 显示65535	十六进制
```

`long`型的整型直接量，后面要加`L`后缀

#### 浮点型直接量

浮点型直接量带小数点，默认情况下是double型的。  

如：5.0是double类型的，若要表示为float类型，则后面加`F` 为`5.0F`

#### 科学记数法

浮点可用$a×10^b$形式的科学记数法表示。  

如：123.456 = $1.23456 × 10^2$ = 1.23456E2  

Java中为了提高可读性，允许数值直接量的两个数字间使用下划线。  

```java
long ssn = 234_56_789;
long tel = 1575_972_9922;
```







