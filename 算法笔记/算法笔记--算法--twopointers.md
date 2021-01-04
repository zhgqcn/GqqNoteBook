---
title: 算法笔记-two pointers
date: 2020-03-16 10:55:00
categories: 算法
tags: 算法
id: Gqq
---

# two pointers

## 什么是two pointers

从一个题目引入：

```shell
给定一个序列{1, 2, 3, 4, 5, 6}和正整数M = 8， 求序列中的两个数 a + b = M
```

本题最直接的解法就是枚举：

```Cpp
for(int i = 0; i < n; i++){
    for(int j = i + 1; j < n; j++){
        if(a[i] + a[j] == M){
            printf("%d %d\n", a[i], a[j]);
        }
    }
}
```
显然，这个解法的时间复杂度为`O(n)`，当n为$10^5$的规模是不可承受的

<!--more-->

**利用序列的递增特性**，我们可以利用以下解法：

- 令下标`i`的初始值为`0`，下标`j`的初始值为`n-1`，即令`i`、`j`分别指向序列的第一个元素和最后一个元素，接下来根据`a[i]+a[j]`与`M`的大小进行判断：
  - 如果满足`a[i]+a[j]==M`，说明找到了其中一组方案。由于序列递增，不等式`a[i+1]+a[j]>M`和`a[i]+a[j-1]<M`均成立，但是`a[i+1]+a[j-1]`与`M`的大小未知，因此剩余的方案只可能在`[i+1, j-1]`区间内产生，令`i=i+1,j=j-1`
  - 如果满足`a[i]+a[j]>M`，由于序列递增，不等式`a[i+1]+a[j]>M`成立，但是`a[i]+a[j-1]`与`M`的大小未知，所以剩余方案为`[i, j-1`]区间内，令`j=j-1`
  - 如果满足`a[i]+a[j]<M`，由于序列递增，不等式`a[i]+a[j-1]<M`成立，但是`a[i+1]+a[j]`与`M`的大小未知，所以剩余方案为`[i+1, j`]区间内，令`i=i+1`

- 反复执行直到`i>=j`成立

```cpp
while(i < j){
    if(a[i] + a[j] == M){
        printf("%d %d", i, j);
        i = i + 1;
        j = j - 1;
    }else if(a[i] + a[j] < M){
        i = i + 1;
    }else{
        j = j - 1;
    }
}
```

这便是`two pointers`思想，充分利用了序列递增的性质，使复杂度降低到`O(n)`



### 序列合并问题

```shell
假设有两个递增序列A和B，要求将他们合并为递增序列C
```

解法：

- 设置下标`i`、`j`，初值均为`0`，表示分别指向`序列A`的第一个元素和`序列B`的第一个元素
  - 若`A[i] < B[j]`，说明`A[i]`是当前`序列A`与`序列B`的剩余元素中最小的那个，因此把`A[i]`加入`序列C`中，并让`i = i + 1`
  - 若`A[i]>B[j]`，说明`B[j]`是当前`序列A`和`序列B`的剩余元素中最小的那个，因此把`B[j]`加入`序列C`中，并让`j = j + 1`
  - 若`A[i]=B[j]`，任意选择一个加入`C`，并让对于下标`加1`
- 直到`i`、`j`中的一个`到达序列末端`为止，然后将另一个序列的所有元素依次加入C中

```cpp
int mergeABToC(int A[], int B[], int C[], int n, int m){
    int i = 0, j = 0, index = 0;
    while(i < n && j < m){
        if(A[i] <= B[j]){
            C[index++] = A[i++];
        }else{
            C[index++] = B[j++];
        }
    }
    while(i < n) C[index++] = A[i++];
    while(j < m) C[index++] = B[j++];
    return index;
}
```

##   归并排序

主要学习`二路归并排序`，其原理为：将序列归并为$[n/2]$个组，组内单独排序；然后将这些组再两两归并排序，生成$[n/4]$个组，组内再单独排序；以此类推，直到剩下一个组为止。归并排序的时间复杂度为`O(nlogn)`

```shell
利用归并排序实现{12, 18, 27, 33, 57, 64, 66}
```

![Snipaste_2020-03-17_20-43-52](https://tva1.sinaimg.cn/large/005tpOh1gy1gcx7mue6qsj30bp07gq3r.jpg)

**用一个动图来理解：**

![归并排序](https://tvax4.sinaimg.cn/large/005tpOh1gy1gcxvysfun1g30mj0e1qcv.gif)

### 递归实现

只需要反复将当前区间`[left, right]`分为两半，对两个子区间`[left, mid]`和`[mid+1, right]`分别递归进行归并排序，然后将两个已经有序的子区间合并为有序序列即可。

```cpp
/*先实现将两个数组合并*/
int maxn = 100;
void merge(int A[], int L1, int R1, int L2, int R2){
    int i = L1, j = L2;
    int temp[maxn], index = 0;
    while(i <= R1 && j <= R2){
        if(A[i] < A[j]){
            temp[index++] = A[i++];
        }else {
            temp[index++] = A[j++];
        }
    }
    while(i <= R1) temp[index++] = A[i++];
    while(j <= R2) temp[index++] = A[j++];
    for(int i = 0; i < index; i++){
        A[L1 + i] = temp[i];			// 将合并后的序列赋值会数组A
    }
}
```

```cpp
/*将array数组当前区间[left, right]进行归并排序*/
void mergeSort(int A[], int left, int right){
    if(left < right){						// 只要left小于right
        int mid = left + (right - left) / 2;// 取left, right 的中点
        mergeSort(A, left, mid);		    // 递归，将左区间归并
        mergeSort(A, mid + 1, right);		// 递归，将右区间归并
        merge(A, left, mid, mid + 1, right);// 将左右区间合并
    }
}

```

### 非递归实现

```cpp
void mergeSort(int A[]){
    for(int step = 2; step / 2 <= n; step *= 2){
        for(int i = 1; i <= n; i += step){
            int mid = i + step / 2 - 1;
            if(mid + 1 <= n){
                merge(A, i, mid, mid + 1, min(i + step - 1, n));
            }
        }
    }
}
```

如果题目中只要求给出归并排序每一趟结束的序列，那么可以使用sort函数代替merge

```cpp
void mergeSort(int A[]){
    for(int step = 2; step / 2 <= n; step *= 2){
        for(int i = 1; i <= n; i += step){
            sort(A + i, A + min(i + step, n + 1));
        }
    }
}
```

## 快速排序

快速排序的平均时间复杂度为`O(logn)`，其要解决是：

```shell
对于序列A为{a, b, c, d, e, f, g, ......}，调整序列中元素的位置，使得a的左侧所有元素不超过a, 右侧所有元素都大于a, 如下图:
```

![Snipaste_2020-03-18_11-12-10](//tvax4.sinaimg.cn/large/005tpOh1gy1gcxwput21pj30ab059q3b.jpg)

解决该题最快的做法就是**two pointers思想**：

- 1️⃣先将a存在一个临时变量temp中，并令两个下标left、 right分别指向序列首尾（如令left = 1, right = n)
- 2️⃣只要right指向的元素A[right]大于temp，就将right继续左移；当某个时候A[right]小于等于temp时，将原素A[right]挪到left指向的元素A[left]处
- 3️⃣只要left指向的元素A[left]小于等于temp， 就将left继续右移；当某个时候A[left]大于temp时，将元素A[left]挪到right指向的元素A[right]处
- 4️⃣重复上面2️⃣3️⃣步骤, 直到left与right相遇，把temp放到相遇的地方

⭕**举例：**

![IMG_1245(20200318-114957)](//tvax3.sinaimg.cn/large/005tpOh1gy1gcxxvfqlp0j30l812i78b.jpg)

**进行一次左右划分：**

```cpp
// 对区间[left, right]进行划分
int partition(int A[], int left, int right){
    int temp = A[left];
    while(left < right){
        while(left < right && A[right] > temp) right--;
        A[left] = A[right];
        while(left < right && A[left] <= temp) left++;
        A[right] = A[left];
    }
    A[left] = temp;
    return left;
}
```

**正式实现快速排序算法**

- 🅰调整序列中的元素，使得当前序列最左端的元素在调整后满足左侧所有元素均不超过该元素，右侧所有元素均大于该元素
- 🅱对该元素的**左侧和右侧**分别**递归**进行🅰的调整，直到当前调整区间的长度不超过1.

```cpp
void quickSort(int A[], int left, int right){
    if(left < right){					// 当前区间的长度不超过1
        int pos = partition(A, left, right); // 将区间一分为二
        quickSort(A, left, pos - 1);	// 对左子区间递归进行快速排序
        quickSort(A, pos + 1, right);	// 对右子区间递归进行快速排序
    }
}
```

当序列中元素接近有序时，会达到最坏时间复杂度$O(n^2)$，产生原因在于主元没有把当前区间划分为两个长度接近的子区间。为了解决这一问题，我们要在序列中**随机**选择一个主元，虽然最坏的情况下时间复杂度仍可能$O(n^2)$，但是对于任意输入数据的期望时间复杂度都能到达$O(nlogn)$

### C语言中随机数的生成

```cpp
#include<stdio.h>
#include<stdlib.h>		// 参数随机数需要这两个头文件
#include<time.h>
int main(){
    srand(unsigned)time(NULL);  	// 参数随机数种子
    for(int i = 0; i < 10; i++){	// 产生10个随机数
        printf("%d", rand(i));      // rand()函数参数随机数
    }
    return 0;
}
```

`rand()`函数只能产生`[0, RAND_MAX]`范围内的整数`(RAND_MAX`是`stdlib.h`中的一个常数，不同系统中不同，如可能为：32767)，所以想要产生`[a,b]`内的随机数，应该使用`rand() % (b - a + 1) + a`。

```shell
rand % (b - a + 1) + 0; // 产生范围[0, b - a]
rand % (b - a + 1) + a; // 产生范围[a, b]
```

```cpp
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
int main(){
    srand(unsigned)time(NULL);
    for(int i = 0; i < 10; i++){
        printf("%d", rand() % 2); // rand() % (1 + 1) + 0 产生[0, 1]之间的随机数
    }
    for(int i = 0; i < 10; i++){
        printf("%d", rand() % 6 + 2); // rand()%(1+5)+2 产生[2, 7]之间的随机数
    }
}
```

如果要产生大于`RAND_MAX`的随机数时，可以如下：

- 先用`rand()`产生一个`[0, RAND_MAX]`范围内的随机数
- 用这个随机数除以`RAND_MAX`，得到`[0, 1]`之间的浮点数
- 将这个浮点数乘以范围长度`(b-a+1)`，再加上`a`即可
- 即，`(int)(double)rand() / RAND_MAX * (b - a + 1) + a)`

🔵**举个例子**：生成[10000, 60000]范围内的随机数

```cpp
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
int main(){
    srand(unsigned)time(NULL);
    for(int i = 0; i < 10; i++){
        printf("%d", (int)(round(1.0 * rand() / RAND_MAX * 50000 + 10000)));
    }
}
```

### 使用随机数的快速排序

```cpp
int randPartition(int A[], int left, int right){
    int p = (int)(round(1.0 * rand() / RAND_MAX + (right - left) + left));
    swap(A[p], A[left]);
    int temp = A[left];
    while(left < right){
        while(left < right && A[right] > temp) right--;
        A[left] = A[right];
        while(left < right && A[left] <= temp) left++;
        A[right] = A[left];
    }
    A[left] = temp;
    return left;
}
```



## 练习

### [基础排序III：归并排序](http://codeup.cn/problem.php?cid=100000586&pid=1)

```CPP
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
void mymerge(int A[], int L1, int R1, int L2, int R2);
void mergeSort(int A[], int left, int right);
const int MAXN = 100010;
int main(){

    int n;
    int A[MAXN];
    while(scanf("%d", &n) != EOF){
        while(n != 0){
            n--;
            int k;
            scanf("%d", &k);

            for(int i = 0; i < k; i++){
                scanf("%d", &A[i]);
            }
            mergeSort(A, 0, k - 1);
            for(int i = 0; i < k; i++){
                printf("%d\n", A[i]);
            }
        }
    }

}

void mymerge(int A[], int L1, int R1, int L2, int R2){
    int temp[MAXN], index = 0;
    int i = L1, j = L2;
    while(i <= R1 && j <= R2){
        if(A[i] < A[j]) temp[index++] = A[i++];
        else temp[index++] = A[j++];
    }
    while(i <= R1) temp[index++] = A[i++];
    while(j <= R2) temp[index++] = A[j++];
    for(int i = 0; i < index; i++){
        A[L1 + i] = temp[i];
    }
}

void mergeSort(int A[], int left, int right){
    if(left < right){
        int mid = left + (right - left) / 2;
        mergeSort(A, left, mid);
        mergeSort(A, mid + 1, right);
        mymerge(A, left, mid, mid + 1, right);
    }
}
```

### [快速排序 qsort](http://codeup.cn/problem.php?cid=100000586&pid=2)

```cpp
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
const int MAXN = 5005;
void quickSort(int A[], int left, int right);
int mypartition(int A[], int left, int right);
int main(){
    int n;
    int A[MAXN];
    while(scanf("%d", &n) != EOF){
        for(int i = 0; i < n; i++){
            scanf("%d", &A[i]);
        }
        quickSort(A, 0, n - 1);
        for(int i = 0; i < n; i++){
            printf("%d\n", A[i]);
        }
    }
}

int mypartition(int A[], int left, int right){
    int temp = A[left];
    while(left < right){
        while(left < right && A[right] > temp) right--;
        A[left] = A[right];
        while(left < right && A[left] <= temp) left++;
        A[right] = A[left];
    }
    A[left] = temp;
    return left;
}

void quickSort(int A[], int left, int right){
    if(left < right){
        int mid = mypartition(A, left, right);
        quickSort(A, left, mid - 1);
        quickSort(A, mid + 1, right);
    }
}
```

