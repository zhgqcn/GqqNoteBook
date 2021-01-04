---
title: 算法笔记--标准模板库STL--stack
date: 2020-03-28 06:00:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# stack的常见用法

> stack翻译为栈，是STL中实现的一个**后进先出**的容器

**头文件**

```cpp
#include<stack>
using namespace std;
```

<!--more-->

# stack的定义

```cpp
stack<typename> name;
```

# stack容器内元素的访问

**只能通过`top()`来访问栈顶元素**

```cpp
#include<iostream>
#include<stack>
using namespace std;

int main(){
    stack<int> st;
    for(int i = 1; i <= 5; i++)
        st.push(i);

    printf("%d", st.top());	// 5

    return 0;
}

```

# stack常用函数

## push( )

> push(x) 将x入栈，时间复杂度为O(1)

## top( )

> top( ) 访问栈顶元素，时间复杂度为O(1)

## pop( )

> pop( ) 用以弹出栈顶元素，时间复杂度为O(1)

```cpp
#include<iostream>
#include<stack>
using namespace std;

int main(){
    stack<int> st;
    for(int i = 1; i <= 5; i++)
        st.push(i);

    for(int i = 1; i <= 3; i++)
        st.pop();               // 出栈3次

    printf("%d", st.top());     // 2

    return 0;
}
```

## empty( )

> empty( ) 可以检测stack内是否为空，空返回true，非空返回false，时间复杂度为O(1)

## size( )

> size( ) 返回stack内元素个数，时间复杂度为O(1)

```cpp
#include<iostream>
#include<stack>
using namespace std;

int main(){
    stack<int> st;
    for(int i = 1; i <= 5; i++)
        st.push(i);

    for(int i = 1; i <= 3; i++)
        st.pop();               // 出栈3次

    st.empty() ? printf("Empty\n") : printf("No Empty\n");

    printf("%d\n", st.size());
    printf("%d", st.top());

    return 0;
}
```

```shell
No Empty
2
2
```

# stack的常见用途

- 用来模拟实现一些递归，防止程序对栈内存的限制而导致程序运行错误