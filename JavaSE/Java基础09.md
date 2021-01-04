---
title: Java基础-day09
date: 2020-02-20
categories: JavaSE
tags: Java
id: Gqq
---

# 一维数组

> 单个的数组变量可以引用一个大的数据集合

## 1. 基础知识

> 一旦数组被创建，它的大小是固定的，使用一个数组引用变量，通过下标来访问数组中的元素<!--more-->

### 1.1 声明数组变量

```java
elementType[] arrayRefVar; (元素类型[] 数组引用变量;)
e.g
    double[] mylist;
```

声明数组不在内存中分配空间，它只是创建一个`对数组的引用`的存储位置

### 1.2 创建数组

```java
arrayRefVar = new elementType[arraySize];
```

- [x] 使用`new elementType[arraySize]`创建一个数组
- [x] 把新创建的数组的`引用赋值`给变量`arrayRefVar`

**声明一个数组变量、创建数组、然后将数组引用赋值给变量**这三个步骤可以合并成一条：

```java
elementType[] arrayRefVar = new elementType[arraySize];
元素类型[] 数组引用变量 = new 元素类型[数组大小]
```

```java
double[] myList = new double[10];
```

**赋值**为：

```java
arrayRefVar[index] = value;
```

在java中，数组变量时一种引用，如`myList`是一个含有10个`double`型元素数组的引用变量 

### 1.3 数组大小和默认值

在分配数组时候，必须指定该数组能够存储的元素个数。创建数组之后，就不能再修改它的大小。可以使用`arrayRefVar.length`得到数组大小。

当创建数组后，它的元素被赋予默认值。`数值型`基本数据类型的默认值为`0`，`char`型的默认值为`'\u0000'`，`boolean型`的默认值为`false`

### 1.4 访问数组元素

通过`下标`从`0`到`length - 1`来访问数组

```java
arrayRefVar[index]; (数组引用变量[下标];)
```

### 1.5 数组初始化

```java
elementType[] arrayRefVar = {value0, value1, ..., valuek};
```

数组初始化必须像上面语法一样，包含`声明数组` ，`创建数组`，`初始化数组` 结合在一条语句中，此时数组含有`k`个元素，从`0`到`k-1`。 数组初始化语法中不使用操作符 `new`

### 1.6 处理数组

经常使用`for`循环

1. 数组中所有元素都是同一个类型
2. 数组大小已知

> 对于`char[]`类型的数组，可以使用一条打印语句打印

```java
char[] iloveu = {'I', ' ', 'L', 'o', 'v', 'e', ' ', 'U'};
System.out.println(iloveu);
```

**应用**

`随机打乱（shuffling)`

```java
for(int j = myList.length - 1; j > 0; j--){
    int k = (int)(Math.random() * (i + 1));
    double temp = myList[i];
    myList[i] = myList[k];
    myList[k] = temp;
}
```

`移动元素`

```java
double temp = mylist[0];
for(int i = 1; i < mylist.length; i++){
    mylist[i - 1] = mylist[i];
}
mylist[mylist.length - 1] = temp;
```

`简化编码`

```java
String[] months = {"Jan", "Feb", ..., "Dec"};
System.out.print("Enter a month nunber:");
int monthNumber = input.nextIn();
System.out.println("The month is" + months[monthNumber - 1]);
```

### 1.7 `foreach`循环

对于`foreach`循环，可以不使用下标变量就可以顺序地遍历整个数组

```java
for(double e: myList){
    System.out.println(e);
}
```

### 1.8 练习题

#### 1.8.1 分析数字

找到大于所有项平均值的那些项

```java
import java.util.Scanner;
public class AnalyzeNumber {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int[] str = new int[10];
        System.out.println("Enter 10 numbers:");
        int total = 0;
        for (int j = 0; j < str.length; j++) {
            str[j] = input.nextInt();
            total += str[j];
        }
        int average = total / str.length;
        String remembers = "";
        for(int j = 0; j < str.length; j++){
            if (str[j] > average){
                remembers += str[j] + " ";
            }
        }
        System.out.print("大于" + average + "的数有");
        System.out.println(remembers);
    }
}
 
```

**结果**

```java
Enter 10 numbers:
1 2 3 4 5 6 7 8 9 10
大于5的数有6 7 8 9 10 
```

#### 1.8.2 一副牌

从一副牌中随处选出4张，判断花色和牌号

```java
public class DeckOfCards {
    public static void main(String[] args) {
        String[] color = {"黑桃", "红桃", "方块", "梅花"};
        String[] number = {"A", "2", "3", "4", "5", "6", "7", "8", "9",
        "10", "J", "Q", "K"};
        int[] deck = new int[52];
        for(int j = 0; j < deck.length; j++){
            deck[j] = j;                        // 初始化，从0到51共52张牌
        }
        // 打乱排序
        for(int j = 0; j < deck.length; j++){
            int site = (int)(Math.random() * deck.length);
            int temp = deck[j];
            deck[j] = deck[site];
            deck[site] = temp;
        }
        //
        for ( int i = 0 ; i < 4 ; i++) {
            String suit = color [ deck [ i ] / 13 ] ;
            String rank = number [ deck [ i ] % 13 ] ;
            System . out . println (deck[i] + "号牌是：" + suit + " " + rank) ;
            }

    }
}

```

**结果**

```java
12号牌是：黑桃 K
46号牌是：梅花 8
40号牌是：梅花 2
4号牌是：黑桃 5
```

### 1.9 数组的复制

**要将一个数组中的内容复制到另一个中，需要将数组的每一个元素复制到另一个数组中**，不能直接使用语句`list2 = list1`，这是***非常严重的错误***，会导致`list2`原先所引用的数组不能再引用。

复制数组的三种方法：

- 使用循环语句逐个

```java
int[] sourceArray = {2, 3, 4, 5};
int[] targetArray = new int[sourceArray.length];
for(int i = 0; i < sourceArray.length; i++){
    targetArray[i] = sourceArray[i];
}
```

- 使用`System`类中的静态方法`arraycopy`

```java
arraycopy(源数组，源数组起始位置，目标数组，目标数组起始位置，复制长度);
e.g
    System.arraycopy(sourceArray, 0, targetArray, 0, sourceArray.length);
```

`arraycopy`方法没有给目标数组分配内存空间，复制前必须创建目标数组以及分配给它的内存空间。复制完成后，`sourceArray`和`targetArray`具有相同的内容，但是占有独立的内存空间。

- 使用`clone`方法复制数组



## 2.数组与方法

### 2.1 将数组传递给方法

> 当将一个数组传递给方法时，数组的引用被传给方法

```java
public static void printArray(int[] array){
    for(int i = 0; i < array.length; i++){
        System.out.print(array[i] + " ");
    }
}
```

调用如下：

```java
printArray(new int[]{3, 1, 2});
```

像上面这样创建的数组叫做`匿名数组`，创建语句为`new elementType[]{value0, value1, ..., valuek}`

`Java`使用`按值传递`的方式将实参传递给方法。传递基本数据类型变量的值和传递数组值大有不同：

* 对于基本数据类型参数，传递的是实参的值
* 对于数组类型参数，参数值是`数组的引用`

![Snipaste_2020-02-22_17-07-57](//tvax1.sinaimg.cn/large/005tpOh1gy1gc5aj6ysl3j30jr06xju7.jpg)



### 2.2 从方法中返回数组

> 当从方法中返回一个数组时，数组的引用被返回

如下面，返回一个与输入数组元素顺序相反的数组：

```java
public static int[] reverse(int[] list){
    int[] result = new[list.length - 1];
    for(int i = 0, j = list.length - 1; i < list.length; i++, j--){
        result[j] = list[i];
    }
    return result;
}
int[] list1 = {1, 2, 3};
int[] list2 = reverse(list1)
```

### 2.3 实例： 统计每个字母出现的次数

利用hash表来统计字母出现次数

```java
public class CountLettersInArray {
    public static void main(String[] args) {
        char[] myString = new char[100];
        int[]  myCount = new int[26];
        myCount = getArray(myString);
        printTable(myString, myCount);
    }
    // 返回随机字符
    public static char getRandomLetter(char ch1, char ch2){
        return (char)(ch1 + (Math.random() * (ch2 - ch1 + 1)));
    }

    public static int[] getArray(char[] string){
        int[] count = new int[26];
        for (int i = 0; i < string.length; i++){
            string[i] = getRandomLetter('a', 'z');
            count[string[i] - 'a'] ++;
        }
        return count;
    }
    // 打印输出
    public static void printTable(char[] string, int[] count){
        System.out.print("100 个字母如下");
        for(int i = 0; i < string.length; i++){
            if (i % 20 == 0)System.out.println();
            System.out.print(string[i] + " ");
        }
        System.out.print("\n26 个字母出现的次数");
        for(int i = 0; i < 26; i++){
            if (i % 10 == 0)System.out.println();
            System.out.print(count[i] + " " + (char)('a' + i) + " ");
        }
    }
}
```

**结果：**

```java
100 个字母如下
i u k g a l f d s u q z l n z o w e k m 
c r o d q q e s l f l r c y j v m q v a 
g o v z n y t n a v m i b q o t i o r o 
m h l g d q m n t j j z h x c a g m l k 
r k d q z z p q m u c i z w h u s n y s 
26 个字母出现的次数
4 a 1 b 4 c 4 d 2 e 2 f 4 g 3 h 4 i 3 j 
4 k 6 l 7 m 5 n 6 o 1 p 8 q 4 r 4 s 3 t 
4 u 4 v 2 w 1 x 3 y 7 z 
```



## 3. 可变长参数列表

> 具有同样类型的可变长度的参数可以传递给方法，并将作为数组对待

**方法**中的参数声明如下

```java
typeName... parameterName(类型名...参数名)
```

- 指定类型后面紧跟省略号
- 一个方法只能指定一个可变长度参数
- 任何常规参数必须放在可变长参数前面

```java
public class VarArgsDemo{
    public static void main(String[] args){
        printMax(34, 3, 3, 2, 56.2);
        printMax(new double[]{1, 2, 3});
    }
    public static void printMax(double... numbers){
        if(number.length == 0){
            System.out.println("NO argument passed");
            return;
        }
        double result = number[0];
        for(int i = 1; i < numbers.length; i++){
            if(numbers[i] > result)
                result = numbers[i];
        }
        System.out.println("The max value is " + result);
    }
}
```



