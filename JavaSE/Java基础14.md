---
title: Java基础-day14
date: 2020-03-06
categories: JavaSE
tags: Java
id: Gqq
---

# 面向对象思考

## 类的抽象和封装

> 类的抽象是指将类的实现和类的使用分离开，实现的细节被封装并且对用户隐藏，这被称为类的封装<!--more-->

- 从类外可以访问的方法和数据域的集合以及预期这些成员如何行为的描述 ， 合称为`类的合约`

- 类的使用者不需要知道类是如何实现的, 实现的细节经过封装, 对用户隐藏起来, 这称为`类的封装`

- 类也称为`抽象数据类型(Abstract Date Type，ADT)`

**实例：贷款**

`Loan类`的`UML类图`如下：

![Snipaste_2020-03-06_14-47-22](//tvax1.sinaimg.cn/large/005tpOh1gy1gck7i9wtu9j30l80f8gvn.jpg)

**Loan.java**

```java
import java.util.Date;

public class Loan {
    private double annualIntersetRate; 	// 年利率
    private int numberOfYears;          // 贷款年数
    private double loanAmount;          // 贷款金额
    private Date loanDate;    			// 贷款日期

    // 默认构造函数
    Loan(){
        this(2.5, 1, 1000);				// 调用含参数构造函数
    }
    // 带参数构造函数
    // 参数为 年利率 贷款年数 贷款金额
    Loan(double annualIntersetRate, int numberOfYears, double loanAmount){
        this.annualIntersetRate = annualIntersetRate;
        this.numberOfYears = numberOfYears;
        this.loanAmount = loanAmount;
        this.loanDate = new Date();
    }
    // 返回贷款年利率
    public double getAnnualIntersetRate(){
        return annualIntersetRate;
    }
    // 设置贷款年利率
    public void setAnnualIntersetRate(double annualIntersetRate){
        this.annualIntersetRate = annualIntersetRate;
    }
    // 返回贷款年数
    public int getNumberOfYears(){
        return numberOfYears;
    }
    // 设置贷款年数
    public void  setNumberOfYears(int numberOfYears){
        this.numberOfYears = numberOfYears;
    }
    // 返回贷款金额
    public double getLoanAmount(){
        return loanAmount;
    }
    // 设置贷款金额
    public void setLoanAmount(double loanAmount){
        this.loanAmount = loanAmount;
    }
    // 返回贷款日期
    public Date getLoanDate(){
        return loanDate;
    }
    // 返回这笔贷款的月支付额
    public double getMonthlyPayment(){
        double monthlyInterestRate = annualIntersetRate / 12 / 100; // 百分数形式
        double monthlyPayment = loanAmount * monthlyInterestRate / (1 - (1 / Math.pow(1 +
                monthlyInterestRate, numberOfYears * 12)));
        return  monthlyPayment;

    }
    // 返回总支付额
    public double getTotalPayment(){
        return getMonthlyPayment() * numberOfYears * 12;
    }
}
```

**TestLoanClass**

```java
import java.util.Scanner;

public class TestLoanClass {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.printf("输入年利率:");
        double annaulInterestRate = input.nextDouble();
        System.out.print("输入贷款年数: ");
        int numberOfYears = input.nextInt();
        System.out.print("输入贷款金额: ");
        double loanAmount = input.nextDouble();

        Loan loan = new Loan(annaulInterestRate, numberOfYears, loanAmount);
        System.out.printf("\n贷款时间为: %s\n" +
                "月支付额度为: %.2f\n" + "总支付额度为: %.2f\n",
                loan.getLoanDate().toString(), loan.getMonthlyPayment(), loan.getTotalPayment());
    }
}
```



## 面向对象的思考

>  面向过程的范式重点在于设计方法 。 面向对象的范式将数据和方法耦合在一起构成对象 。 使用面向对象范式的软件设计**重点**在***对象以及对对象的操作上***



## 类的关系

>  类中间的关系通常是`关联` 、` 聚合` 、` 组合`以及`继承` 

### 关联

关联是一种常见的二元关系，用于描述两个类之间的活动

![Snipaste_2020-03-06_15-50-09](//tva2.sinaimg.cn/large/005tpOh1gy1gck9beh2e8j30m703x0ur.jpg)

- 关联由两个类之间的实线表示 ， 可以有一个可选的标签描述关系 。标签是`Take `和`Teach `。 每个关系可以有一个可选的**小的黑色三角形**表明关系的方向 。 在该图中 ， 方向表明学生选取课程 （ 而不是相反方向的课程选取学生 ） 。

- 关联中涉及的每个类可以有一个角色名称 ， 描述在该关系中担当的角色
- 关联中涉及的每个类可以给定一个`多重性 （ multiplicity ) `, 放置在类的边上用于给定 UML
  图中关系所涉及的类的对象数
  - \* 表示无穷多个对象
  - m..n 表示对象处于[m, n]之间
  - 也可以是一个具体的数，如1

![Snipaste_2020-03-06_15-58-12](//tva4.sinaimg.cn/large/005tpOh1gy1gck9jtjsc1j30po07tgoz.jpg)

### 聚集和组合

聚集是关联的一种特殊形式 ， 代表了两个对象之间的**归属关系**

 聚集建模` has - a` 关系, 所有者对象称为`聚集对象` ， 它的类称为`聚集类` 。 而从属对象称为`被聚集对象` ， 它的类称为`被聚集类`

— 个对象可以被多个其他的聚集对象所拥有 

 如果一个对象只归属于一个聚集对象 ， 那么它和聚集对象之间的关系就称为`组合`，即`一对一`

![Snipaste_2020-03-06_16-05-58](//tvax4.sinaimg.cn/large/005tpOh1gy1gck9rwc41qj30i00490tg.jpg)

在上图中 ， 每个学生只能有一个地址 ， 而每个地址最多可以被 3 个学生共享 。 每个学生都有一个名字 ， 而每个学生的名字都是唯一的

`聚集`可以存在于**同一类**的**多个对象**之间 。 例如 ： 一个人可能有一个管理者

![Snipaste_2020-03-06_16-08-31](//tva1.sinaimg.cn/large/005tpOh1gy1gck9uj2vxmj308o03tmxm.jpg)

```java
public class Person{
    private Person supervis;
}
```

如果一个人可以有多个管理者，则可以用一个数组存储管理者

```java
public class Person{
    private Person[] supervisors;
}
```



## 示例学习

### 设计Course类

> 设计一个类对课程建模

![111111111111111111111111111111111111111111111111111111](//tva3.sinaimg.cn/large/005tpOh1gy1gcr3ttd5cuj30m109fjwe.jpg)

**Course类**

```java
public class Course {
    private String courseName;
    private String[] students = new String[100];
    private int numberOfStudents;

    public Course(String courseName){
        this.courseName = courseName;
    }

    public String getCourseName(){
        return courseName;
    }

    public void addStudents(String student){
        students[numberOfStudents] = student;
        numberOfStudents ++;
    }

    public void dropStudent(String student){
        int k = 0;
        for (int i = 0; i < numberOfStudents; i++){
            if (student.compareTo(students[i])== 0){
                k = i;
                break;
            }
        }
        for (; k < numberOfStudents; k++){
            students[k] = students[k + 1];
        }
        numberOfStudents--;
    }

    public String[] getStudents(){
        return students;
    }

    public int getNumberOfStudents(){
        return numberOfStudents;
    }
}
```

**TestCourse类**

```java
import java.util.Scanner;

public class TestCourse {
    public static void main(String[] args) {
        Course mycourse = new Course("Java程序设计");
        Scanner input = new Scanner(System.in);
        System.out.print(mycourse.getCourseName() + "课程的选课学生数: ");
        int numberOfStudent = input.nextInt();
        System.out.println("输入" + numberOfStudent + "学生姓名：");
        for (int j = 0; j < numberOfStudent; j++) {
            String stu = input.next();
            mycourse.addStudents(stu);
        }
        System.out.println("选课学生数:" + mycourse.getNumberOfStudents());
        System.out.println("选课学生姓名如下：");
        PrintStudent(mycourse);

        System.out.print("输入要删除学生：");
        String dropStu = input.next();
        mycourse.dropStudent(dropStu);
        System.out.println("删除" + dropStu + "后:");
        PrintStudent(mycourse);
    }
    public static void PrintStudent(Course mycourse){
        String[] stu = mycourse.getStudents();
        for (int j = 0; j < mycourse.getNumberOfStudents(); j++){
            System.out.println(stu[j]);
        }
    }
}
```

**Result**

```shell
Java程序设计课程的选课学生数: 5
输入5学生姓名：
yzy
zgq
wjy
ymw
cjz
选课学生数:5
选课学生姓名如下：
yzy
zgq
wjy
ymw
cjz
输入要删除学生：yzy
删除yzy后:
zgq
wjy
ymw
cjz
```

### 设计栈类

> 设计一个类来对栈建模
>
> > 栈（stack）：是一种`后进先出`数据结构

为简单起见，假设该栈存储`int`数值

![Snipaste_2020-03-12_15-12-36](//tva1.sinaimg.cn/large/005tpOh1gy1gcr5y8x6iwj30ll09l79m.jpg)

**StackOfIntegers**

```java
public class StackOfIntegers { // 整型栈
    private int[] elements;     // 一个存储栈中整数的数组
    private int size;           // 栈中整数的个数

    public StackOfIntegers(){    // 构建一个默认容量为16的栈
        this(16);
    }

    public StackOfIntegers(int capacity){  // 构建一个指定容量的栈
        elements = new int[capacity];
    }

    public boolean empty(){         // 如果栈空返回true
        return (size == 0) ? true : false;
    }

    public int peek(){          // 查看栈顶元素
            return elements[size - 1];
    }

    public void push(int value){
        if (size >= elements.length){   // 如果栈满了，则创建一个容量为当前容量2倍的新数组
            int[] temp = new int[elements.length * 2];
            System.arraycopy(elements, 0, temp, 0, elements.length); // 将当前数组内容复制给新数组
            elements = temp;
        }
        elements[size] = value;
        size ++;
    }

    public int pop(){
        return elements[--size];
    }

}
```

**TestStack**

```java
public class TestStack {
    public static void main(String[] args) {
        StackOfIntegers stack = new StackOfIntegers();

        for (int i = 0; i < 10; i++){
            stack.push(i);
        }

        while (!stack.empty()){
            System.out.print(stack.pop() + " ");
        }
    }
}

```

**Result**

```powershell
9 8 7 6 5 4 3 2 1 0 
```



