---
title: 算法笔记--搜索专题
date: 2020-04-05 16:20:00
categories: 算法与数据结构
tags: 算法
id: Gqq
---

# 深度优先搜索（DFS）

## **迷宫问题**

对于一个迷宫，我们要找到出口，我们可以通过当遇到**岔道口**时，总选择其中一条路前进（沿着右手），当岔路上如果又遇到新的岔道口，仍然选择新的岔道口的其中一条岔路前进，直到碰到**死胡同**才退回到最近的岔道口选择另一条路，也就是说，当碰到岔道口时，总是以**深度**作为前进的关键词，不碰到死胡同就不回头，这中搜索的方式就称为**深度优先搜索**`Depth First Search`

<!--more-->

<img src="https://tvax3.sinaimg.cn/large/005tpOh1gy1gdiqeuv41bj30j60e5ta1.jpg" alt="Snipaste_2020-04-05_11-30-29" style="zoom: 67%;" />

**抽象下迷宫，我们可以看到**类似树的结构

![Snipaste_2020-04-05_11-40-31](https://tvax1.sinaimg.cn/large/005tpOh1gy1gdiqow0magj30lt09yq3f.jpg)

在此迷宫中，整个DFS过程中先后访问的结点顺序为**ABDHIJEKLMCFG**，有点类似栈的进出，DFS可以用栈的来实现，但比较复杂，本次学习以**递归方式**实现DFS

所以，**深度优先搜索是一种枚举所有完整路径以及遍历所有情况的搜索方法**

## 通过实例学习DFS

**使用递归可以很好地实现深度优先搜索**，但DFS的实现不仅仅只是递归，但一般情况下，非递归的实现比较麻烦。在递归中，系统会调用一个叫做**系统栈**的东西来存放递归中每一层的状态，因此使用递归实现DFS的本质还是**栈**

### 背包问题

**题目**

```cpp
有n件物品，每一件物品的重量为w[i]，价值为c[i].现在需要选出若干件物品放入一个容量为V的背包中，使得在选入背包的物品重量和不超过容量V的前提下，让背包中物品的价值之和最大，求最大价值(1≤n≤20)
```

**思路**

- 岔道口：对每件物品的选与不选
- 死胡同：选择的物品的总量超过V

**代码**

```cpp
#include<cstdio>
const int maxn = 30;
int maxValue = 0;                   // 最大价值
int n, V;               // 物品件数n，背包容量V
int w[maxn], c[maxn];   // 记录每件的重量和价值
/* index为当前处理的物品
sumW和sumC分别为当前总重量和当前总价值*/
void DFS(int index, int sumW, int sumC){
    if(index == n){
        if(sumW <= V && sumC > maxValue){
            maxValue = sumC;
        }
        return;
    }
    // 岔道口，选择或者不选择
    DFS(index + 1, sumW, sumC);						  // 不选第index件物品
    DFS(index + 1, sumW + w[index], sumC + c[index]); // 选择第index件物品
}


int main(){
    scanf("%d%d", &n, &V);
    for(int i = 0; i < n; i++){
        scanf("%d", &w[i]);
    }
    for(int i = 0; i < n; i++){
        scanf("%d", &c[i]);
    }
    DFS(0, 0, 0);
    printf("%d", maxValue);
    return 0;
}
```

因为每件物品都有两种选择，所以，上面代码的复杂度为$O(2^n)$，我们可以优化代码为：

```cpp
void DFS(int index, int sumW, int sumC){
    if(index == n){
        return;		// 已经完成对n件物品的选择
    }
    DFS(index + 1, sumW, sumC);		// 不选第index件物品
    // 只有加入第index件物品后未超过容量V,才能继续
    if(sumW + w[index] <= V){
        if(sumC + c[index] > ans){
            ans = sumC + c[index];	// 更新最大价值maxValue
        }
        DFS(index + 1, sumW + w[index], sumC + c[index]); // 选择第index件物品
    }
}
```

这种通过题目条件的限制来节省DFS计算量的方法称为**剪枝**

### 枚举从N个整数中选择K个数的所有方案

**题目**

```cpp
给定N个整数（可能有负数），从中选择K个数，使得这K个数之和恰好等于一个给定的整数X; 如果有多种方案，选择它们中元素平方和最大的一个。数据保证这样的方案唯一。如，从4个整数{2,3,3,4}中选择2个数，使它们的和为6，显然有两种方案{2,4}和{3,3}，前一中方案的平方和最大。
```

**代码**

```cpp
#include<cstdio>
#include<vector>
using namespace std;
const int maxn = 10;
int A[maxn];               // 用于存放数组
int n, k, x, maxSquare = -1;
vector<int> temp, ans;	   // temp为临时方案，ans为平方和最大时的方案
// index为当前处理的位置，nowK是现在K的个数，sum为和，sumSquare为平方和
void DFS(int index, int nowK, int sum, int sumSquare){
    if(nowK == k && sum == x){
        if(sumSquare > maxSquare){
            maxSquare = sumSquare;
            ans = temp;
        }
        return;
    }
    // 已经处理完n个数，或者超过k个数，或者和超过x，返回
    if(index == n || nowK > k || sum > x) return;
	// 选index号
    temp.push_back(A[index]);
    DFS(index + 1, nowK + 1, sum + A[index], sumSquare + A[index] * A[index]);
    temp.pop_back();
    // 不选index号
    DFS(index + 1, nowK, sum, sumSquare);
}

int main(){
    scanf("%d%d%d", &n, &k, &x);
    for(int i = 0; i < n; i++)
        scanf("%d", &A[i]);
    DFS(0, 0, 0, 0);
    printf("%d", maxSquare);
    return 0;
}
```

**假如N个整数中的每一个都可以被选择多次，那么选择K个数，使得K个数之和恰好为X**，这个问题只需要对上面的代码进行少量的修改即可。由于每个整数都可以**被选择多次**，因此当选择了`index号数`时，不应当直接进入`index+1号数`的处理。显然，应当能够继续选择`index号数`，直到某个时刻决定不再选择`index号数`，就会通过`不选index号数`这条分支进入`index + 1`号数的处理。因此只需要把`选index号数`这条分支的代码修改为`DFS(index, nowK + 1, sum + A[index], sumSquare + A[index] * A[index])`即可



# 广度优先搜索(BFS)

## 迷宫问题

**广度优先搜索**`Breadth First Search`则是以**广度**为第一关键词，当碰到岔道口时，总是先依次访问从该岔道口能**直接到达的所有**结点，然后再按这些结点被访问的顺序去依次访问它们能直接到达的所有结点，以此类推，直到所有结点都被访问为止。

![Snipaste_2020-04-05_16-15-44](https://tvax3.sinaimg.cn/large/005tpOh1gy1gdiynd10l9j30fq0bh75o.jpg)****

这种访问方式很像一个**队列**：

![1](https://tva1.sinaimg.cn/large/005tpOh1gy1gdiyzfmn29j30yl0tp41x.jpg)

因此BFS一般又**队列**来实现，且总是按层次的顺序进行遍历，其**基本写法**如下：

```cpp
void BFS(int s){
    queue<int> q;
    q.push(s);			// s为起点
    while(!q.empty()){
        取出队首元素top;
        访问队首元素top;
        将队首元素出队;
        将top的下一层结点中未曾入队的结点全部入队，并设置为已入队;
    }
}
```

## 通过实例学习BFS

### 找“块”数

**题目**

```shell
给定一个m*n的矩阵，矩阵中的元素为0或者1。称位置(x, y)与其上下左右四个位置(x,y+1),(x,y-1),(x+1,y),(x-1,y)是相邻的。如果矩阵中的若干个1是相邻的（不必两两相邻），那么成这些1构成了一个块。求给定的矩阵中块的个数。
```

***如有下面6\*6矩阵，其块的个数为4:***

![Snipaste_2020-04-05_17-00-43](https://tva2.sinaimg.cn/large/005tpOh1gy1gdizy81u7ej304904oglg.jpg)

**思路**

枚举每一个位置的元素

- 如果是0则跳过
- 如果是1则使用BFS查询与该位置相邻的4个位置，判断它们是否为1
  - 如果为1则同样去查询与该位置相邻的4个位置，直到整个块访问完毕

为了防止走回头路，一般设置`bool型数组inq`(即in queue的简写)来记录每个位置是否在BFS中已经入过队

**代码**

```cpp
#include<cstdio>
#include<queue>
using namespace std;
const int maxn = 100;

struct node{
    int x, y;
}Node;

int n, m; // 矩阵大小
int matrix[maxn][maxn]; // 0|1矩阵
bool inq[maxn][maxn] = {false};    // 判断是否入队
int X[4] = {0, 0, 1, -1};   // 增量矩阵，用于判断四个邻域
int Y[4] = {1, -1, 0, 0};

bool judge(int x, int y){
    if(x >= n || x < 0 || y >= m || y < 0) return false;    // 越界
    if(matrix[x][y] == 0 || inq[x][y] == true) return false; // 0值或者已经入队
    return true;
}

void BFS(int x, int y){
    queue<node> q;      // 定义队列
    Node.x = x;         // 当前结点坐标(x, y)
    Node.y = y;
    q.push(Node);       // 入队
    inq[x][y] = true;   // 设置(x,y)已经入队
    while(!q.empty()){  // 如果队列非空
        node top = q.front();   // 取出队首元素
        q.pop();                // 队首元素出队
        for(int i = 0; i < 4; i++){ // 访问与队首的4个邻位
            int newX = top.x + X[i];
            int newY = top.y + Y[i];
            if(judge(newX, newY)){  // 判断新位置是否访问
                Node.x = newX;
                Node.y = newY;
                q.push(Node);       // 将结点加入队列
                inq[newX][newY] = true; // 设置新位置已经入队标志
            }
        }
    }
}

int main(){
    scanf("%d%d", &n, &m);
    for(int x = 0; x < n; x++){
        for(int y = 0; y < m; y++){
            scanf("%d", &matrix[x][y]); // 读入0|1矩阵
        }
    }
    int ans = 0;        // 记录块数
    for(int x = 0; x < n; x++){
        for(int y = 0; y < m; y++){
            if(matrix[x][y] == 1 && inq[x][y] == false){
                ans++;
                BFS(x, y);
            }
        }
    }
    printf("%d\n", ans);
    return 0;
}
```

```shell
6 6
0 1 1 1 0 0 1
0 0 1 0 0 0 0
0 0 0 0 1 0 0
0 0 0 1 1 1 0
1 1 1 0 1 0 0
1 1 1 1 0 0 0
4
```

### 迷宫从起点到终点的步数

**问题**

```shell
给定一个n*m的迷宫，其中"*"表示不可通过的墙壁，"."表示平地，S表示起点，T代表终点。移动过程中，如果当前位置是(x, y)(下标从0开始)，且每次只能前往上下左右(x,y+1),(x,y-1),(x+1,y),(x-1,y)四个位置的平地，求从起点S到达终点T的最少步数
                        . . . . .
                        . * . * .
                        . * S * .
                        . * * * .
                        . . . T *
在上面样例中，S的坐标为(2,2), T的坐标为(4,3)                              
```

**思路**

由于求的是最少步数，而BFS是通过层次的顺序来遍历的，因此可以从起点S开始计数遍历的层数，那么在到达终点T时的层数就是需要求解的起点S到达终点T的最少步数

**代码**

```cpp
#include<cstdio>
#include<queue>
#include<cstring>
using namespace std;

const int maxn = 100;

struct node{
    int x, y;       // 表示结点坐标
    int step;       // 表示从起点到该结点的步数
}S, T, Node;        // S为起点，T为终点

int n, m;                           // 设置迷宫大小
int maze[maxn][maxn];               // 设置迷宫是*还是.等
bool inqueue[maxn][maxn] = {false}; // 是否进队
int X[4] = {0, 0, 1, -1};           // 增量数组
int Y[4] = {1, -1, 0, 0};

bool judge(int x, int y){                               // 判断是否访问该点
    if(x >=n || x < 0 || y >=m || y < 0) return false;  // 越界
    if(maze[x][y] == '*') return false;                 // 墙壁
    if(inqueue[x][y] == true) return false;              // 已入队过
    return true;
}

int BFS(){            // 广度优先搜索
    queue<node> q;
    q.push(S);                      // 起点入队
    while(!q.empty()){
        node top = q.front();       // 取出队首
        q.pop();                    // 删除队首
        if(top.x == T.x && top.y == T.y){    // 到达终点，直接返回最少步数
            return top.step;
        }

        for(int i = 0; i < 4; i++){         // 遍历4个邻位
            int newX = top.x + X[i];
            int newY = top.y + Y[i];
            if(judge(newX, newY)){
                Node.x = newX;
                Node.y = newY;
                Node.step = top.step + 1;
                q.push(Node);
                inqueue[newX][newY] = true;
            }
        }
    }
    return -1;      // 当无法到达终点时返回-1
}

int main(){
    scanf("%d%d", &n, &m);
    for(int i = 0; i < n; i++){
        getchar();               // 过滤掉每行后面的换行符
        for(int j = 0; j < m; j++){
            maze[i][j] = getchar();
        }
        maze[i][m + 1] = '\0';  // 每一行结尾
    }
    scanf("%d%d%d%d", &S.x, &S.y, &T.x, &T.y);
    S.step = 0;
    printf("%d\n", BFS());
    return 0;
}
```

## queue注意点

在使用STL的queue时，元素入队的push操作只是制造了该元素的一个**副本**入队，对入队后的原元素进行更改不会改变队列中的副本，同样的，对副本更改也不会影响原元素，所以这个原因可能会引入bug

```cpp
#include<cstdio>
#include<queue>
using namespace std;

struct node{
    int data;
}a[10];

int main(){
    queue<node> q;
    for(int i = 1; i <= 3; i++){
        a[i].data = i;
        q.push(a[i]);
    }
    // 将队首元素的数据改成100
    q.front().data = 100;
    // 事实上对队列元素的修改无法改变原元素
    printf("%d %d %d\n", a[1].data, a[2].data, a[3].data);
    // 对原元素修改
    a[1].data = 200;
    // 事实上对原元素的修改不会应该队列中的元素
    printf("%d\n", q.front().data);
    return 0;
}
```

```shell
1 2 3
100
```

为了避免这个问题，在队列中不要存放元素本身，而是它们的**编号**

```cpp
#include<cstdio>
#include<queue>
using namespace std;

struct node{
    int data;
}a[10];

int main(){
    queue<node> q;
    for(int i = 1; i <= 3; i++){
        a[i].data = i;
        q.push(i);
    }

    a[q.front()].data = 100;
    printf("%d\n", a[1].data);
    return 0;
}
```

```shell
100
```

