---
title: Java基础-day12
date: 2020-02-27
categories: JavaSE
tags: Java
id: Gqq
---

# 对象和类

>  面向对象编程可以有效地帮助开发大规模的软件以及图形用户界面

<!--more-->

## 为对象定义类

>  `类`为对象定义`属性`和`行为`

```shell
e.g
对象：一个圆
属性：半径radius
方法：getArea()返回圆的面积 setRadius(radius)修改圆的半径
```

- 一个**通用类**来定义**同一类型**的对象
- 一个对象是类的一个实例
-  可以从一个类中创建多个实例

![Snipaste_2020-02-27_21-02-03](//tvax4.sinaimg.cn/large/005tpOh1gy1gcb9ebulsrj30jx0a5jux.jpg)

**构造方法：**可以创建一个**新对象**，为了完成初始化动作

一般的类不包含`main函数`，有包含`main函数`的类叫做`主类`

**统一建模语言`UML`**：利用*图形符号*对类的模板和对象的图示进行标准化。这种表示方法称为`UML类图`或者简称`类图`

```java
数据域表示为
	dataFieldName : dataFieldType
构造方法表示为
    ClassName(parameterName : parameterType)	
方法表示为
    methodName(parameterName : parameterType)
```

## 定义类和创建对象

> 类是对象的定义，对象从类中创建

```java
public class TestSimpleCircle{
    public static void main(String[] args){
        SimpleCircle circle1 = new SimpleCircle();
        SimpleCircle circle2 = new SimpleCircle(25);
        SimpleCircle circle3 = new SimpleCircle();
        circle3.radius = 10;		// circle.setRadius(10)
        System.out.println(circle1.radius + "为半径的圆的面积为:" + circle1.getArea());
        System.out.println(circle2.radius + "为半径的圆的面积为:" + circle2.getArea());
        System.out.println(circle3.radius + "为半径的圆的面积为:" + circle3.getArea());
    }
}
    
class SimpleCircle{
    double radius;
    SimpleCircle(){
        raduis = 1;
    }
    SimpleCircle(double newRadius){
        radius = newRadius;
    }
    double getArea(){
        return radius * radius * Matn.PI;
    }
    double getPerimeter(){
        return 2 * Math.PI * radius;
    }
    void setRadius(double newRadius){
        radius = newRadius;
    }
}
```

- 程序包含两个类，第一个类`TestSimpleCircle`是主类，他的唯一目的就是测试第二个类`SimpleCircle`。使用这样的类的程序通常称为`类的客户`，运行这个程序时，Java运行系统会调用这个主类的`main`方法。`Java编译器`会生成两个以类名为文件名的`.class`文件

- 可以把两个类放在同一个文件中 ， 但是文件中只能有**一个**类是`公共类 `。 此外 ,公共类必须与文件同名
- 利用`new`来创建对象
- 可以将本例子中的两个类组成一个类
- 对于`公共public方法`可以在其他类中访问

## 使用构造方法构造对象

- 构造方法必须具备和所在类**相同的名字**
-  构造方法**没有返回值**类型 ， 甚至连 void 也没有
- 构造方法是在创建一个对象使用**new操作符**时调用的 。 构造方法的作用是**初始化对象**

```java
new ClassName(arg);
```

`无参数构造方法`: 通常，一个类会提供一个**没有参数**的构造方法

`默认构造函数`: — 个类可以**不定义构造方法** 。在这种情况下 , 类中**隐含定义**一个方法体为空的**无参构造方法**

 

## 通过引用变量访问对象

>  对象的数据和方法可以运用点操作符`.`通过对象的引用变量进行访问
>
> 新创建的对象在内存中被分配空间，他们可以通过引用变量来访问

### 引用变量和引用类型

对象是通过对象**引用变量**来访问的，该变量包含对对象的引用

```java
ClassName objectRefVar;
```

一个类是一个程序员定义的类型，类是一种**引用类型**

```java
Circle myCircle; // 变量myCircle能够引用一个Circle对象
myCircle = new Circle(); // 创建一个对象，并且将他的引用赋给变量myCircle
// 可以合并为一句
Circle myCircle = new Circle();
```

**对象Circle**和**对象引用变量myCircle**是不同的，但是大多数情况下这种差异是可以忽略的。

### 访问对象的数据和方法

通过`点操作符.`或称为`对象成员访问操作符`来引用对象的数据域和方法

- `objectRefVar.dataField`引用对象的数据域
- `objectRefVar.method(args)`调用对象的方法
- 对于只能通过具体实例调用的数据域或方法分别称为**实例变量**、**实例方法**

**思考**：

`Math类`可以通过`Math.methodName()`来调用类中的方法，而上例中的类却不能通过`Circle.getArea()`来调用。因为`Math类`中的所有方法都是用关键字`static`定义的`静态方法`, 而`getArea()`是实例方法，是非静态的，必须通过**引用对象**来实现调用



有时候，一个对象在创建后并不需要应用，这时，可以创建一个对象而并不将它明确地赋值给一个变量，如：

```java
new Circle();
System.out.println("Area is" + new Circle(5).getArea());
```

这样创建的对象成为`匿名对象`

### 引用数据域和null值

类的数据域可以是`引用类型`的，如`String`类型

```java
class Student{
    String name; // 引用类型的数据域
    int age;
    boolean isScienceMajor;
    char gender;
}
```

如果一个`引用类型的数据域`没有引用任何对象 ， 那么这个数据域就有一个特殊的 `Java`值 `null`

- `null`是引用类型直接量
- `true`  和`false`是`boolean`类型直接量

对于**类**，Java会**赋予**默认值；而对于**局部变量**，Java**不会**赋予默认值。

| 数据域的数据类型 |   默认值   |
| :--------------: | :--------: |
|    $引用类型$    |   $null$   |
|    $数值类型$    |    $0$     |
|  $boolean类型$   |  $false$   |
|    $char类型$    | $'\u0000'$ |

### 基本类型变量和引用类型变量的区别

- 对于基本类型变量，赋值的是具体的值
- 对于引用类型变量，赋值的是对应内存所存储的值的一个引用，是地址
- 对于引用类型变量，赋值语句`c1 = c2`会使原来`c1`指向的引用对象无法继续使用而变成`垃圾`,垃圾会占用内存空间，`Java运行系统`会检测垃圾并自动回收，称为`垃圾回收`
-  如果不再需要某个对象时 ， 可以显式地给该对象的引用变量赋`null`值 。 如果该对象没有被任何引用变量所引用 ， `Java 虚拟机`将自动回收它所占的空间

## 使用Java库中的类

### Date类

Java在`java.util.Date`类中提供了与系统无关的对日期和时间的封装

```java
+ Date()							// 为当前时间创建一个Date对象
+ Date(elapseTime: long)			// 为一个从格林威治时间1970.1.1至今流逝的以ms为单位的给定时间创建Date对象
+ toString(): long					// 返回一个代表日期和时间的字符串表示
+ getTime(): long					// 返回从格林威治时间1970.1.1至今流逝的ms数
+ setTime(elapseTime: long): void	// 在对象中设置一个新的流逝时间
```

```java
java.util.Date date = new java.util.Date();
System.out.println("The elapsed time since 1970.1.1 is: " + date.getTime() + "ms");
System.out.println(date.toString());
```

**结果**

```shell
The elapsed time since 1970.1.1 is: 1582870711588ms
Fri Feb 28 14:18:31 CST 2020
```

### Random类

除了使用`Math.Random()` ，Java在`java.util.Random`提供了参数随机数的方法

```java
+ Random()					// 以当前时间作为种子创建一个 Random 对象
+ Random(seed: long)		// 以一个特定值作为种子创建一个 Random 对象 
+ nextInt(): int			// 返冋一个随机的 int 值
+ nextInt(n: int): int		// 返回一个 0 到 n ( 不包含 n ) 之间的随机 int 类型的值
+ nextLong(): long			// 返回一个随机的 long 值
+ nextDouble(): double		// 返回一个 0.0 到 1.0 ( 不包含 1.0 ) 之间的随机 doubl e 类型的值
+ nextFloat(): float		// 返回一个 0.0 F 到 1.0 F ( 不包含 1.0 F ) 之间的随机 float 类型的值
+ nextBoolean(): boolean	// 返回一个随机的 boolean 值
```

- 创建一个 `Random` 对象时 ， 必须指定一个种子或者使用默认的种子。`种子`是一个用于初
  始化一个`随机数字生成器`的数字
- `无参构造方法`使用当前已经`逝去`的时间作为种子 ， 创建一个 `Random` 对象 
-  如果这`两个 Random` 对象有`相同`的种子 ， 那它们将产生`相同的数列 `

```java
import java.util.Random;

public class TestRandom {
    public static void main(String[] args) {
        Random random1 = new Random(3);
        System.out.print("From random1 : ");
        for (int i = 0; i < 10; i++)
            System.out.print(random1.nextInt(1000) + " ");
        Random random2 = new Random(3);
        System.out.print("\nFrom random2 : ");
        for (int i = 0; i < 10; i++)
            System.out.print(random2.nextInt(1000) + " ");
    }
}
```

**结果**

```shell
From random1 : 734 660 210 581 128 202 549 564 459 961 
From random2 : 734 660 210 581 128 202 549 564 459 961 
```

 产生相同随机值序列的能力在软件测试以及其他许多应用中是很有用的 。在软件测试中 ， 经常需要从一组固定顺序的随机数中来重复生成测试案例 。

### Point2D类

`Java API` 在 `javafx.geometry` 包中有一个便于使用的 `Point 2D` 类 ， 用于表示二维平面上
的点

```java
+ Point2D(x: double, y: double)			// 用给定的x和y坐标来创建一个Point2D对象
+ distance(x: double, y: double): double// 返回该点到给定点之间的距离
+ distance(p: Point2D): double			// 返回该点到给定点 P 之间的距离
+ getX(): double						// 返回该点的 x 坐标
+ getY(): double						// 返回该点的 y 坐标
+ toString(): String					// 返回该点的字符串表示
```

```java
import java.util.Scanner;
import javafx.geometry.Point2D;
public class TestPoint2D {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter point1's x-, y-coordinates: ");
        double x1 = input.nextDouble();
        double y1 = input.nextDouble();
        System.out.print("Enter point2's x-, y-coordinates: ");
        double x2 = input.nextDouble();
        double y2 = input.nextDouble();

        Point2D p1 = new Point2D(x1, y1);
        Point2D p2 = new Point2D(x2, y2);
        System.out.println("p1 is " + p1.toString());
        System.out.println("p2 is " + p2.toString());
        System.out.println("The distance between p1 and p2 is " + p1.distance(p2));
        System.out.println("The distance between p1 and (1, 1) is " + p1.distance(1, 1));
    }
}
```

**结果**

```shell
Enter point1's x-, y-coordinates: 1.5 5.5
Enter point2's x-, y-coordinates: -5.3 -4.4
p1 is Point2D [x = 1.5, y = 5.5]
p2 is Point2D [x = -5.3, y = -4.4]
The distance between p1 and p2 is 12.010412149464313
The distance between p1 and (1, 1) is 4.527692569068709
```

