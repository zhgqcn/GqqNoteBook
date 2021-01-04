---
title: Java基础-day15
date: 2020-03-12
categories: JavaSE
tags: Java
id: Gqq
---

## 将基本数据类型值作为对象处理

> 基本数据类型值不是一个对象 ， 但是可以使用 `Java API`中的包装类来包装成一个对象 。
>
> > 出于对性能的考虑 ， 在 Java 中基本数据类型不作为对象使用, 因为处理对象需要额外
> > 的系统开销

<!--more-->

在`Java.lang`包里为基本数据类型提供了`Boolean`、`Character`、`Double`、`Float`、`Byte`、`Short`、`Integer`、`Long`等包装类。 大多数基本类型的包装类的名称与对应的基本數据类型名称一样 ， 第一个字母要大写 。` Integer `和` Character` 例外

![t](https://tvax4.sinaimg.cn/large/005tpOh1gy1gcrhnxbscfj30pc0dnwou.jpg)

- 每一个数值包装类都有常量` MAX _ VALUE` 和` MIN _ VALUE`

  - 对于` Byte 、 Short 、 Integer 、 Long `而言 ， `MIN _ VALUE `表示对应的基本类型
    `byte 、 short 、 int 、 long `的最小值

  - 对`Float `和` Double` 类而言 ， `MIN _ VALUE` 表示 `float` 型和 `double` 型的最小正值 。

- 数值包装类有一个有用的静态方法 `valueOf(String s )` 。 该方法创建一个新对象 ， 并将
  它初始化为指定字符串表示的值。

- 每个数值包装类都有两个重载的方法 ， 将数值字符串转换为正确的以 10 ( 十进制 ） 或指定值为基数 （ 例如 ， 2 为二进制 ， 8 为八进制 ， 16 为十六进制 ） 的数值

```java\
Integer.parseInt("11", 2) returns 3;
Integer.parseInt("12", 8) returns 10;
Integer.parseInt("13", 10) returns 13;
Integer.parseInt("1A", 16) returns 26;

Integer.parseInt("12", 2) ; // error! 12不是二进制数
// 使用format方法将一个十进制数转换为十六进制数
String.format("%x", 26) returns 1A;
```

## 基本类型和包装类类型之间的自动转换

> 根据上下文环境 ， 基本数据类型值可以使用包装类自动转换成一个对象 ， 反过来的自动转换也可以

将**基本类型值**转换为**包装类对象**的过程称为`装箱（boxing）` , 相反的转换过程称为`开箱(unboxing)`

过程：

- `Java` 允许基本类型和包装类类型之间进行`自动转换 `。 如果一个`基本类型值`出现在需要`对象的环境`中 ， 编译器会将基本类型值进行`自动装箱` ； 如果一个`对象`出现在需要`基本类型值的环境`， 编译器会将对象进行`自动开箱`

```java
Integer[] intArray = {1, 2, 3};
System.out.println(intArray[0] + intArray[1] + intArray[2]);
```

-  第一行中，基本类型值1、2、3被自动装箱成对象`new Integer[1]`、`new Integer[2]`、`new Integer[3]`
- 第二行中，对象`intArray[0]`、`intArray[1]`、`intArray[2]`被自动转换为`int`值，然后进行相加



## `BigInteger`和`BigDecimal`类

> 这两种类可以用于表示**任意大小**和**精度**的***整数***或者***十进制数***
>
> > 在`java.math`包中

`long`类型的最大整数值仅仅为`long.MAX_VALUE（即9223372036854775807)`

使用`new BigInteger(String)`和`new BigDecimal(String)`来创建`BigInteger`和`BigDecimal`的实例，有以下方法进行运算：

- 算术运算
  - add
  - subtract
  - multiply
  - divide
  - remainder
- 比较
  - compareTo

```java
BigInteger a = new BigInteger("9223372036854775807");
BigInteger b = new BigInteger("2");
BigInteger c = a.multiply(b);
```

对 `BigDecimal `对象的精度没有限制 。 如果结果不能终止 ， 那么 `divide `方法会抛出
`ArithmeticException` 异常。 可以使用重载的 `divide ( BigDecimal d, int scale, int
roundingMode) `方法来指定尺度和舍入方式来避免这个异常, `scale`是小数点后最小的整数位数

```java
BigDecimal a = new BigDecimal(1.0);
BigDecimal b = new BigDecimal(3.0);
BigDecimal c = a.divide(b, 20, BigDecimal.ROUND_UP);
System.out.println(c);
```

**Result：0.33333333333333333334**

### 实例

> 一个整数的阶乘可能会很大。该代码可以返回任意整数阶乘的方法

```java
import java.math.*;
public class LargeF{
    public static void main(String[] args){
        System.out.println("50! is \n" + F(50));
    }
    
    public static BigInteger F(long n){
        BigInteger result = BigInteger.ONE;  // 相当于初始化为 1 
        for(int i = 1; i <= n; i++){
            result = result.multiply(new BigInteger(i + ""));
        }
    }
}
```



## String类

> String对象是不可改变的，字符串一旦创建，内容不能改变

### 构造字符串

可以使用`字符串直接量`或者`字符数组`创建一个字符串对象

创建一个字符串:`String newString = new String(stringLiteral);`

- 直接量方法

```java
String message = new String("Gqq's page");
```

* 字符数组方法

```java
char[] charArray = {'G', 'q', 'q', '\'', 's',  ' ', 'p', 'a', 'g', 'e'};
String message = new String(charArray);
```

### 不可变字符串和限定字符串

```java
String s = "Java";
s = "HTML";			// 这里并不能改变原始字符串内容，只是引用对象变了
```

![Snipaste_2020-03-13_11-56-43](//tva4.sinaimg.cn/large/005tpOh1gy1gcs5wpn933j30m606jtbi.jpg)

`限定字符串`：因为字符串在程序设计中不可变，但同时频繁使用，所以`Java虚拟机`为了提高效率并节约内存，***对具有相同字符序列的字符串直接量使用同一个实例***，这种实例称为限定的字符串

![Snipaste_2020-03-13_11-59-08](//tva3.sinaimg.cn/large/005tpOh1gy1gcs5zfqv15j30mo087n05.jpg)

尽管s1和s2内容相同，但它们是不同的字符串对象

### 字符串的替换和分隔

```java
+ replace(oldChar: char, newChar: char): String      // 将字符串中所有匹配的字符替换成新的宇符,然后返冋新的字符串 
+ replaceFirst(oldChar: char, newChar: char): String // 将字符串中第一个匹配的子字符串替换成新的子字符串,然后返回新的字符串
+ replaceAll(oldChar: char, newChar: char): String	 // 将字符串中所有匹配的子字符串替换成新的子字符串,然后返回新的字符串
+ split(delimiter: String): String[]    			 // 返回一个字符串数组 ， 其中包含被分隔符分隔的子宇符串集
```

**字符串一旦创建其内容就不能改变**，上述方法返回一个源自原始字符串的新字符串（并没有改变原始字符串）

`split 方法`可以从一个指定分隔符的字符串中提取标识

```java
String[] tokens = "Java#HTML#PERL".split("#");
for(int i = 0; i < tokens.length; i++){
    System.out.print(tokens[i] + " ");
}
```

**Result : Java HTML PERL**

### 依照模式匹配、替换、分隔

`正则表达式(regular expression)(regex)`：是一个字符串， 用于描述匹配一个字符串集的模式。 可以通过指定某个模式来匹配 、 替换或分隔一个字符串

虽然`equals`方法亦可以来判断两个字符串是否相同，但是利用`matches`方法功能更加强大

```java
"Java is fun".matches("Java.*");  		// return true 
"Java is powerful".matches("Java.*");	// return true
```

举几个例子：

```java
"440-02-4534".matches("\\d{3}-\\d{2}-\\d{4}")
```

\\\d表示单个数字位，\\\\\d{3}表示三个数字位，所以返回true

```java
String s = "a+b#c".replaceAll("[$+#]", "NNN");
```

[$+#]表示能够匹配$、+、#的模式，所以返回**aNNNbNNNNNNc**

```java
String[] tokens = "Java,C?C#,C++".split("[.,:;?]");
for(int i = 0; i < tokens.length; i++){
    System.out.println(tokens[i]);
}
```

返回Java、C、C#、C++



### 字符串和数字之间的转换

字符串不是数组，但是它们可以相互转换。

