---
title: 算法笔记--标准模板库STL--queue
date: 2020-03-28 16:00:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# queue的常见用法

> queue是队列，在STL主要实现了一个`先进先出`的容器

**头文件**

```cpp
#include<queue>
using namespace std;
```

<!--more-->

# queue的定义

```cpp
queue<typename> name;
```

# queue 容器内的元素访问

> 由于队列`queue`本身就是一种`先进先出`的限制性数据结构，因此在STL中只能通过`front()`来访问**队首元素**，或是通过`back()`来访问**队尾元素**

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    queue<int> q;
    for(int i = 1; i <= 5; i++){
        q.push(i); // 将i 压入队列
    }
    printf("%d %d\n", q.front(), q.back());	// 结果: 1 5
    return 0;
}
```

# queue常用函数

## push( )

> `push(x)`将x进行入队，时间复杂度为`O(1)`

## front( ) | back( )

> 分别用以获得队首和队尾元素，时间复杂度为`O(1)`

## pop( )

> 令队首元素出队，时间复杂度为`O(1)`

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    queue<int> q;
    for(int i = 1; i <= 5; i++){
        q.push(i); // 将i 压入队列
    }
    for(int i = 0; i < 3; i++){
        q.pop();		// 三次出队，分别是1 2 3 
    }
    printf("%d %d\n", q.front(), q.back()); // 4 5
    return 0;
}

```

## empty( )

> 检测queue是否为空，返回true为空，false为非空，时间复杂度为`O(1)`

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    queue<int> q;
    q.empty() ? printf("Empty\n") : printf("No Empty");
    for(int i = 1; i <= 5; i++){
        q.push(i); // 将i 压入队列
    }
    for(int i = 0; i < 3; i++){
        q.pop();
    }
    printf("%d %d\n", q.front(), q.back()); // 4 5
    q.empty() ? printf("Empty\n") : printf("No Empty");
    return 0;
}
```

## size( )

> 返回queue内元素个数，时间复杂度为`O(1)`

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    queue<int> q;
    for(int i = 1; i <= 5; i++){
        q.push(i); // 将i 压入队列
    } 
    printf("%d", q.size()); // 结果: 5
    return 0;
}

```

# queue的常见用途

- 当需要实现广度优先搜索时，可以不用自己手动实现一个队列，而是用`queue`作为代替，
  以提高程序的准确性。
- 另外有一点注意的是，使用`front()`和`pop()`函数前，必须`empty()`判断队列是否为空，
  否则可能因为队空而出现错误。