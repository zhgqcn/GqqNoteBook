---
title: Java基础-day07
date: 2020-02-12
categories: JavaSE
tags: Java
id: Gqq
---

# 循环语句

**利用`while`循环完成猜数字**：数字范围[1,100]<!--more-->

```java
import java.util.Scanner;
public class test {
    public static void main(String[] args) {
        int number = (int)(Math.random() * 101);  // 产生0到100的随机数
        System.out.println("random number is: " +number);
        Scanner input = new Scanner(System.in);
        System.out.print("Enter :");
        int guessNumber = input.nextInt();
        int low = 0;
        int high = 100;
        while (guessNumber != number){
            while (guessNumber < 0 || guessNumber > 100){
                System.out.print("The number is not in the right range, enter: ");
                guessNumber = input.nextInt();
            }
            if(guessNumber < number){
                low = guessNumber + 1;
            }else{
                high = guessNumber - 1;
            }
            System.out.println("The number is between " + low + " and " + high);
            System.out.print("Enter: ");
            guessNumber = input.nextInt();
        }
        System.out.println("You are right!");
    }
}
```

**多个减法测试题**

```java
import java.util.Scanner;
public class SubtractionQuizLoop {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        String output = "";							// 字符串连接应用
        final int TIME_COUNT = 5;
        long startTime = System.currentTimeMillis();
        int count = 0;
        int i = 0;
        while (i < TIME_COUNT){
            int number1 = (int)(Math.random() * 10);
            int number2 = (int)(Math.random() * 10);
            System.out.printf("What is %d - %d ?", number1, number2);
            int answer = input.nextInt();
            if(number1 - number2 != answer){
                System.out.println("Your answer is wrong.");
                System.out.printf("%d - %d should be %d\n", number1, number2, number1 - number2);
                output += number1 + " - " + number2 + " = " + answer + " wrong \n";
            }else {
                System.out.println("You are correct !");
                count ++;
                output += number1 + " - " + number2 + " = " + answer + " correct \n";
            }
            i ++;
        }
        double endTime = System.currentTimeMillis();
        int totalTime = (int)((endTime - startTime) / 1000);
        System.out.printf("Correct count is %d\nTest time is %d seconds\n", count, totalTime);
        System.out.println(output);
    }
}
```



# 输入和输出重定向

可以将数据用空格隔开，保存在一个名为`input.txt`的文本文件中，然后使用下面的`输入重定向`命令运行：

```java
java SentinelValue < input.txt
```

还有`输出重定向`

```
java ClassName > output.txt
```

可以在同一命令行中同时使用输入重定向和输出重定向

```java
java SentineValue < input.txt > output.txt
```



# do ... while循环

```java
do{
    // 循环体
    语句（组）;
}while(循环继续条件);  	// ;不能掉
```

```java
import java.util.Scanner;
public class test {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.println("输入被加数, 0 退出");
        int sum = 0;
        int i;
        do{
            System.out.print("输入一个整数: ");
            i = input.nextInt();
            sum += i;
        }while (i != 0);
        System.out.println("总和: " + sum);
    }
}
```



# for循环

```java
for(初始操作;循环继续条件;每次迭代后的操作){
	// 循环体;
    语句组;
}
```

> for循环中的初始动作可以是0个或者多个以逗号隔开的变量声明语句或赋值表达式

```java
for(int i = 0, j = 0; i + j < 10; i++, j++){}	
```

> for循环中的每次迭代后的动作可以是0个或者多个以逗号隔开的语句

```java
for(int i = 1; i < 100; System.out.println(i), i++){}
```

# 嵌套循环

> 一个循环里面嵌套在另外一个循环中

`打印乘法口诀表`

```java
import java.util.Scanner;
public class test {
    public static void main(String[] args) {
        System.out.print("\t\t\t\t乘法口诀表\n\t");
        for(int i = 1; i < 10; i++){
            System.out.print(i + "\t");
        }
        System.out.print("\n");
        for(int i = 1; i < 10; i++){
            System.out.print(i + " | ");
            for(int j = 1; j < 10; j++){
                System.out.print( (i * j) + "\t");
            }
            System.out.print("\n");
        }
    }
}
```

> 		1	2	3	4	5	6	7	8	9	
> 	1 | 1	2	3	4	5	6	7	8	9	
> 	2 | 2	4	6	8	10	12	14	16	18	
> 	3 | 3	6	9	12	15	18	21	24	27	
> 	4 | 4	8	12	16	20	24	28	32	36	
> 	5 | 5	10	15	20	25	30	35	40	45	
> 	6 | 6	12	18	24	30	36	42	48	54	
> 	7 | 7	14	21	28	35	42	49	56	63	
> 	8 | 8	16	24	32	40	48	56	64	72	
> 	9 | 9	18	27	36	45	54	63	72	81	



# 最小化数值错误

> 在循环继续条件中使用浮点数将导致数值错误

```java
double sum = 0;
for(double i = 0.01; i <= 1.0; i = i + 0.01) sum += i;
```

> 上面结果为：49.50000000000003. 结果差距较大，应该采用以下方法

```java
double currentValue = 0.01;
for(int count = 0; cout < 100; count ++){
    sum += currentValue;
    currentValue += 0.01;
}
```



# 实例学习

`1. 求最大公约数`

```java
int gcd = 1;	// 1是最小公约数
int k = 2;		
while(k <= n1 && k <= n2){
    if(n1 % k ==0 && n2 % k == 0){
        gcd = k;
    }
    k++;
}
```



`2.  十进制转换为十六进制`

```java
import java.util.Scanner;
public class test {
    public static void main(String[] args) {
        String hex = "";
        Scanner input = new Scanner(System.in);
        System.out.print("输入十进制数：");
        int decimal = input.nextInt();

        while (decimal != 0){
            int hexValue = decimal % 16;
            char hexDigit = (hexValue <= 9 && hexValue >= 0) ?
                    (char)(hexValue + '0') : (char)(hexValue - 10 + 'A');
            hex += hexDigit;
            decimal /= 16;
        }
        String reverse = new StringBuffer(hex).reverse().toString();  // 字符串反转
        System.out.println("十六进制数为" + reverse);
    }
}
```

> 输入十进制数：1234
> 十六进制数为：4D2



# break和continue

`break:`立即终止循环，跳出循环

`continue:`终止本次循环，进入下次循环



# 练习题

`**判断回文串`**

```java
import java.util.Scanner;
public class Palindrome {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        String s = input.next();
        int len = s.length();
        boolean k = true;
        for(int i = 0; i < len / 2; i++){
            if(s.charAt(i) == s.charAt(len - 1 - i)){
                k = true;
            }else {
                k = false;
                break;
            }
        }
        if(k == true)System.out.println(s + " 是回文串");
        else System.out.println(s + "不是回文串");
    }
}
```

> hahaha
> hahaha不是回文串
>
> hoooh
> hoooh 是回文串

**`判断素数`**

素数: 它的因子只有1和本身

```java
public class PrimeNumber {
    public static void main(String[] args) {
        final int NUMBER_OF_PRIMES = 50;
        final int NUMBER_OF_PRIMES_PER_LINE = 10;
        int count = 0;
        int number = 2;
        System.out.println("The first 50 prime numbers are ");
        while (count < NUMBER_OF_PRIMES){
            boolean isPrime = true;
            for (int divisor = 2; divisor <= number / 2; divisor ++){
                if(number % divisor == 0){
                    isPrime = false;
                    break;
                }
            }
            if(isPrime){
                count ++;
                if(count % NUMBER_OF_PRIMES_PER_LINE == 0){
                    System.out.println(number);
                }else{
                    System.out.print(number + " ");
                }
            }
            number ++;
        }
    }
}
```

