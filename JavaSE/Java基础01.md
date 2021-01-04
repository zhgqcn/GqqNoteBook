---
title: Java基础-day01
date: 2020-02-06 
categories: JavaSE
tags: Java
id: Gqq
---


在虚拟机上配置了Java的开发环境，下载了IntelliJ IDEA

第一次使用不懂得RUN，在杨某帮助下，成功解决了

1. 创建Project

2. 将项目设置为 源根目录（source root）<!--more-->

![photo1](https://wx3.sinaimg.cn/mw690/005tpOh1gy1gbnt7xa7z3j30s90n3jwp.jpg)

3. file放在src文件夹下面

4. 公共类名要和文件名相同

![img2](https://wx4.sinaimg.cn/mw690/005tpOh1gy1gbnt7rnedpj30rg03r75c.jpg)

5. 代码

```java
public class Welcome{
public static void main(String[]args){
        System.out.println("Welcome to Java\n");
        System.out.println("Today is 2020-02-05\n");
        System.out.println("Gqq 奥利给！");
        }
        }
```

6. Java源代码执行过程

![img3](https://tvax1.sinaimg.cn/large/005tpOh1gy1gbntafvahyj30pz0bvdib.jpg)

执行语句：

```
javac Welcome.java
java Welcome
```

