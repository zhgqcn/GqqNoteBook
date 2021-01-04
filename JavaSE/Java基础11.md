---
title: Java基础-day11
date: 2020-02-25
categories: JavaSE
tags: Java
id: Gqq
---

# 多维数组

> 表格或者矩阵中的数据可以表示为二维数组

<!--more-->

## 二维数组基础知识

### 声明变量与创建

```java
数据类型[][] 数组名;
或者
数据类型 数组名 [][]; // 不推荐
```

```java
int[][] matrix;
matrix = new int[5][5];
```

二维数组中，第一个表示行，第二个表示列，访问如下：

 ![Snipaste_2020-02-25_14-20-59](//tvax3.sinaimg.cn/large/005tpOh1gy1gc8mk47dxlj30ki07idiq.jpg)

可以使用数组初始化来声明、创建、初始化一个二维数组，如上图**第三个**，其等价于：

```java
int[][] array = new int[4][3];
array[0][0] = 1; array[0][1] = 2; array[0][2] = 3;
array[1][0] = 4; array[1][1] = 5; array[1][2] = 6;
array[2][0] = 7; array[2][1] = 8; array[2][2] = 9; 
array[3][0] = 10;array[3][1] = 11;array[3][2] = 12;
```

### 获取二维数组的长度

二维数组实际上是一个数组，它的每个元素都是一个**一维数组**。数组***x***的长度是数组中元素的个数，可以使用`x.length`获取该值。

```java
x = new int[3][4];
// x[0] x[1] x[2] 都是一维数组，每个数组都包含4个元素
// length = 3
// x[0].length = x[1].length = x[2].length = 4;
```

### 锯齿数组

二维数组的每一行可以不同长度，这样的数组称为`锯齿数组`

```java
int[][] triangleArray = {
    {1, 2, 3, 4, 5},			// triangleArray[0].length = 5
    {2, 3, 4, 5},				// triangleArray[1].length = 4
    {3, 4, 5},					// triangleArray[2].length = 3
    {4, 5},						// triangleArray[3].length = 2
    {5}							// triangleArray[4].length = 1
};
```

**如果事前不知道锯齿数组的值，但是知道它的长度，创建为下：

```java
int[][] triangleArray = new[5][];
triangleArray[0] = new int[5];
triangleArray[1] = new int[4];
triangleArray[2] = new int[3];
triangleArray[3] = new int[2];
triangleArray[4] = new int[1];
```

### 处理二维数组

> 嵌套的for循环常用于处理二维数组

```java
import java.util.Scanner;
public class Matrix {
    public static void main(String[] args) {
        final int ROW = 3;
        final int COL = 2;
        Scanner input = new Scanner(System.in);
        int[][] matrix = new int[ROW][COL];
        int[][] matrix1 = new int[ROW][COL];
        // 通过输入值初始化数组
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                matrix[i][j] = input.nextInt();
            }
        }
        // 通过随机值初始化数组
        for (int i = 0; i < matrix1.length; i++){
            for (int j = 0; j < matrix1[0].length; j++){
                matrix1[i][j] = (int)(Math.random() * 100); // 随机产生0到99
            }
        }
        // 打印输出二维数组
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
        // 求所有元素的和
        int sum = 0;
        for (int i = 0; i < matrix.length; i++){
            for (int j =0; j < matrix[0].length; j++){
                sum += matrix[i][j];
            }
        }
        System.out.println("The sum is : " + sum);
        // 对数组按列求和
        int[] colSum = new int[COL];
        for (int i = 0; i < matrix[0].length; i++){
            for (int j = 0; j < matrix.length; j++){
                colSum[i] += matrix[j][i];
            }
        }
        System.out.println("按列求和为: ");
        for (int i = 0; i < colSum.length; i++){
            System.out.print(colSum[i] + " ");
        }
        System.out.println();
        // 哪一行的值最大
        int maxRow = 0;             // 跟踪和的最大值
        int indexOfMaxRow = 0;      // 跟踪所在行

        for (int i = 0; i < matrix.length; i++){
            int thisRow = 0;
            for (int j = 0; j < matrix[0].length; j++){
                thisRow += matrix[i][j];
            }
            if (maxRow < thisRow){
                maxRow = thisRow;
                indexOfMaxRow = i;
            }
        }
        System.out.println("最大值行为: " + indexOfMaxRow + " 该行和为: " + maxRow);
        // 随意交换
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                int i_ = (int)(Math.random() * matrix.length);
                int j_ = (int)(Math.random() * matrix[0].length);
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i_][j_];
                matrix[i_][j_] = temp;
            }
        }
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

### 将二维数组传递给方法

> 将一个二维数组传递给方法的时候，数组的引用传递给方法

```java
import java.util.Scanner;

public class PassTwoDimensionArrays {
    public static void main(String[] args) {
        int[][] myString = getArray();
        System.out.println("The sum of myString is : " + sum(myString));
    }
    public static int[][] getArray(){
        Scanner input = new Scanner(System.in);
        System.out.print("Input row and col :");
        int row = input.nextInt();
        int col= input.nextInt();
        int[][] myArray = new int[row][col];
        for (int i = 0; i < myArray.length; i++){
            for (int j = 0; j < myArray[0].length; j++){
                myArray[i][j] = input.nextInt();
            }
        }
        return myArray;
    }
    public static int sum(int[][] string){
        int sum = 0 ;
        for (int i = 0; i < string.length; i++){
            for (int j = 0; j < string[0].length; j++){
                sum += string[i][j];
            }
        }
        return  sum;
    }
}
```

## 实例学习

### 找出距离最近的点对

- 输入点的个数
- 计算点对之间的距离，并找到最小值
- 打印输出点对

```java
import java.util.Scanner;

public class FindNearestPoints {
    public static void main(String[] args) {
        double[][] myPoints = getPoints();
        int p1 = 0;
        int p2 = 1;
        double min = distance(myPoints[p1], myPoints[p2]);
        for (int i = 0; i < myPoints.length; i++){
            for (int j = i + 1; j < myPoints.length; j++){
                double dis = distance(myPoints[i], myPoints[j]);
                if (dis < min){
                    min = dis;
                    p1 = i;
                    p2 = j;
                    }       // end if
                }           // end for
            }               // end for
        System.out.println("the min distance is :" + min);
        System.out.println("They are " + "(" + myPoints[p1][0] + ", " + myPoints[p1][1] + ")" +
                " and " + "(" + myPoints[p2][0] + ", " + myPoints[p2][1] + ")");
        }


    public static double[][] getPoints(){
        Scanner input = new Scanner(System.in);
        System.out.print("Enter the number of points : ");
        int numberOfPoints = input.nextInt();
        double[][] myPoints = new double[numberOfPoints][2];
        for (int i = 0; i < myPoints.length; i++){
            for (int j = 0; j < myPoints[0].length; j++){
                myPoints[i][j] = input.nextDouble();
            }
        }
        return myPoints;
    }
    public static double distance(double[] p1, double[] p2){
        return Math.sqrt((p1[0] -p2[0]) * (p1[0] -p2[0]) +
                         (p1[1] - p2[1]) * (p1[1] - p2[1]));
    }
}
```

**结果**

```java
Enter the number of points : 8
-1 3 -1 -1 1 1 2 0.5 2 -1 3 3 4 2 4 -0.5 
the min distance is :1.118033988749895
They are (1.0, 1.0) and (2.0, 0.5)
```

### 判断数独是否正确

- 每一行、每一列、每一个3*3的正方形都由1到9的数组成
- 每一行、每一列、每一个3*3的正方形都没有重发的数字

```java
import java.util.Scanner;
public class CheckSudoSolution {
    public static void main(String[] args) {
        int[][] sudo = getSudoAnswer();
        System.out.println((isValid(sudo) ? "Wrong" : "Correct"));

    }
    public static int[][] getSudoAnswer(){
        Scanner input = new Scanner(System.in);
        int[][] answer = new int[9][9];
        System.out.println("Input your answer :");
        for (int i = 0; i < 9; i++){
            for (int j = 0; j < 9; j++){
                answer[i][j] = input.nextInt();
            }
        }
        return answer;
    }
    public static boolean isValid(int[][] string){
        for (int i = 0; i < string.length; i++){
            for (int j = 0; j < string[0].length; j++){
                if (string[i][j] < 1 || string[i][j] > 9 || isValid(i, j, string))
                    return false;
            }
        }
        return true;
    }
    public static boolean isValid(int i, int j, int[][] string){
        for (int row = 0; row < string.length; row++){
            if (row != i && string[row][j] == string[i][j])
                return false;
        }
        for (int col = 0; col < string[0].length; col++){
            if (col != j && string[i][col] == string[i][j])
                return false;
        }
        for (int row = (i / 3) * 3; row < (i / 3) * 3 + 3; i++){
            for (int col = (j / 3) * 3; col < (j / 3) * 3 + 3; j++){
                if (row != i && col != j && string[row][col] == string[i][j])
                    return false;
            }
        }
        return true;
    }
}
```

## 多维数组

> 二维数组由一个一维数组的数组组成
>
> 而一个三维数组可以认为是一个二维数组的数组组成

```java
double[][][] scores = new double[6][5][2];
double[][][] scores = {					     //score[0]是二维数组，含有5个一维数组
    {{7, 8}, {1, 2}, {3, 4}, {5, 6}, {3, 2}},//score[0][0]是一维数组，含有2个元素
    {{7, 8}, {1, 2}, {3, 4}, {5, 6}, {3, 2}},
    {{7, 8}, {1, 2}, {3, 4}, {5, 6}, {3, 2}},
    {{7, 8}, {1, 2}, {3, 4}, {5, 6}, {3, 2}},
    {{7, 8}, {1, 2}, {3, 4}, {5, 6}, {3, 2}},
    {{7, 8}, {1, 2}, {3, 4}, {5, 6}, {3, 2}},
}
```

