---
title: Java基础-day08
date: 2020-02-18
categories: JavaSE
tags: Java
id: Gqq
---

# 方法

> 方法即函数

## 定义方法

```java
修饰符 返回值类型 方法名（参数列表）{
    // 方法体
}
```

<!--more-->

如：

```java
public static int max(int num1, int num2){
    int result;
    if(num1 > num2){
        result = num1;
    }else{
        result = num2;
    }
    return result;
}
```

**注意：**

1. `void`没有返回值，但是可以用`return;`来终止方法的执行。

2. 在该类中调用其他类的方法，通过`类名.方法名`来调用
3. 注意参数要`顺序匹配`，实参必须与方法签名中定义的参数在次序和数量上匹配，在类型上兼容
4. `重载方法`使得可以使用同样的名字来定义不同方法



## 实战

`假设要编写一个程序，显示给定年月的日历。程序提示用户输入年份和月份，然后显示该月的整个日历`

```java
import java.util.Scanner;
public class PrintCalendar {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter full year:");
        int year = input.nextInt();
        System.out.print("Enter month as number:");
        int month = input.nextInt();
        printHeader(year, month);
        printBody(year, month);
    }
    // 打印日历表头
    public static void printHeader(int y, int m) {
        System.out.println("\t\t" + y + "年\t" + m + "月");
        System.out.println("----------------------------");
        System.out.printf("%4s%4s%4s%4s%4s%4s%4s\n", "Sun", "Mon", "Tue", "Wed",
                "Thu", "Fri", "Sat");
    }
    // 打印日历表体
    public static void printBody(int y, int n){
        int startDay = getStarDay(y, n);
        int dayOfTheMonth = dayOfmonth(y, n);
        for(int i = 0; i < startDay; i ++){
            System.out.printf("    ");
        }
        for(int i = 1; i <= dayOfTheMonth; i ++){
            System.out.printf("%4d", i);
            if((i + startDay) % 7 == 0){
                System.out.println();
            }
        }
        System.out.println();
    }
    // 计算该月第一天是星期几
    public static int getStarDay(int year, int month){
        int total = dayOfall(year, month);
        final int START_DAY_18000101 = 3;           // 1800/01/01这一天为星期三
        return (total + START_DAY_18000101) % 7;
    }
    // 计算从1800/01/01到该月第一天经历了几天，等于 除改年的年份天数 + 除该月的月份天数
    public static int dayOfall(int year,int month){
        int total = 0;
        for(int i = 1800; i < year; i++){
            if(isLeapYear(i)){
                total += 366;
            }else {
                total += 365;
            }
        } // end for
        for(int j = 1; j < month; j++){
            total += dayOfmonth(year, j);
        }
        return total;
    }
    // 计算平年还是闰年
    public static  boolean isLeapYear(int year){
        boolean isleap = true;
        if((year % 4 == 0 && year % 100 == 0) || year % 400 == 0 ){
            return isleap = true;
        }else{
            return  isleap = false;
        }
    }
    // 计算月份天数
    public static int dayOfmonth(int year, int month){
        if(month == 1 || month == 3 || month == 5 || month == 7 ||
        month == 8 || month == 10 || month == 12)
            return 31;
        else if (month == 4 || month == 6 || month == 9 || month == 11)
            return 30;
        else return isLeapYear(year) ? 29 : 28;
    }
}

```

**结果如下：**

![Snipaste_2020-02-20_14-18-33](//tva1.sinaimg.cn/large/005tpOh1gy1gc2udp3gxwj30ar09et8r.jpg)

