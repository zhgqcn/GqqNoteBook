---
title: Java基础-day10
date: 2020-02-23
categories: JavaSE
tags: Java
id: Gqq
---

# 1. 数组的查找

> 查找`searching`是在数组中寻找特定元素的过程，本次主要学习
>
> > 线性查找
> >
> > 二分查找

<!--more-->

## 1.1 线性查找法

> 线性査找法将要査找的关键字 key 与数组中的元素逐个进行比较

```java
public class LinearSearch{
    public static int linearSearch(int[] list, int key){
        for(int i = 0; i < list.length; i++){
            if(key == list[i])
                return i;
        }
        return -1;
    }
}
```

## 1.2 二分查找

> 二分查找法的前提条件是`数组有序`，然后从数组中间开始查找

- 如果关键字小于中间元素 ， 只需要在数组的前一半元素中继续査找关键字 
- 如果关键字和中间元素相等 ， 则匹配成功 ， 査找结束 
- 如果关键字大于中间元素 ， 只需要在数组的后一半元素中继续査找关键字 

复杂度为`O(logn)`

```java
public class BinarySearch{
    public static int binarySearch(int[] list, int key){
        int low = 0;
        int high = list.length - 1;
        while(high >= low){
            int mid = (low + high) / 2;
            if(key < list[mid])
                high = mid - 1;
            else if (key == list[mid])
                return mid;
            else 
                low = mid + 1;
        }
        return -low - 1;
    }
}
```

# 2. 数组的排序

> 本次主要学习`选择排序`，主要的思想在于`比较`和`交换`

```java
public class SelectSort {
    public static void main(String[] args) {
        int[] string = {1, 2, 10, 3, 2, 8, 4};
        for (int e:selectsort(string)     // 利用foreach遍历数组输出
             ) {
            System.out.print(e + " ");
        }

    }

    public static int[] selectsort(int[] string) {  // 从小到大
        for (int i = 0; i < string.length; i++) {
            for (int j = i + 1; j < string.length; j++) {
                if (string[j] < string[i]) {		// 比较
                    int temp;
                    temp = string[i];				// 交换
                    string[i] = string[j];
                    string[j] = temp;
                }
            }
        }
        return string;
    }
}
```

# 3. `Arrays`类

> `java.util.Arrays`类包含一些实用的方法用于常见的数组操作，比如**排序**和**查找**

## 3.1 排序

```java 
double[] numbers = {6.0, 1, 2, 3, 4.0};
java.util.Arrays.sort(numbers);			 // 整个数组排序
java.util.Arrays.sort(numbers, 1, 3);	 //	部分数组排序
```

如果计算机有多个***处理器***，则可以使用`parallelSort`

```java
char[] chars = {'a', 'b', '4', 'd'};
java.util.Arrays.parallelSort(chars);	// 整个排序
java.util.Arrays.parallelSort(chars, 1, 3); // 部分排序
```

## 3.2 查找

`二分查找`必须提前**升序**排列好，如果数组中不存在关键字，返回`-(插入点下标+1)`

```java
int[] list = {2 , 4 , 7 , 10 , 11 , 45 , 50 , 59 , 60 , 66 , 69 , 70 , 79};
System.out.println("1.Index is " +
                  java.util.Arrays.binarySearch(list, 11));
System.out.println("2.Index is " +
                  java.util.Arrays.binarySearch(list, 12));
```

**结果**

```java
1.Index is 4
2.Index is -6
```

## 3.3 `equals`检测数组是否相等

```java
System.out.println(java.util.Arrays.equals(list1, list2));
```

- 相同返回`true`
- 不同返回`false`

## 3.4 `fill`填充整个数组或者部分

```java
java.util.Arrays.fill(list, 5); // 将5填充到整个数组
java.util.Arrays.fill(list, 1, 5, 8); // 将8填充到下标1到5的位置
```

## 3.5 `toString`返回字符串

```java
int[] list = {1, 2, 3};
System.out.println(Arrays.toString(list));
```

**结果**

```java
显示 [1, 2, 3];
```

```java
import java.util.Arrays;
public class ArraysTest {
    public static void main(String[] args) {
        char[] mystring = {'z', 'g', 'q', 'a', 'c', 's'};
        System.out.println("Before sort:");
        printArrays(mystring);
        System.out.println("\nFind z in mystring:" + Arrays.binarySearch(mystring, 'z')); // 错误的二分查找，二分查找必须升序
        Arrays.sort(mystring);   // sort
        System.out.println("\nAfter sort:");
        printArrays(mystring);
        System.out.println("\nFind z in mystring after sort:" + Arrays.binarySearch(mystring, 'z')); // 正确二分查找，已经有序

        char[] mystring1 = new char[6];
        Arrays.fill(mystring1, 'a');
        printArrays(mystring1);
        System.out.println("\nWhether mystring == mystring1 :" + Arrays.equals(mystring, mystring1));

    }
    public static void printArrays(char[] string){
        for (int i = 0; i < string.length; i++){
            System.out.print(string[i]);
        }
    }
}

```



# 4. 命令行参数

> `main`方法可以从命令行接收字符串参数

```java
public class TestMain{
    public static void main(String[] args){
        for(int i = 0; i < args.length; i++){
            System.out.println(args[i]);
        }
    }
}
```

在命令行中运行该类，并对`main`函数传入参数

```shell
java TestMain "First Name" alpah 53
```

- 当字符串有空格时，应用引号引起
- 其他可以不用引号，直接拼写
- 53不是数字，而是字符串

当调用 `main `方法时 ，` Java 解释器`会创建一个数组存储命令行参数 ， 然后将该数组的引
用传递给 `args`，如上面例子创建了`args = new String[3];` 

>  如果运行程序时**没有**传递字符串 ， 那么使用`new String [ 0 ]` 创建数组 。 在这种情况
> 下 ， 该数组是长度为 0 的空数组 。 `args` 是对这个空数组的引用 。 因此 ， `args` 不是 `null` ,但是 `args.length` 是 0 

## 4.1 示例学习: 计算器

利用`main`的参数，完成如下输入

```java
java Calculator 2 + 3
结果：
    2 + 3 = 5
```

```java
public class Calculator {
    public static void main(String[] args) {
        if (args.length != 3){
            System.out.println("Error Input!");
            System.exit(0);
        }

        int result = 0;
        switch (args[1].charAt(0)){
            case '+': result = Integer.parseInt(args[0]) +
                            Integer.parseInt(args[2]);
            break;
            case '-': result = Integer.parseInt(args[0]) -
                            Integer.parseInt(args[2]);
            break;
            case '.': result = Integer.parseInt(args[0]) *
                            Integer.parseInt(args[2]);
            break;
            case '/': result = Integer.parseInt(args[0]) /
                            Integer.parseInt(args[2]);
            break;
        }
        System.out.println(" " + args[0] + args[1] + args[2] + " = " + result);
    }
}
```

**结果**

![Snipaste_2020-02-23_21-27-49](//tva4.sinaimg.cn/large/005tpOh1gy1gc6nnz9ch2j30k20203yj.jpg)

