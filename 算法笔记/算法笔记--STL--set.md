---
title: 算法笔记--标准模板库STL--set
date: 2020-03-26 18:00:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# set的常见用法详解

> set翻译为集合，是一个**内部自动有序**且**不含重复元素**的容器

**头文件**

```cpp
#include<set>
using namespace std;
```

<!--more-->

## set的定义

**单个set**

```
set<typename> name;

set<int> name;
set<double> name;
set<char> name;
set<node> name;	// node是结构体的类型
```

**set数组**

```cpp
set<typename> ArrayName[arraySize];
set<int> a[1000];
```

## set容器内元素访问

***只能通过迭代器访问***

```cpp
set<typename>::iterator it;
```

**除了vector和string之外的STL容器都不能使用*(it + i) 的访问方式**，所以只能枚举：

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    st.insert(3);
    st.insert(5);
    st.insert(2);
    st.insert(3);

    // 注意：不支持it < st.end()写法
    for(set<int>::iterator it = st.begin(); it != st.end(); it++){
        printf("%d ", *it);
    }

    return 0;
}
```

```shell
2 3 5
```

通过结果可以发现，set内的元素自动递增排序，且自动去除重复元素

# set常用函数

## insert( )

> `insert(x)` 可将x插入set容器中，并且自动递增排序和去重，时间复杂度为`O(logN) `， N为set内元素的个数

## find( )

> `find(value) `返回set中对应值为value的迭代器，时间复杂度为`O(logN) `， N为set内元素的个数

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    for(int i = 1; i <=10; i += 2){
        st.insert(i);
    }

    for(set<int>::iterator it = st.begin(); it != st.end(); it++){
        printf("%d ", *it);
    }

    set<int>::iterator it = st.find(7);

    printf("\n%d\n", *it);

    return 0;
}
```

```shell
1 3 5 7 9
7
```

## erase( )

### 删除单个元素

1. `st.erase(it)`，it为所需要删除元素的迭代器，时间复杂度为`O(1)`，可以结合`find()`使用

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300); 
    st.erase(st.find(100)); // 利用find找到100，然后用erase删除
    st.erase(st.find(200));
    for(set<int>::iterator it = st.begin(); it != st.end(); it++)
        printf("%d", *it);
    return 0;
}
```

2. `st.erase(value)`，`value`为所需要删除元素的值，时间复杂度为`O(logN)`， `N`为`set`内元素个数

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300);
    st.erase(100); // 删除set中值为100的元素

    for(set<int>::iterator it = st.begin(); it != st.end(); it++)
        printf("%d ", *it);
    return 0;
}
```

### 删除一个区间内的元素

`st.erase(first, last)`可以删除一个区间内的所有元素，删除区间是`[first, last)`，时间复杂度为`O(last - first)`

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300);

    set<int>::iterator it = st.find(200);
    st.erase(it, st.end()); // 删除元素200至set末尾之间的元素，即200,300

    for(set<int>::iterator it = st.begin(); it != st.end(); it++)
        printf("%d ", *it);
    return 0;
}
```

## size( )

`size()`用来取得set内元素的个数，时间复杂度为`O(1)`

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300);

    printf("%d", st.size());
    return 0;
}
```

## clear( )

`clear()`用来清空set中的所有元素，复杂度为`O(N)`

```cpp
#include<cstdio>
#include<set>
using namespace std;

int main(){
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300);
    st.clear();
    printf("%d", st.size());
    return 0;
}
```

# set的常见用途

- 自动去重并按升序排序