---
title: 算法笔记--标准模板库STL--vector
date: 2020-03-26 16:00:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# Vector的常见用法详解

> Vector 翻译为向量，是**长度可根据需要而自动改变的数组**

## **头文件**

```cpp
#include<vector>
using namespace std;
```

<!--more-->

## **vector定义**

**单独一个vector**：相当于一维数组`name[SIZE]`

```cpp
vector<typename> name;

vector<int> name;			// 基本类型
vector<double> name;
vector<char> name;
vector<node> name;			// 结构体类型
vector<vector<int>> name; 	// STL容器， 这种的vector数组是两个维度都可变的
```

**vector数组**：一维长度已经固定为`arraySize`，另一个维度是可变长度的

```cpp
vector<typename> ArrayName[arraySize];

vector<int> vi[100]; 
```

这样`ArrayName[0]` ~ `ArrayName[arraySize - 1]`中每一个都是一个vector容器

## Vector容器内元素的访问

**通过下标访问**

​		和访问普通的数组是一样，对一个定义为`vector<typename> vi`的vector容器来说，直接
访问`vi[index]`即可(如可`vi[0]`)。当然，这里下标是从`0到vi.size()-1`，访问这个范围外的元素可能会运行出错。

**通过迭代器访问**

`迭代器iterator`相当于类似**指针**的东西，其定义为：

```cpp
vector<typename>::iterator it; // it就是一个vector<typename>::iterator型的变量
							   // typename就是定义vector时填写的类型
vector<int>::iterator it;
vector<double>::iterator it;
```

有了迭代器`it`，就可以通过`*it`来访问vector里面的元素

```cpp
vector<int> vi;
for(int i = 1; i <= 5; i++){			   		 // 循环完毕后vi中元素为1 2 3 4 5 
    vi.push_back(i);		// push_back(i)在vi的末尾添加元素i,即依次添加1 2 3 4 5 
}
```

通过类似下标和指针数组的方式来访问容器内的元素：

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }
    // vi.begin()为取vi的首元素地址，而it指向这个地址
    vector<int>::iterator it = vi.begin();
    for(int i = 0; i < 5; i++)
        printf("%d ", *(it + i)); // 输出vi[i]
    return 0;
}
```

**`vi[i]`和`*(vi.begin() + i)`是等价的**

1️⃣`vi.begin()`是取`vi`的首元素地址

2️⃣`vi.end()`是取`vi`的尾元素地址的下一个地址

3️⃣迭代器还提供了自加操作和自减操作，如：`++it`和`it++`

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }

    // vector的迭代器不支持it < vi.end()写法，因此循环条件只能用it != vi.end()
    for(vector<int>::iterator it = vi.begin(); it != vi.end(); it++){
        printf("%d ", *it);
    }
    return 0;
}
```

在常用的STL容器中，只有在vector和string中，才允许使用`vi.begin()+3`这种迭代器加上整数的写法

## vector常用函数

1. **push_back( )**

> push_back(x) 就是在vector后面添加一个元素x，时间复杂度为O(1)

2. **pop_back( )**

> pop_back( ) 用以删除vector的尾元素，时间复杂度为O(1)

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }

    for(int i = 0; i < vi.size(); i++){
        printf("%d ", vi[i]);
    }

    printf("\n");
    vi.pop_back();

    for(int i = 0; i < vi.size(); i++){
        printf("%d ", vi[i]);
    }
    return 0;
}
```

```shell
1 2 3 4 5
1 2 3 4
```

3. **size( )**

> size( ) 用来获取vector中元素的个数，时间复杂度为O(1)
>
> size( ) 返回的是 unsigned 类型，一般来说 %d 不会出现很多问题

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }

    printf("%d ", vi.size());

    return 0;
}
```

4. **clear( )**

> clear( ) 用来清空vector中的所有元素，时间复杂度为O(N)

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }
    printf("%d ", vi.size());
    vi.clear(); // 清空操作
    printf("%d ", vi.size());
    return 0;
}
```

5. **insert( )**

> insert(it, x) 用来向vector的任意迭代器it处插入一个元素x，时间复杂度为O(N)

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }
    
    vi.insert(vi.begin() + 3, -1); // 插入操作

    for(int i = 0; i < vi.size(); i++)
        printf("%d ", *(vi.begin() + i));
    return 0;
}
```

6. **erase( )**

- 🅰删除单个元素，时间复杂度为O(N)
- 🅱删除一个区间内的所有元素，时间复杂度为O(N)

**删除单个元素**

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }

    vi.erase(vi.begin() + 3); // 删除单个元素

    for(int i = 0; i < vi.size(); i++)
        printf("%d ", *(vi.begin() + i));
    return 0;
}
```

**删除一个区间内的所有元素**

```cpp
#include<cstdio>
#include<vector>
using namespace std;

int main(){
    vector<int> vi;
    for(int i = 1; i <= 5; i++){
        vi.push_back(i);
    }

    vi.erase(vi.begin() + 2, vi.begin() + 4); // 删除区间元素

    for(int i = 0; i < vi.size(); i++)
        printf("%d ", *(vi.begin() + i));
    return 0;
}

```

## vector的常见用途

- 存储数据

- 用作邻接表存储图

  

