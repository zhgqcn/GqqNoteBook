---
title: 算法笔记--标准模板库STL--algorithm头文件
date: 2020-03-29 9:30:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq

---

> 使用algorithm头文件，需要在头文件下加一行`using namespace std;`才可以正常使用

<!--more-->

# max( )、min( )、abs( )

`max(x, y)`和`min(x, y）`分别返回`x`和`y`中的最大值和最小值，且**参数必须是两个（可以是浮点数）**。如果想要返回三个数`x、Y、z`的最大值，可以使用`max(x, max(y, z))`的写法。`abs(x)`返回`x`的绝对值。**x必须是整数**，**浮点型**的绝对值请用`math`头文件下的`fabs`

```cpp
#include<stdio.h>
#include<algorithm>
using namespace std;

int main(){
    int x = 1, y = -2;
    printf("%d %d\n", max(x, y), min(x, y));
    printf("%d %d\n", abs(x), abs(y));

    return 0;
}
```

```shell
1 -2
1 2
```

# swap( )

**用来交换x和y的值**

```cpp
#include<stdio.h>
#include<algorithm>
using namespace std;

int main(){
    int x = 1, y = -2;
    swap(x, y);
    printf("%d %d\n", x, y);
    return 0;
}
```

```shell
-2 1
```

# reverse( )

`reverse(it1, it2)`可以将**数组指针**在`[it1, it2)`之间的元素或容器的迭代器在`[it1, it2)`范围内的元素进行**反转**

**数组举例**

```cpp
#include<stdio.h>
#include<algorithm>
using namespace std;

int main(){
    int a[10] = {10, 11, 12, 13, 14, 15};
    reverse(a, a + 4);      // 将a[0]到a[3]反转
    for(int i = 0; i < 6; i++){
        printf("%d ", a[i]);
    }
    return 0;
}
```

```shell
13 12 11 10 14 15
```

**字符串举例**

```cpp
#include<stdio.h>
#include<algorithm>
#include<string>
using namespace std;

int main(){
    string str = "1234567890";
    reverse(str.begin() + 2, str.begin() + 6); // 对str[2]到str[5]进行反转
    for(int i = 0; i < str.length(); i++){
        printf("%c", str[i]);
    }
    return 0;
}
```


```shell
1265437890
```

# next_permutation( )

`next_permutation()`给出一个序列在全排列中的**下一个序列**

如，当`n==3`时，全排列有

```cpp
123;
132;
213;
231;
312;
321;
```

所以，231的下一个序列就是312

**实例**

```cpp
#include<stdio.h>
#include<algorithm>
#include<string>
using namespace std;

int main(){
    int a[10] = {1, 2, 3};
    //a[0]到a[2]之间的序列需要求解next_permutation
    do{
        printf("%d%d%d\n", a[0], a[1], a[2]);
    }while(next_permutation(a, a + 3));
    return 0;
}
```

**结果同上**

# fill( )

`fill()`可以把数组或者容器中的某一段区间赋为某一个相同的值。和`memset`不同，这里的赋值**可以是数值类型对于范围内的任意值**

```cpp
#include<stdio.h>
#include<algorithm>
#include<string>
using namespace std;

int main(){
    int a[10] = {1, 2, 3, 4, 5};
    fill(a, a + 5, 233); // 将a[0]到a[4]赋值为233
    for(int i = 0; i < 5; i++)
        printf("%d " , a[i]);

    return 0;
}
```

# sort( )

`sort`用来实现**排序**

**头文件**

```cpp
#include<algorithm>
using namespace std;
```

## **sort简单使用**

```cpp
sort(首元素地址(必填), 尾元素地址的下一个地址(必填), 比较函数(非必填))
```

**简单举例**

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

int main(){
    int a[6] = {10, 5, 3, 6, 7, -2};
    sort(a, a + 4);    // 将a[0]到a[3]从小到大排序
    for(int i = 0; i < 6; i++){
        cout << a[i] << " ";
    }
    cout << endl;
    sort(a, a + 6);     // 将a[0]到a[5]从小到大排序
    for(int i = 0; i < 6; i++){
        printf("%d ", a[i]);
    }
    return 0;
}
```

```shell
3 5 6 10 7 -2
-2 3 5 6 7 10
```

## 实现比较函数cmp

**若比较函数不填，则默认按照从小到大的顺序排序**，如上面的例子

1. 🅰**基础数据类型数组的排序**

如果想要实现**从大到小**，则可以使用**比较函数cmp**来进行`sort`排序

```cpp
#include<iostream>
#include<algorithm>
using namespace std;
bool cmp(int a, int b){
    return a > b;   // 从大到小
}

int main(){
    int a[6] = {10, 5, 3, 6, 7, -2};
    sort(a, a + 4, cmp);    // 将a[0]到a[3]从小到大排序
    for(int i = 0; i < 6; i++){
        cout << a[i] << " ";
    }
    return 0;
}
```

```shell
10 6 5 3 7 -2
```

2. 🅱**结构体数组的排序**

如果想将结构体按照某个元素从大到小排序，可以如下实现：

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

struct node{
    int x, y;
}ssd[10];

bool cmp(node a, node b){
    return a.x > b.x;   // 按照x的值从大到小对结构体数组排序
}

int main(){
    ssd[0].x = 2;
    ssd[0].y = 2;
    ssd[1].x = 1;
    ssd[1].y = 3;
    ssd[2].x = 3;
    ssd[2].y = 1;
    sort(ssd, ssd + 3, cmp);
    for(int i = 0; i < 3; i++){
        printf("%d %d\n", ssd[i].x, ssd[i].y);
    }
    return 0;
}

```

```shell
3 1
2 2
1 3
```

如果想先按照x从大到小排序，但是**x相当**的情况下，按照y的大小从小到大排序

```cpp
bool cmp(node a, node b){
    if(a.x != b.x) return a.x > b.x;
    else return a.y < b.y;
}
```

3. 🅾**容器的排序**

在STL标准容器中，**只有vector，string，queue可以使用sort**，因为想**set，map**这种容器是用红黑树实现的，本身就是有序的，所以不允许使用sort排序

**以vector为例**

```cpp
#include<stdio.h>
#include<vector>
#include<algorithm>
using namespace std;

bool cmp(int a, int b){
    return a > b;
}

int main(){
    vector<int> vi;
    vi.push_back(3);
    vi.push_back(1);
    vi.push_back(2);
    sort(vi.begin(), vi.end(), cmp); // 对整个vector排序
    for(int i = 0; i < 3; i++){
        printf("%d ", vi[i]);
    }
    return 0;
}
```

```shell
3 2 1
```

# lower_bound() 和 upper_bound()

**这两个函数需要用在一个有序数组或者容器中**

`lower_bound(first, last, val)`用来寻找在数组或者容器的`[first, last)`范围内**第一个大于等于val**的元素的位置，如果是数组，则返回该位置的指针；如果是容器，则返回该位置的迭代器。

`upper_bound(first, last, val)`用来寻找在数组或者容器的`[first, last)`范围内**第一个大于val**的元素的位置，如果是数组，则返回该位置的指针；如果是容器，则返回该位置的迭代器。

**若数组或者容器中没有需要找到的元素**，则这两个函数均返回可以 插入该元素的位置的指针或者迭代器。这两个函数的复杂度均为`O(log(last - first))`

```cpp
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(){
    int a[10] = {1, 2, 2, 3, 3, 3, 5, 5, 5, 5};
    // 寻找-1
    int* lowerPos = lower_bound(a, a + 10, -1);
    int* upperPos = upper_bound(a, a + 10, -1);
    printf("%d, %d\n", lowerPos - a, upperPos - a);
    // 寻找2
    lowerPos = lower_bound(a, a + 10, 2);
    upperPos = upper_bound(a, a + 10, 2);
    printf("%d, %d\n", lowerPos - a, upperPos - a);
    // 寻找4
    lowerPos = lower_bound(a, a + 10, 4);
    upperPos = upper_bound(a, a + 10, 4);
    printf("%d, %d\n", lowerPos - a, upperPos - a);
    return 0;
}

```

```cpp
0, 0
1, 3
6, 6
```

**如果只是想要获得欲查元素的下标，就可以不使用临时指针，而直接令返回值减去数组首地址即可**

```cpp
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(){
    int a[10] = {1, 2, 2, 3, 3, 3, 5, 5, 5, 5};
    // 寻找3
    printf("%d, %d", lower_bound(a, a + 10, 3) - a, upper_bound(a, a + 10, 3) - a);
    return 0;
}
```

```cpp
3 6
```



