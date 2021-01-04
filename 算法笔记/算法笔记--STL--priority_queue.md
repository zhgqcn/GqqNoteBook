---
title: 算法笔记--标准模板库STL--priority_queue
date: 2020-03-28 18:20:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# priority_queue 的常见用法

> priority_queue又称为优先队列，其底层使用堆实现

在优先队列中，**队首元素**一定要是当前队列中优先级最高的那个

在任何时候往优先队列里面加入(push)元素，而优先队列底层的数据结构堆(heap)会随时调整结构，使得每次的队首元素都是优先级最大的

**头文件**

```cpp
#include<queue>
using namespace std;
```

<!--more-->

# priority_queue的定义

```cpp
priority_queue<typename> name;
```

# priority_queue容器内元素的访问

不同于普通队列，优先队列没有`front()`和`back()`函数，而是能通过`top()`来访问队首元素（也可以称为堆顶元素），也就是优先级最高的元素

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    priority_queue<int> q;
    q.push(3);
    q.push(4);
    q.push(1);
    printf("%d", q.top());	// 结果为: 4
    return 0;
}
```

# priority_queue常用函数

## push( )

> push( ) 将令x入队，时间复杂度为O(logN)，其中N为当前优先队列的元素个数

## top( )

> top( ) 可以（访问）获得队首元素，时间复杂度为O(1)

## pop( )

> pop( ) 令队首元素出队，时间复杂度为`O(logN)`

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    priority_queue<int> q;
    q.push(3);
    q.push(4);
    q.push(1);
    printf("%d\n", q.top());	// 4
    q.pop();
    printf("%d\n", q.top());	// 3
    return 0;
}
```

## empty( )

> empty( ) 检测队列是否为空，空返回true，非空返回false，时间复杂度为O(1)

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    priority_queue<int> q;
    q.push(3);
    q.push(4);
    q.push(1);

    q.empty() ? printf("Empty\n") : printf("NO Empty\n");

    for(int i = 1; i <= 3; i++){
        q.pop();
    }

    q.empty() ? printf("Empty\n") : printf("NO Empty\n");

    return 0;
}
```

```shell
NO Empty
Empty
```

## size( )

> size( ) 返回优先队列内元素的个数，时间复杂度为O(1)

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    priority_queue<int> q;
    q.push(3);
    q.push(4);
    q.push(1);
    printf("%d", q.size());	// 3
    return 0;
}
```

# priority_queue内元素优先级的设置

## 基本数据类型的优先级设置

对于基本数据类型，一般数字大的优先级高，因此队首元素就是优先队列内元素最大的那个（如果char型，则是字典序最大的）

对于基本数据类型来说，下面两种优先队列的定义是**等价**的:

```cpp
priority_queue<int> q;
priority_queue<int, vector<int>, less<int>> q;
// vector<int> 是用来承载底层数据结构堆(heap)的容器
// less<int> 表示数字大的优先级大，great<int> 表示数字小的优先级大
```

如，想让优先队列总是把最小的元素放在队首，

```cpp
priority_queue<int, vector<int>, greater<int>> q;
```

```cpp
#include<stdio.h>
#include<queue>
using namespace std;

int main(){
    priority_queue<int, vector<int>, greater<int>> q;
    q.push(3);
    q.push(4);
    q.push(1);
    printf("%d", q.top()); // 1
    return 0;
}
```

## 结构体的优先级设置

如水果的结构体如下：

```cpp
struct fruit{
    string name;	// 水果名称
    int price;		// 水果价格
}
```

现在希望按照水果的价格高的为优先级高，就需要**重载overload**小于号`<`

`重载`：对已有运算符进行重新定义，也就是说，可以改变小于号的功能

```cpp
struct fruit{
    string name;
    int price;
    friend bool operator < (fruit f1, fruit 2){
        return f1.price < f2.frice;
    }
}
```

此时就可以直接定义fruit类型的优先队列，其内部就是以价格高的水果为优先级高，

```cpp
priority_queue<fruit> q;
```

如果要以价格低的水果优先级高，那么只需要把return中的小于号改为大于号

```cpp
#include<iostream>
#include<queue>
#include<string>
using namespace std;

struct fruit{
    string name;
    int price;
    friend bool operator < (fruit f1, fruit f2){
        return f1.price > f2.price;
    }
}f1, f2, f3;

int main(){
    priority_queue<fruit> q;
    f1.name = "桃子";
    f2.name = "栗子";
    f3.name = "苹果";
    f1.price = 4;
    f2.price = 2;
    f3.price = 7;
    q.push(f1);
    q.push(f2);
    q.push(f3);
    cout << q.top().name << "  " << q.top().price << endl;
    return 0;
}
```

```shell
栗子 2
```

**优先队列中对小于号重载的函数和`sort()`中的`cmp`函数是相反的**

当然，重载函数也可以写在结构体外：

```cpp
#include<iostream>
#include<queue>
#include<string>
using namespace std;

struct fruit{
    string name;
    int price;
}f1, f2, f3;

struct cmp{
   bool operator () (fruit f1, fruit f2){
        return f1.price > f2.price;
   }
};

int main(){
    priority_queue<fruit, vector<fruit>, cmp> q;
    f1.name = "桃子";
    f2.name = "栗子";
    f3.name = "苹果";
    f1.price = 4;
    f2.price = 2;
    f3.price = 7;
    q.push(f1);
    q.push(f2);
    q.push(f3);
    cout << q.top().name << "  " << q.top().price << endl;
    return 0;
}
```

**所以，即便是基本数据类型或者其他STL容器，也可以通过同样的方式定义优先级**

如果结构体内的数据量较为庞大（如出现了字符串或者数组），建议使用**引用**来提高效率，此时比较类的参数中需要加上`const`和`&`

```cpp
friend bool operator < (const fruit &f1, const fruit &f2){
    return f1.price > f2.price;
}

bool operator () (const fruit &f1, const fruit &f2){
    return f1.price > f2.price;
}
```

# priority_queue的常见用途

- 贪心问题
- Dijkstra算法

**注意：使用`top()`之前必须先用`empty()`判断队列是否为空**