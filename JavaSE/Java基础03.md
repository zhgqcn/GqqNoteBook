---
title: Java基础-day03
date: 2020-02-08 14:41:22
categories: JavaSE
tags: Java
id: Gqq
---

# 表达式求值以及操作符优先级

Java表达式的求值和数学表达式求值一样

- 有括号先算括号里面
- 乘除取余先算
- 再算加减
- 同级计算从左到右

<!--more-->

题目：`华氏度转换为摄氏度`

```java
import java.util.Scanner;
public class FahrenheitToCelsius {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.print("输入华氏度：");
        double f = input.nextDouble();
        double c = (5.0 / 9) * (f - 32);
        System.out.println("摄氏度为：" + c);
    }
}
```

输出：

```shell
输入华氏度：100
摄氏度为：37.77777777777778
```

> 注意：5 / 9 的结果为0，应当为 5.0/9



# 显示当前时间

> 通过调用`System.currentTimeMillis()`返回当前时间
>
> `Sysytem.currentTimeMillis`返回自UNIX时间戳以来的毫秒数

![Snipaste_2020-02-08_15-05-42](//tva2.sinaimg.cn/large/005tpOh1gy1gbp0b5ip1sj30sw05uwfs.jpg)

代码：

```java
public class ShowCurrentTime {
    public static void main(String[] args) {
        long totalMilliseconds = System.currentTimeMillis(); //总毫秒数
        long totalSeconds = totalMilliseconds / 1000;		 //总秒数
        long currentSecond = totalSeconds % 60;
        long totalMinutes = totalSeconds / 60;
        long currentMinute = totalMinutes % 60;
        long totalHours = totalMinutes / 60;
        long currentHour = totalHours % 24;

        System.out.println("当前GMT时间为" + currentHour + ":" +
                currentMinute + ":" + currentSecond);
    }
}
```

> 核对下时间：[GMT](https://time.is/zh/)   
>
> 注意数据类型应该为：`long`



# 增强赋值操作符

| 操作符 |      名称      |
| :----: | :------------: |
|   +=   | 加法赋值操作符 |
|   -=   | 减法赋值操作符 |
|   *=   | 乘法赋值操作符 |
|   /=   | 除法赋值操作符 |
|   %=   | 求余赋值操作符 |

如：  

$$x /= 4 + 5.5 * 1.5$$ 等同于 $$x = x / (4 + 5.5 * 1.5)$$

# 自增和自减操作符

| 操作符 |      名称      |                说明                 |
| :----: | :------------: | :---------------------------------: |
| ++var  | 前置自增操作符 | 变量var的值加1且使用var增加后的新值 |
| var++  | 后置自增操作符 |   变量var的值加1但使用var原来的值   |
| --var  | 前置自减操作符 | 变量var的值减1且使用var增加后的新值 |
| var--  | 后置自减操作符 |   变量var的值减1但使用var原来的值   |



# 数值类型转换

通过显式转换，浮点数可以被转换为整数  

```shell
3 * 4.5 => 3.0 * 4.5
```

总是可以将一个数值赋给支持更大数据范围类型的变量，如：可以将`long`型的值赋给`float`型的变量  

但是，若不进行**类型转换**，就不能将一个值赋给范围较小类型的变量。  

**拓宽类型：**将一个小范围类型的变量转换为大范围类型的变量  

**缩窄类型：**把大范围类型的变量转换为小范围类型的变量  

> Java将自动拓展一个类型，但是，缩窄类型必须*显式*完成。

显式类型转换：

```java
System.out.println((double)1 / 2);   // 结果为0.5
System.out.println((int)1.7);		 // 结果为1
```

**注意：** 

1. 类型转换不会改变被转换的变量

```java
double b = 4.5;
int i = (int)b; // i = 4; b 仍然为 4.5
```

2. Java中， $x1 op= x2$形式的增强赋值表达式，执行为 $x1 = (T)(x1 op x2)$，这里T是x1的类型。如：

```java
int sum = 0;
sum += 4.5;  //sum = 4
// sum += 4.5 等价于 sum = (int)(sum + 4.5)
```

3. 将一个`int`型变量赋值给`short`型或者`byte`型变量，必须进行显式转换。

```java
int i = 1;
byte b = i;  // 编译错误
byte c = (byte)i; // 正确
```

然而，只要整型直接量是在目标范围允许内的，那么将整型直接量赋给`short`型或`byte`型变量时，就不需要显示的类型变换。



