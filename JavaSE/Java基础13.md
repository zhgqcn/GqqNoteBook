---
title: Java基础-day13
date: 2020-03-04
categories: JavaSE
tags: Java
id: Gqq
---

## 静态变量、常量和方法

>  `静态变量`被类中的所有对象所共享 。 `静态方法`不能访问类中的`实例`成员 <!--more-->

**Circle类**的**数据域radius**称为一个`实例变量`。实例变量是绑定到类的某个特定实例的，它是不能被同一个类的不同对象所共享的。

```java
Circle circle1 = new Circle();
Circle circle2 = new Circle(2);
```

Circle1和Circle2中的radius是不同的，它们存储在不同的内存位置。

如果想让一个类的`所有实例`共享数据 ， 就要使用`静态变量` , 也称为`类变量`。 静态变量将变量值存储在一个`公共的内存地址`，如果某一个对象`修改`了静态变量的值 ， 那么同一个类的`所有对象`都会受到影响

` Java `支持`静态方法`和`静态变量` ， 无须创建类的实例就可以调用`静态方法`

![Snipaste_2020-02-28_15-41-24](//tvax3.sinaimg.cn/large/005tpOh1gy1gcc5qdilvsj30pu08uwj2.jpg)

`静态变量`和`静态方法`的定义，只需加修饰符`static`

```java
static int numberOfobjects;
static int getNumberOfObjects(){
    return numberOfobjects;
}
```

类中的`常量`是被类的所有对象所共享的，声明常量用`final static`

```java
final static double PI = 3.1415;
```

`Math类`中的所有方法都是**静态**的。`main方法`也是**静态方法**

>  使用 “ 类名 . 方法名 （ 参数 ) ” 的方式调用静态方法 ， 使用 “ 类名 . 静态变量 ” 的方
> 式访问静态变量 。 这会提高可读性 ， 因为可以很容易地识别出类中的静态方法和数据

静态成员和实例成员的关系总结：

![Snipaste_2020-02-28_15-51-27](//tva4.sinaimg.cn/large/005tpOh1gy1gcc60l1vu5j30hs046762.jpg)

- 如果一个变量或方法**依赖于**类的某个具体实例 ， 那就应该将它定义为**实例变量**或**实例方法**
- 如果一个变量或方法**不依赖于**类的某个具体实例 ， 应该将它定义为**静态变量**或**静态方法**

<img src="//tva2.sinaimg.cn/large/005tpOh1gy1gcc66spjwoj30p208k0wl.jpg" alt="Snipaste_2020-02-28_15-57-24" style="zoom:80%;" />

## 可见性修饰符

>  可见性修饰符可以用于确定一个类以及它的成员的可见性

如果**没有**使用可见性修饰符 ， 那么则默认类、方法和数据域是可以被**同一个包**中的**任何一个
类**访问的 。 这称作`包私有` 或`包内访问` 

 **包可以用来组织类 。** 为了完成这个目标 ， 需要在程序中首先出现下面这行语句 ， 在
这行语句之前**不能有注释**也**不能有空白**

```java
package packageName;
```

**Java 建议最好将类放入包中 ， 而不要使用默认包**

修饰符有：

- 默认可见修饰符（即不被修饰）
  - ​可以在同一个包的任何一个类中访问 
  - 不能在不同包中使用
- public修饰符
  - 可以在同一个包的任何一个类中访问 
  - 能在不同包中使用

- private修饰符
  - 只能在同一个包的同一个类中使用
- protected修饰符

<img src="//tvax4.sinaimg.cn/large/005tpOh1gy1gchrmlp1vlj30nq0a7793.jpg" alt="Snipaste_2020-03-04_12-06-10" style="zoom:100%;" />

如果一个类没有被定义为公共类 ， 那么它只能在同一个包内被访问 。

![Snipaste_2020-03-04_12-08-15](//tva4.sinaimg.cn/large/005tpOh1gy1gchrnwhec1j30nx054dh9.jpg)

可见性修饰符指明类中的数据域和方法是否能在**该类之外**被访问 。 在**该类之内** ， 对数据
域和方法的访问是没有任何限制的

## 数据域的封装

>  将数据域设为私有保护数据 ， 并且使类易于维护

如果数据不进行`private`封装，会造成：

- 🅰数据可能被修改
- 🅱使类维护变困难，同时容易出现错误

对于数据域的类外对象是不能访问这个数据域的，使用

- 访问器`get方法`
- 修改器`set方法`

来访问数据，如一般有：

```java
public returnType getPropertyName()
public void setPropertyName(dataType propertyValue)
```

```java
public class CircleWithPrivateDataFields {
    public static void main(String[] args) {
        Circle circle1 = new Circle();
        Circle circle2 = new Circle(2.0);
        Circle circle3 = new Circle(3.0);
        System.out.println("圆的个数: " + Circle.getNumberOfObjects()); // 通过类名访问静态方法
        System.out.println(circle1.getRadius() + " 的面积为: " + circle1.getArea());
        System.out.println(circle2.getRadius() + " 的面积为: " + circle2.getArea());
        System.out.println(circle3.getRadius() + " 的面积为: " + circle3.getArea());
        double originalRadius = circle1.getRadius();
        circle1.setRadius(4.0);
        System.out.println("半径为：" + originalRadius + "的圆修改半径为：" + circle1.getRadius() +
                "\n修改后的面积为：" + circle1.getArea());
    }
}

class Circle{
    private double radius;              // 实例数据域
    private static int numberOfObjects; // 静态数据域

    public Circle(){                    // 无参构造函数
        radius = 1;
        numberOfObjects++;
    }

    public Circle(double newRadius){       // 带参构造函数
        radius = newRadius;
        numberOfObjects++;
    }

    public double getRadius(){          // 访问器
        return radius;
    }

    public void setRadius(double newRadius){   // 修改器
        radius = (newRadius >= 0) ? newRadius : 0;
    }

    public static int getNumberOfObjects(){ // 访问器 静态方法
        return numberOfObjects;
    }

    public double getArea(){
        return Math.PI * radius * radius;
    }
}
```

**Result**

```powershell
圆的个数: 3
1.0 的面积为: 3.141592653589793
2.0 的面积为: 12.566370614359172
3.0 的面积为: 28.274333882308138
半径为：1.0的圆修改半径为：4.0
修改后的面积为：50.26548245743669
```



## 向方法传递对象参数

> 给方法传递一个对象，是将对象的引用传递给方法，类似于数组

`Java`只有一种参数传递方式：`值传递`，而将对象传递给给方法，就是传递**引用值**

```java
public class TestPassObject {
    public static void main(String[] args) {
        Circle c = new Circle(1.0);
        int n = 5;
        printAreas(c, n);
        System.out.println("\n" + "Radius is " + c.getRadius());
        System.out.println("n is " + n);
    }

    public static void printAreas(Circle c, int times){
        System.out.println("Radius \t\tArea");
        while(times >= 1){
            System.out.println(c.getRadius() + "\t\t" + c.getArea());
            c.setRadius(c.getRadius() + 1);
            times --;
        }
    }
}
```

**Result**

```powershell
Radius 		Area
1.0		3.141592653589793
2.0		12.566370614359172
3.0		28.274333882308138
4.0		50.26548245743669
5.0		78.53981633974483

Radius is 6.0
n is 5
```

 引用上的传值在语义上最好描述为`传共享`  , 也就是说 ， 在方法中引用的对象和传递的对象是一样的 。

![Snipaste_2020-03-04_15-01-48](//tva3.sinaimg.cn/large/005tpOh1gy1gchwoinur8j30ig06x765.jpg)

## 对象数组

> 数组既可以存储基本类型数据，也可以存储对象

```java
Circle[] circleArray = new Circle[10];
for(int i = 0; i < circleArray.length; i++){
    circleArray[i] = new Circle();	// 使用new初始化数组元素
}
```

对象的数组实际上是`引用变量的数组`，因此，调用`circleArray[1].getArray()`实际上调用了两个层次的应用。   

 ![Snipaste_2020-03-04_16-09-21](//tvax2.sinaimg.cn/large/005tpOh1gy1gchynn98m6j30jo05cq4j.jpg)

**实例：利用对象数组求圆的数组的总面积**

```java
public class TotalAreaOfCircles {
    public static void main(String[] args) {
        Circle[] circleArray;
        circleArray = createCircleArray();
        printCircleArray(circleArray);
    }

    public static Circle[] createCircleArray(){
        Circle[] circleArray = new Circle[5];
        for (int i = 0; i < circleArray.length; i++){
            circleArray[i] = new Circle(Math.random() * 100);
        }
        return circleArray;
    }

    public static void printCircleArray(Circle[] circleArray){
        System.out.printf("%-30s%-15s\n", "Radius", "Area");
        for (int i = 0; i < circleArray.length; i++){
            System.out.printf("%-30.2f%-15.2f\n", circleArray[i].getRadius(),
                    circleArray[i].getArea());
        }
        System.out.println("----------------------------------------");
        System.out.printf("%-30s%-15.2f\n", "The total area of circles is",
                sum(circleArray));
    }

    public static double sum(Circle[] circleArray){
        double sum = 0;
        for (int i = 0; i < circleArray.length; i++){
            sum += circleArray[i].getArea();
        }
        return sum;
    }
}
```

**Result**

```powershell
Radius                        Area           
17.78                         993.67         
43.50                         5945.32        
92.78                         27044.81       
66.02                         13692.14       
63.30                         12587.48       
----------------------------------------
The total area of circles is  60263.42       
```

## 不可变对象和类

> 可以定义不可变类来产生不可变对象。不可变对象的内容不能被改变

如果一个类是不可变的 ， 那么它的**所有数据域**必须都是**私有的** ， 而且没有对任何一个数据域提供公共的 set 方法。**一个类的所有数据都是私有的且没有修改器并不意味着它一定是不可变的类 **，如下面例子：

```java
public static Student{
    private int id;
    private String name;
    private java.util.Date dateCreated;
    public Student(int ssn, String newName){
        id = ssn;
        name = newName;
        dateCreated = new java.util.Date();
    }
    public int getId(){
        return id;
    }
    public String getName(){
        return name;
    }
    public java.util.Date getDateCreated(){
        return dateCreated;
    }
}
```

```java
public class Test{
    public static void main(String[] args){
        Student stu = new Student(123456, "Gqq");
        java.util.Date dateCreated = student.getDateCeated();
        dateCreated.setTime(20000); // 此时，dateCreated 的数据域改变了！
    }
}
```

 使用 `getDateCreated()` 方法返回数据域 `dateCreated` 。 它是对`Date 对象`的一个`引用` 。 通过这个引用 ， 可以`改变 dateCreated` 的值

**要使一个类成为不可变的 ， 它必须满足下面的要求**

- 所有数据域都是`privated`
- 没有修改器`set`方法
- **没有一个返回指向可变数据域的引用的访问器的方法**

## 变量的作用域

> `类变量/数据域`：实例变量和静态变量，作用域是整个类
>
> `局部变量`：在方法内部定义的变量

类的变量和方法可以在类中以**任意顺序**出现， 但是当一个**数据域**是**基于**对**另一个数据域**的引用来进行初始化时则不是这样 。

```java
public class Circle{
    public double findArea(){
        return r * r * Math.PI;
    }
    private double r = 1;  	// 方法和变量任意顺序
}
```

```java
public class F{
    private int j;
    private int sum = j + 1;  // j必须在sum之前声明，因为sum的初始值依赖于j
}
```

- 类变量只能声明一次 ， 但是在一个方法内不同的非嵌套块中 ， 可以多次声明相同的变量名 
- 如果一个局部变量和一个类变量具有相同的名字, 那么局部变量优先 . 而同名的类变量将被隐藏

```java
public class F{
    private int x = 0;
    private int y = 1;
    public F(){  
    }
    public void p(){
        int x = 1;						// 这里 x 是局部变量
        System.out.println("x = " + x);
        System.out.println("y = " + y);
    }
}
```

**为避免混淆和错误 ， 除了方法中的参數 ， 不要将实例变量或静态变量的名字作为局部变量名**



## this引用

>  关键字 this 引用对象自身 。 它也可以在构造方法内部用于调用同一个类的其他构造方法

- `this引用`通常是可以省略的

![Snipaste_2020-03-04_17-02-32](//tvax2.sinaimg.cn/large/005tpOh1gy1gci064pdmwj30pl09hn0f.jpg)

-  在引用`隐藏数据域`以及调用一个`重载的构造方法`的时候 ，` this 引用`是必须的 。

### 使用this引用隐藏数据域

在数据域的`set方法`中，经常将数据域名用作形参，此时this不能省略

```java
public class F{
    private int i = 5;
    private static double k = 0;
    public void setI(int i){		// 形参也命名为 i
        this.i = i; 
    }
    public static void setK(double k){	// 形参也命名为 k
        F.k = k;						// 因为 k 是静态数据域，所以用类名调用 
    }
}
```

### 使用this调用构造方法

关键字` this `可以用于调用`同一个类`的`另一个构造方法`

```java
public class Circle{
    private double radius;
    public Circle(double radius){
        this.radius = radius; // this关键字用于引用所构建的对象的隐藏数据域radius
    }
    public Circle(){
        this(1.0);			// this用于调用另外一个构造方法，即上面那个构造方法
    }
}
```

- `Java` 要求在构造方法中 ， 语句` this ( 参数列表 ） `应在任何其他可执行语句**之前**出现
-  如果一个类有**多个**构造方法 ， 最好尽可能使用` this ( 参数列表 ）`实现它们