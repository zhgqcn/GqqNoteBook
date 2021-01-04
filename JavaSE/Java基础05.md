---
title: Java基础-day05
date: 2020-02-10
categories: JavaSE
tags: Java
id: Gqq
---

# 常用数学函数

## 三角函数方法

|         方法         |                     描述                     |
| :------------------: | :------------------------------------------: |
|    $sin(radians)$    |  返回以**弧度**为单位的角度的三角正弦函数值  |
|    $cos(radians)$    |  返回以**弧度**为单位的角度的三角余弦函数值  |
|    $tan(radians)$    |  返回以**弧度**为单位的角度的三角正切函数值  |
| $toRadians(degree)$  |   将以**度**为单位的角度值转换为以弧度表示   |
| $toDegrees(radians)$ |   将以**弧度**为单位的角度值转换为以度表示   |
|      $asin(a)$       | 返回以**弧度**为单位的角度的反三角正弦函数值 |
|      $acos(a)$       | 返回以**弧度**为单位的角度的反三角余弦函数值 |
|      $atan(a)$       | 返回以**弧度**为单位的角度的反三角正切函数值 |

<!--more-->

- $asin$和$atan$的返回值是$-π/2$~$π/2$的一个弧度值
- $acos$的返回值在$0$到$π$之前
- $1°$相当于$π/180$弧度, $90°$相当于$π/2$弧度, $30°$相当于$π/6$弧度 

```java
    double x1 = Math.toDegrees(Math.PI / 2);    // 90.0
    double x2 = Math.toRadians(30);             // 0.5236(π/6)
    double x3 = Math.sin(0);                    // 0.0
    double x4 = Math.sin(Math.toRadians(270));  // -1.0
    double x5 = Math.cos(Math.PI / 6);          // 0.866
    double x6 = Math.asin(0.5);                 // 0.523598333(π/6)
```

## 指数函数方法

|    方法    |              描述               |
| :--------: | :-----------------------------: |
|  $exp(x)$  |          返回e的x次方           |
|  $log(x)$  |         返回x的自然底数         |
| $log10(x)$ |      返回x的以10为底的对数      |
| $pow(a,b)$ |          返回a的b次方           |
| $sqrt(x)$  | 对于x ≥ 0 的数字，返回x的平方根 |

```java
    double y1 = Math.exp(1);        // 2.71828
    double y2 = Math.log(Math.E);   // 1.0
    double y3 = Math.log10(10);     // 1.0
    double y4 = Math.pow(2, 3);     // 8.0
    double y5 = Math.sqrt(4);       // 2.0     
```

## 取整方法

|    方法    |                             描述                             |
| :--------: | :----------------------------------------------------------: |
| $ceil(x)$  |    x向上取整为它最接近的整数。该整数作为一个双精度值返回     |
| $floor(x)$ |    x向下取整为它最接近的整数。该整数作为一个双精度值返回     |
| $rint(x)$  | x取整为它最接近的整数。如果x与两个整数的距离相等，偶数的整数作为一个双精度值返回 |
| $round(x)$ | $$如果x是单精度数，返回（int）Math.floor(x+0.5)$$$$如果x是双精度数，返回（long）Math.floor(x+0.5)$$ |

## min、max和abs方法

1. min和max方法用于返回`两个`数（int、long、float、double型)的最小值和最大值。  

2. abs方法以返回`一个`数（int、long、float、double型)的绝对值。

```java
Math.max(2, 3);		// 3
Math.max(2.5, 3);	// 3.0
Math.abs(-2.1);		// 2.1
```



## random方法

> 可以使用`Math.random()`来获得一个`0.0`到`1.0`之间的随机`double`值，不包括`1.0`

```java
(int)(Math.random() * 10);		// 	返回0到9之间的一个随机数
50 + (int)(Math.random() * 10);	// 	返回50到99之间的一个随机数
a + Math.random() * b;			//	返回a到 a+b 之间的一个随机数，不包括 a+b
```

**练习: 利用三角形的三条边计算三个角度 **

```java
import java.util.*;

public class ComputerAngles {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.print("输入三角形三个顶点坐标:");
        double x1 = input.nextDouble();
        double y1 = input.nextDouble();
        double x2 = input.nextDouble();
        double y2 = input.nextDouble();
        double x3 = input.nextDouble();
        double y3 = input.nextDouble();

        double line1 = Math.sqrt(Math.pow(x1 - x2, 2) + Math.pow(y1 - y2, 2));
        double line2 = Math.sqrt(Math.pow(x1 - x3, 2) + Math.pow(y1 - y3, 2));
        double line3 = Math.sqrt(Math.pow(x2 - x3, 2) + Math.pow(y2 - y3, 2));

        double angleA = Math.toDegrees(Math.acos((line1 * line1 - line2 * line2 - line3 * line3) / (-2 * line2 * line3)));
        double angleB = Math.toDegrees(Math.acos((line2 * line2 - line1 * line1 - line3 * line3) / (-2 * line3 * line1)));
        double angleC = Math.toDegrees(Math.acos((line3 * line3 - line2 * line2 - line1 * line1) / (-2 * line2 * line1)));

        System.out.println("3个角度分别为：" + Math.round(angleA * 100) / 100.0 + " " +
                Math.round(angleB * 100) / 100.0 + " " + Math.round(angleC * 100) / 100.0);
    }
}
```



# 字符数据类型和操作 

```java
char letter = 'A';
char numChar = '4';
```

> 字符串直接量必须括在双引号中。而字符直接量是括在单引号中的单个字符。因此 "A" 是一个字符串，而 ’A’ 是一个字符。

##  Unicode 和 ASCII 码 

- Java 支持 Unicode 码，它支持使用 世界各种语言所书写的文本的交换、处理和显示。Unicode 码一开始被设计为 16位的字符编码，后来被扩展到1112064个字符，超过16位限制的称为补充字符。  

- 大多数计算机采用`ASCII`码，它是表示所有大小写字母、数字、 标点符号和控制字符的8位编码表。Unicode码包括ASCII码。

| 字符  | 十进制编码值 |    Unicode值    |
| :---: | :----------: | :-------------: |
| 0 ~ 9 |   48 ~ 57    | \u0030 ~ \u0039 |
| A ~ Z |   65 ~ 90    | \u0041 ~ \u005A |
| a ~ z |   97 ~ 122   | \u0061 ~ \u007A |

```java
char letter = '\u0041'; // A
```

## 特殊字符的转义序列

| 转义序列 |  名称  | Unicode码 | 十进制值 |
| :------: | :----: | :-------: | :------: |
|    \b    | 退格键 |  \u0008   |    8     |
|    \t    | Tab键  |  \u0009   |    9     |
|    \n    | 换行符 |  \u000A   |    10    |
|    \f    | 换页符 |  \u000C   |    12    |
|    \r    | 回车符 |  \u000D   |    13    |
|   \\\    | 反斜杠 |  \u005C   |    92    |
|   \\"    | 双引号 |  \u0022   |    34    |

## 字符型数据与数值型数据之间的转换

char 型数据可以转换成任意一种数值类型，反之亦然

1. 将整数转换成 char型数据时， 只用到该数据的低十六位，其余部分都被忽略。

```java
char ch = (char)0XAB0041; // The lower 16 bits hex code 0041 is assigned to ch
System.out.println(ch);   // ch is character A 
```

2. 要将一个浮点值转换成 char 型时，首先将浮点值转换成 int 型，然后将这个整型值转 换为 char 型。 

```java
char ch = (char)65.25; 		// Decimal 65 is assigned to ch 
System.out.println(ch);		// "ch is character A 
```

3. 当一个 char 型数据转换成数值型时，这个字符的Unicode码就被转换成某个特定的数值 类型。 

```java
int i = (int)'A'; 			// The Unicode of character A is assigned to i
System.out.println(i); 		// i is 65 
```

4. 如果转换结果适用于目标变童，就可以使用隐式转换方式；否则，必须使用显式转换方式

```java
byte b = 'a';
int i = 'a';
```

​		但是，因为 Unicode 码 \uFFF4 超过了一个字节的范围，所以下面的转换就是不正确的

```java
byte b = '\uFFF4';  		// wrong
byte b = (byte)'\uFFF4'; 	// right, 显示转换了
```

5. 0~FFFF的任何一个十六进制正整数都可以隐式地转换成字符型数据。而不在此范围内的任何其他数值都必须显式地转换为 char 型

6. 所有数值操作符都可以用在 char 型操作数上。如果另一个操作数是一个数字或字符， 那么char 型操作数就会被自动转换成一个数字。如果另一个操作数是一个字符串，字符就 会与该字符串相连。

## 字符的比较与测试

通过比较两个字符的Unicode值实现的。  

Character 类中的方法

|        方法         |                    描述                     |
| :-----------------: | :-----------------------------------------: |
|     isDigit(ch)     |     如果指定的字符是一个数字，返冋 true     |
|    isLetter(ch)     |     如果指定的字符是一个字母，返冋 true     |
| isLetterOrDigit(ch) | 如果指定的字符是一个字母或者数字，返回 true |
|   isLowerCase(ch)   |   如果指定的字符是一个小写字母，返冋 true   |
|   isUpperCase(ch)   |   如果指定的字符是一个大写字母，返冋 true   |
|   toLowerCase(ch)   |          返回指定的字符的小写形式           |
|   toUpperCase(ch)   |          返回指定的宇符的大写形式           |



# string类型

为了表示一串字符，使用称为 String (字符串）的数据类型

```java
String message = "Welcome to Java";
```

String类型不是基本类型，而是引用类型。  

常用方法：

|      方法       |                     描述                     |
| :-------------: | :------------------------------------------: |
|   $length()$    |             返回字符串中的宇符数             |
| $charAt(index)$ |        返回字符串 S 中指定位置的字符         |
|  $concat(s1)$   | 将本字符串和字符串 Si 连接，返回一个新字符串 |
| $toUpperCase()$ |     返回一个新宇符串，其中所有的字母大写     |
| $toLowerCase()$ |     返回一个新字符串，其中所有的字母小写     |
|    $trim()$     |    返回一个新字符串，去掉了两边的空白宇符    |

以上方法都为`实例方法`

- 实例方法：只能从一个特定的（字符串）实例来调。
- 静态方法：非实例方法称为静态方法。静态方法可以不使用对象来调用。定义在$Math$类中的所有方法都是静态方法。它们没有绑定到一个特定的对象实例上。 调用一个实例方法的语法是 $reference-Variable.methodName(arguments)$。

`1、求字符串长度：`  

```java
String message = "Welcome to Java"; 
System.out.println("The length of " + message + " is " + message.1ength())；
```

> 显示：The length of Welcome to Java is 15

`2、从字符串中获取字符`

字符串下标从0开始

```java
String message = "Welcome to Java"; 
System.out.println(message.charAt(0));
```

> 显示：W

`3、连接字符串`

```java
String message1 = "Welcome to Java"; 
String message2 = "and Java is fun";
String s = message1.concat(message2); // Welcome to Java and Java is fun
String s_ = message1 + message2;	  // 效果一样，加号也可字符串
```

注意：

```java
int i = 1; 
int j = 2;
System.out.println("i + j is " + i + j); // 结果为: i + j is 12
System.out.println("i + j is " + (i + j)); // 结果为: i + j is 3
```

`4、字符串的转换`

```java
"Welcome".toLowerCase();	// 返回一个新字符串 welcome
"Welcome".toUpperCase();	// 返回一个新字符串 WELCOME
```

空白字符：` `、`\t`、`\f`、`\r`、`\n`

```java
"\t Good Night \n".trim()	// 返回一个新字符串 Good Night
```

`5、从控制台读取字符串`

- 为了从控制台读取字符串，调用 Scanner 对象上的 `next()` 方法
- `next()`方法读取以空白字符结束的字符串
- 可以使用`nextLine()`读取一整行文本

> 为了避免输入错误，不要再`nextByte()`、`nextShort()`、`nextlnt()`、`nextLong()`、 `nextFloat()`、`nextDouble()` 和 `next()`之后使用` nextLine()`

`6、从控制台读取字符`

为了从控制台读取字符，调用 `nextLine()`方法读取一个字符串，然后在字符串上调用 `charAt(0)`来返回一个字符。

```java
import java.util.Scanner;
Scanner input = new Scanner(System.in);
String s = input.nextLine();
char ch = s.charAt(0);
```

`7、字符串比较`

|           方法            |                             描述                             |
| :-----------------------: | :----------------------------------------------------------: |
|       $equals(s1)$        |             如果该字符串等于字符串s1, 返回 true              |
|  $equalsIgnoreCase(s1)$   |      如果该字符串等于字符串s1, 返回 true; 不区分大小写       |
|      $compareTo(s1)$      | 返回一个大于0、等于0、小于0的整数，表明一个字符串是否大于、 等于或者小于s1 |
| $compareToIgnoreCase(s1)$ | 返回一个大于0、等于0、小于0的整数，表明一个字符串是否大于、 等于或者小于s1 , 不区分大小写 |
|   $startsWith(prefix)$    |            如果字符串以特定的前缀开始，返回 true             |
|    $endsWith(suffix)$     |             如果宇符串以特定的后缀结束.返回 true             |
|      $contains(s1)$       |            如果s1 是该字符串的子字符串，返回 true            |

`8、获得子字符串`

利用`substring()`方法

```java
String message = "Welcome to Java’;
String message = message.substring(0, 11) + "HTML"; 
// 字符串message 变成了 Welcome to HTML.
```

|               方法                |                             描述                             |
| :-------------------------------: | :----------------------------------------------------------: |
|      $substring(beginlndex)$      | 返回该字符串的子串，从特定位置 $beginlndex $的字符开始到字符串的结尾 |
| $substring(beginlndex, endlndex)$ | 返回该字符串的子串，从特定位置 $beginlndex$的字符开始到下标为 $endlndex - 1$的字符。注意，位于 $endlndex$ 位置的字符不属 于该子字符串的一部分 |

`9、获取字符串中的字符或者子串的位置下标`

|             方法             |                             描述                             |
| :--------------------------: | :----------------------------------------------------------: |
|        $indexOf(ch) $        | 返回字符串中出现的第一个 $ch$的下标。如果没有匹配的，返回 -1 |
|   $indexOf(ch, fromlndex)$   | 返冋字符串中 $fromlndex$ 之后出现的第一个$ ch$ 的下标。如果没有匹配 的，返回-1 |
|         $indexOf(s)$         | 返冋字符串中出现的第一个字符串 $s$ 的下标。如果没有匹配的，返冋 -1 |
|   $indexOf(s, fromlndex) $   | 返回字符串中 $fromlndex$ 之后出现的第一个字符串 $s$ 的下标。如果没有 匹配的，返回 -1 |
|      $lastIndexOf(ch)$       | 返回字符串中出现的最后一个 $ch$的下标。如果没有匹配的，返回 -1 |
| $lastIndexOf(ch, fromIndex)$ | 返回字符串中 $fromlndex $之前出现的最后一个 $ch$的下标。如果没有匹配 的，返回 -1 |
|       $lastIndexOf(s)$       | 返回字符串中出现的最后一个字符串 $s$ 的下标。如果没有匹配的，返回 -1 |
| $lastIndexOf(s, fromIndex)$  | 返回字符串中 $fromlndex$ 之前出现的最后一个字符串 $s$ 的下标。如果没有匹配的，返回 -1 |

问题：假设一个字符串 s 包含使用空格分开的姓和名。将姓和名分开

```java
string s = "Kim Jones";
int k = s.indexOf(' ');
String firstName = s.substring(0, k);
String lastName = s.substring(k + 1);
```

`10、字符串和数字间的转换`

1. 将字符串转换为`int`值

```java
int intValue = Integer.parseInt(intString);
```

2. 将字符串转换为`double`值

```java
double doubleValue = Double.parseDouble(doubleString);
```

3. 将数字转换为字符串

```java
String s = number + ""; 
```



