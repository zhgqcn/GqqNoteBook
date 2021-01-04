---
title: Java基础-day06
date: 2020-02-11
categories: JavaSE
tags: Java
id: Gqq
---

# 格式化控制台输出 

可以使用 `System.out.printf `方法在控制台上显示格式化输出   

```java
public class test {
    public static void main(String[] args) {
        double amount = 12618.98;
        double interestRate = 0.0013;
        double interest = amount * interestRate;
        System.out.printf("Interest is $%4.2f", interest);
    }
}
```

> 显示：Interest is $16.40
>
> >  注意：%不能漏掉，4是域宽度，2是精度，f是格式标识符 <!--more-->

![Snipaste_2020-02-11_13-49-47](//tva1.sinaimg.cn/large/005tpOh1gy1gbsg3mwx7wj307t03sglw.jpg)

```java
System.out.printf(format, item1, item2, ......, itmek);
```

常用的格式标识符

| 标识符 |          输出          |     举例      |
| :----: | :--------------------: | :-----------: |
|   %b   |         布尔值         | true或者false |
|   %c   |          字符          |      ‘a'      |
|   %d   |       十进制整数       |      200      |
|   %f   |         浮点数         |   46.460000   |
|   %e   | 标准科学记数法形式的数 | 4.556000e+01  |
|   %s   |         字符串         | “Java is fun" |

> 默认情况下，浮点值显示小数点后6位

```java
System.out.printf("%8d%8s%8.1f\n", 1234,"Java", 5.63); 
System.out.printf("%-8d%-8s%-8.1f \n", 1234, "Java", 5.63);
```

![Snipaste_2020-02-11_14-34-31](//tva1.sinaimg.cn/large/005tpOh1gy1gbsg9b3wccj30ae02bjrs.jpg)

**FormatDemo.java**

```java
public class test {
    public static void main(String[] args) {
        System.out.printf("%-10s%-10s%-10s%-10s%-10s\n", "Degree", "Radians", "Sine",
        "Cosine", "Tangent");
        int degrees = 30;
        double randians = Math.toRadians(degrees);
        System.out.printf("%-10d%-10.4f%-10.4f%-10.4f%-10.4f", degrees, randians, Math.sin(randians),
                Math.cos(randians), Math.tan(randians));
    }
}

```

![Snipaste_2020-02-11_14-41-36](//tvax4.sinaimg.cn/large/005tpOh1gy1gbsggprzjtj30gq02j746.jpg)



# 第四章编程练习题

`4.2 几何：最大圆距离`

```java
import java.util.Scanner;
public class CircleDistansce {
    public static void main(String[] args) {
        System.out.print("Enter point 1 (latitude and longitude) in degrees:");
        Scanner input = new Scanner(System.in);
        double x1 = input.nextDouble();
        double y1 = input.nextDouble();
        System.out.print("Enter point 2 (latitude and longitude) in degrees:");
        double x2 = input.nextDouble();
        double y2 = input.nextDouble();

        double earthRadius = 6371.01;           // 记得转换为弧度制 Math.toRadians()
        double d = earthRadius * Math.acos(Math.sin(Math.toRadians(x1)) * Math.sin(Math.toRadians(x2)) +
                Math.cos(Math.toRadians(x1)) * Math.cos(Math.toRadians(x2)) * Math.cos(Math.toRadians(y2 - y1)));

        System.out.print("The distance between the 2 points is:" + d);
    }
}
```

> Enter point 1 (latitude and longitude) in degrees: 39.55 -116.25
> Enter point 2 (latitude and longitude) in degrees: 41.5 87.37
> The distance between the 2 points is: 10691.79183231593

`4.2 圆上面的随机点`

```java
import java.util.Scanner;
public class test {
    public static void main(String[] args) {
        final double r = 40;
        double alpha1 = Math.random() * 400;
        double x1 = r * Math.cos(Math.toRadians(alpha1));
        double y1 = r * Math.sin(Math.toRadians(alpha1));
        double alpha2 = Math.random() * 400;
        double x2 = r * Math.cos(Math.toRadians(alpha2));
        double y2 = r * Math.sin(Math.toRadians(alpha2));
        double alpha3 = Math.random() * 400;
        double x3 = r * Math.cos(Math.toRadians(alpha3));
        double y3 = r * Math.sin(Math.toRadians(alpha3));

        double line1 = Math.sqrt(Math.pow(x1 - x2, 2) + Math.pow(y1 - y2, 2));
        double line2 = Math.sqrt(Math.pow(x1 - x3, 2) + Math.pow(y1 - y3, 2));
        double line3 = Math.sqrt(Math.pow(x2 - x3, 2) + Math.pow(y2 - y3, 2));

        double angleA = Math.toDegrees(Math.acos((line1 * line1 - line2 * line2 - line3 * line3) / (-2 * line2 * line3)));
        double angleB = Math.toDegrees(Math.acos((line2 * line2 - line1 * line1 - line3 * line3) / (-2 * line3 * line1)));
        double angleC = Math.toDegrees(Math.acos((line3 * line3 - line2 * line2 - line1 * line1) / (-2 * line2 * line1)));

        System.out.printf("3个角度分别为：%10.2f%10.2f%10.2f", Math.round(angleA * 100) / 100.0,
                Math.round(angleB * 100) / 100.0, Math.round(angleC * 100) / 100.0);
    }
}

```

> 3个角度分别为：    129.50     15.14     35.36





