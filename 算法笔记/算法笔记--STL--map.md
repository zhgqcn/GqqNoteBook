---
title: 算法笔记--标准模板库STL--map
date: 2020-03-28 13:40:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---



# map的常用用法详解

**map可以将任何基本类型(包括STL容器)映射到任何基本类型(包括STL容器)**

**头文件**

```cpp
#include<map>
using namespace std;
```

<!--more-->

# map的定义

**单个map**

```cpp
map<typename, typename> mp;		// <key, value> 的类型

map<string, int> mp;	// 如果是字符串到整型的映射，必须使用string而不能使用char数组
```

**map的键和值是STL容器**

```cpp
map<set<int>, string> mp;
```

# map容器内元素的访问

## 通过下标

> 类似普通的数组访问，但是map中的键是唯一的

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['c'] = 20;
    mp['c'] = 30;            // 20被覆盖
    printf("%d\n", mp['c']); // 输出30
    return 0;
}
```

## 通过迭代器访问

**迭代器定义**

```cpp
map<typename1, typename2>::iterator it;
```

**因为map的每一对映射都有两个`typename`，这决定了必须通过一个it来同时访问键和值，map通过使用`it-> first`来访问键，通过`it->second`来访问值**

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    map<char, int>::iterator it = mp.begin();
    for(it; it != mp.end(); it++)
        printf("%c %d\n", it->first, it->second);
    return 0;
}
```

```shell
a 40
d 30
e 20
```

**🛑map会以键从小到大的顺序自动排序**

# map常用的函数

## find( )

> find( )返回键为key的映射的迭代器，时间复杂度为`O(logN)`，N为map中映射的个数

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    map<char, int>::iterator it = mp.find('a');
    printf("%c %d\n", it->first, it->second);
    return 0;
}
```

```shell
a 40
```

## erase( )

### 删除单个元素

1. `mp.erase(it)`，it为需要删除的元素的迭代器，时间复杂度为`O(1)`

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    map<char, int>::iterator it = mp.find('d');
    mp.erase(it);
    for(map<char, int>::iterator it = mp.begin(); it != mp.end(); it++)
        printf("%c %d\n", it->first, it->second);
    return 0;
}
```

```shell
a 40
e 20
```



2. `mp.erase(key)`，key为欲删除的映射的键，时间复杂度为`O(logN)`

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    mp.erase('e');
    for(map<char, int>::iterator it = mp.begin(); it != mp.end(); it++)
        printf("%c %d\n", it->first, it->second);
    return 0;
}
```

```shell
a 40
d 30
```

### 删除区间内的元素

> `mp.erase(first, second)`，其中first为需要删除区间的起始迭代器，last为需要删除区间的末尾迭代器的下一个地址，为`[first, last)`，时间复杂度为`O(last - first)`

```cpp
#include<stdio.h>
#include<map>
using namespace std;
int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    map<char, int>::iterator it = mp.find('d');
    mp.erase(it, mp.end());
    for(map<char, int>::iterator it = mp.begin(); it != mp.end(); it++)
        printf("%c %d\n", it->first, it->second);
    return 0;
}
```

```shel
a 40
```

## size( )

> `size()`用来获得map中映射的对数，时间复杂度为`O(1)`

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    printf("%d", mp.size());    // 结果为3
    return 0;
}
```

## clear( )

> `clear()`用来清空map的所有元素，复杂度为`O(N)`

```cpp
#include<stdio.h>
#include<map>
using namespace std;

int main(){
    map<char, int> mp;
    mp['e'] = 20;
    mp['d'] = 30;
    mp['a'] = 40;
    mp.clear();
    printf("%d", mp.size());    // 结果为0
    return 0;
}
```

# map的常见用途

- 需要建立字符（或字符串）与整数之间映射的题目，使用map可以减少代码量。
- 判断大整数或者其他类型数据是否存在的题目，可以把map当bool数组用。
- 字符串和字符串的映射也有可能会遇到。