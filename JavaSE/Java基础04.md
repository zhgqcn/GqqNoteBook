---
title: Java基础-day04
date: 2020-02-09
categories: JavaSE
tags: Java
id: Gqq
---

# 软件开发过程

软件开发生命周期是一个多阶段的过程，包括需求规范、分析、设计、实现、 测试、部署和维护

- 确定输出
- 决定输入
- 将问题分解为可管理的组成部分
- 系统分析和设计的本质是输入、处理、输出（`IPO`)

<!--more-->

![Snipaste_2020-02-08_17-54-22](http://tva3.sinaimg.cn/large/005tpOh1gy1gbp56kc074j30mx0bemzs.jpg)

# 常见错误和陷阱

###### 常见错误1：未声明、未初始化的变量和未使用的变量

###### 常见错误2：整数溢出

###### 常见错误3： 取整错误

###### 常见错误4：超出预期的整数除法

###### 常见陷阱：冗余的输入对象



# 小练习

**驾驶费用**

```java
import java.util.Scanner;
public class DrivePrice {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.print("输入驾驶里程：");
        double drivedistance = input.nextDouble();

        System.out.print("输入每加仑汽油的行驶距离：");
        double milesPergallon = input.nextDouble();

        System.out.print("输入每加仑汽油的价格：");
        double pricePergallon = input.nextDouble();

        double costs = drivedistance / milesPergallon * pricePergallon;

        System.out.println("需要油费：" + new java.text.DecimalFormat("0.00").format(costs));
    }
}
```

> 输出：
>
> 输入驾驶里程：900.5
> 输入每加仑汽油的行驶距离：25.5
> 输入每加仑汽油的价格：3.55
> 需要油费：125.36



**boolean练习**

```java
import java.util.Scanner;

public class AdditionQuiz {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int number1 = (int)(System.currentTimeMillis() % 10);       // 利用时间GMT产生随机数
        int number2 = (int)(System.currentTimeMillis() / 7 % 10);

        System.out.print("What is " + number1 + " + " + number2 + " ?");

        int answer = input.nextInt();

        System.out.println(number1 + "+" + number2 + "=" + answer + " is 1" +
                (number1 + number2 == answer));
    }
}
```

> 利用`System.currentTimeMillis()`产生相关的随机数



# 产生随机数

> 可以使用`Math.random()`来获得一个`0.0`到`1.0`之间的随机`double`值，不包括`1.0`

```java
import java.util.Scanner;
public class SubtractionQuiz {
    public static void main(String[] args) {
        int number1 = (int)(Math.random() * 10); // 产生随机数1
        int number2 = (int)(Math.random() * 10); // 产生随机数2

        if(number1 < number2){					 // 保证结果是整数
            int temp = number1;
            number1 = number2;
            number2 = temp;
        }

        System.out.print("What is " + number1 + " - " + number2 + " ? ");
        Scanner input = new Scanner(System.in);
        int answer = input.nextInt();

        if(number1 - number2 == answer){
            System.out.println("Your answer is right");
        }else{
            System.out.println("Your answer is wrong\n" + number1 + " - " +
                    number2 + " = " + (number1 - number2));
        }
    }
}
```

> 输出：
>
> What is 9 - 2 ? 2
> Your answer is wrong
> 9 - 2 = 7



# 逻辑运算符

| 操作符 | 名称 |   说明   |
| :----: | :--: | :------: |
|   ！   |  非  |  逻辑非  |
|   &&   |  与  |  逻辑与  |
|  \|\|  |  或  |  逻辑或  |
|   ^    | 异或 | 逻辑异或 |

**德模佛定理**  

$$!(condition1 && condition2)$$ 等价于 $$!condition1 || !condition2$$

---

$$!(condition1 || condition2)$$ 等价于 $$!condition1 && !condition2$$



**判断闰年**

- 可以被4整除但不能被100整除
- 可以被400整除

```
boolean isLeapYear = (year % 4 == 0);				// 可以被4整除
isLeapYear = isLeapYear && (year % 100 != 0);		// 不可以被100整除
isLeapYear = isLeapYear || (year % 400 == 0);		// 可以被400整除

isLeapYear = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0); // 整合
```



# 条件表达式

`(布尔表达式 ？ 表达式1 ： 表达式2)`

当表达式为`True` ，返回表达式1  

当表达式为`False`，返回表达式2



# 操作符优先级

![Snipaste_2020-02-09_15-58-38](//tva1.sinaimg.cn/large/005tpOh1gy1gbq7jetrq4j30hn0idq81.jpg)

