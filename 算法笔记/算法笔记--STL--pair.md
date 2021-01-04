---
title: 算法笔记--标准模板库STL--pair
date: 2020-03-28 17:40:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# pair的常见用法

> pair是一个很实用的“小玩意”，当想要将两个元素绑在一起作为一个合成元素、又不想
> 要因此定义结构体时，使用pair可以很方便地作为一个代替品。也就是说，pair实际上可以
> 看作一个内部有两个元素的结构体，且这两个元素的类型是可以指定的，如下面的短代码所
> 示：

```cpp
struct pair{
    typeName1 first;
    typeName2 second;
}
```

<!--more-->

# pair的定义

**头文件**

```cpp
#include<utility>	 
using namespace std;
```

因为实现map时候已经使用了pair，所以记不住`utility`时，可以使用map的头文件

**定义**

```cpp
pair<typeName1, typeName2> name;

pair<string, int> p;
```

**如果想在定义pair时进行初始化，只需要跟上一个小括号**

```cpp\
pair<string, int> p("gqq", 5);
```

**如果想在代码中构建**`临时的`**pair**，**有以下两种方法：**

1. 将类型定义写在前面，后面用小括号内两个元素的方式

```cpp
pair<string, int>("gqq", 5);
```

2. 使用自带的`make_pair函数`

```cpp
make_pair("gqq", 5);
```

# pair中的元素访问

> pair中只有两个元素，分别为first和second，只需要安装正常结构体的方式访问即可

```cpp
#include<iostream>
#include<utility>
#include<string>
using namespace std;

int main(){
    pair<string, int> p;
    p.first = "zgq";
    p.second = 5;
    cout << p.first << " " << p.second << endl;
    p = make_pair("yzy", 55);
    cout << p.first << " " << p.second << endl;
    p = pair<string, int>("wjy", 555);
    cout << p.first << " " << p.second << endl;
    return 0;
}
```

```shell
zgq 5
yzy 55
wjy 555
```

# pair常用函数

## 比较操作数

> 两个pair类型数据可以直接用 `==、!=、<、<=、>、>=`比较大小，比较规则是**先以first的大小作为标准，只有当first相等时才去判断second的大小**

```cpp
#include<iostream>
#include<utility>
#include<string>
using namespace std;

int main(){
    pair<int, int> p1(5, 10);
    pair<int, int> p2(5, 15);
    pair<int, int> p3(10, 5);
    if(p1 < p3) printf("p1 < p3\n");
    if(p1 <= p3) printf("p1 <= p3\n");
    if(p1 < p2) printf("p1 < p2\n");
    return 0;
}
```

```shell
p1 < p3
p1 <= p3
p1 < p2
```

# pair的常见用途

1. 用来代替二元结构体及其构造函数，可以节省编码时间
2. 作为map的键值对来进行插入

```cpp
#include<iostream>
#include<map>
#include<string>
using namespace std;

int main(){
    map<string, int> mp;
    mp.insert(make_pair("gqq", 5));
    mp.insert(pair<string, int>("yzy", 10));
    for(map<string, int>::iterator it = mp.begin(); it != mp.end(); it++){
        cout << it -> first << " " << it -> second << endl;

    }
    return 0;
}
```

```shell
gqq 5
yzy 10
```

