---
title: 算法笔记--数据结构--二叉查找树与平衡二叉树
date: 2020-04-14 9:30:00
categories: 算法与数据结构
tags: 数据结构
id: Gqq
---

# 二叉查找树`BST`

## 定义

二叉查找树(Binary Search Tree)是一种特殊的二叉树，又称为排序二叉树、二叉搜索树、二叉排序树

- 要么二叉树是一棵空树
- 要么二叉查找树又根结点、左子树、右子树组成，其中左子树和右子树都是二叉查找树
  - 左子树上所有结点的数据域均**小于或者等于**根结点的数据域
  - 右子树上所有的结点都**大于**根结点的数据域<!--more-->

![Snipaste_2020-04-14_08-37-08](https://tva3.sinaimg.cn/large/005tpOh1gy1gdszz3l66uj30h1069aba.jpg)

## 二叉查找树的基本操作

1. **查找**

因为二叉树的性质，所以在查找时只需要对其中一棵子树进行遍历，因此查找将会是从树根到查找结点的**一条路径**，所以最坏复杂度为`O(h)`，h是二叉查找树的高度

- 如果当前根结点root为空，说明查找失败，返回。
- 如果需要查找的值x等于当前根结点的数据域`root->data`，说明查找成功，访问之。
- 如果需要查找的值x小于当前根结点的数据域`root->data`，说明应该往左子树查找，因此向`root->lchild`递归。
- 说明需要查找的值x大于当前根结点的数据域`root->data`,则应该往右子树查找，因此向`root->rchild`递归。

```cpp
void search(node* root, int x){
    if(root == NULL){
        printf("Search Failed\n");
        return;
    }
    if(root->data == x){
        printf("%d\n", root->data);
    }else if(x < root->data){
        search(root->lchild, x);
    }else{
        search(root->rchild, x);
    }
}

```

2. **插入**

- 当对某个需要查找的值在二叉查找树中查找成功，说明结点已经存在
- 如果需要查找的值在二叉树中查找失败，那么说明查找失败的地方一定是结点需要插入的地方

- 插入的时间复杂度为`O(n)`

```cpp
void insert(node* &root, int x){
    if(root == NULL){
        root = newNode(x);
        return;
    }
    if(root->data == x){
        return;			// 查找成功，说明结点存在，直接返回
    }else if(x < root->data){
        insert(root->lchild, x);
    }else{
        insert(root->rchild, x);
    }
}
```

3. **建树**

建立一棵二叉树，就是先后插入n个结点的过程

```cpp
node* create(int data[], int n){
    node* root = NULL;
    for(int i = 0; i < n; i++){
        insert(root, data[i]);
    }
    return root;
}
```

一组相同的数字，插入的顺序不同，最后生成的二叉查找树也不相同

4. **删除**

![Snipaste_2020-04-14_09-07-37](https://tva3.sinaimg.cn/large/005tpOh1gy1gdt0umxkezj308807qaav.jpg)

如图，要删除5，则要用4代替5，之后删除4，4为5的**前驱结点**

或者，要删除5，则要用6代替5，之后删除6，6为5的**后继结点**

`前驱结点`：二叉查找树中比结点权值**小**的**最大结点**称为该结点的前驱

`后继结点`：二叉查找树中比结点权值**大**的**最小结点**称为该结点的后继

```cpp
// 寻找以root为根结点的树中的最大权值结点
node* findMax(node* root){
    while(root->rchild != NULL){
        root = root->rchild;
    }
    return root;
}
// 寻找以root为根结点的树中的最小权值结点
node* findMin(node* root){
    while(root->lchild != NULL){
        root = root->lchild;
    }
    return root;
}
```

**删除的基本思路**

1. 如果当前结点root为空，说明不存在权值为给定权值x的结点，直接返回。
2. 如果当前结点root的权值恰为给定的权值x,说明找到了想要删除的结点，此时进入删除处理。
   - 如果当前结点root不存在左右孩子，说明是叶子结点，直接删除。
   - 如果当前结点root存在左孩子．那么在左子树中寻找结点前驱pre,然后让pre的数据覆盖root，接着在左子树中删除结点pre
   - 如果当前结点root存在右孩子，那么在右子树中寻找结点后继next,然后让next的数据覆盖root，接着在右子树中删除结点next
3. 如果当前结点root的权值大于给的的权值x，则在左子树中递归删除权值为x的结点
4. 如果当前结点root的权值小于给的的权值x，则在右子树中递归删除权值为x的结点

```cpp
void deleteNode(node* &root, int x){
    if(root == NULL) return;
    if(root->data == x){
        if(root->lchild == NULL && root->rchild == NULL){
            root = NULL;       // 置为NULL则访问不到 
        }else if(root->lchild != NULL){
            node* pre = findMax(root->lchild);
            root->data = pre->data;
            deleteNode(root->lchild, pre->data);
        }else{
            node* next = findMin(root->rchild);
            root->data = next->data;
            deleteNode(root->rchild, next->data);
        }
    }else if(root->data > x){
        deleteNode(root->lchild, x);
    }else{
        deleteNode(root->rchild, x);
    }
}
```

**总是优先删除前驱（或者后继）容易导致树的左右子树高度极度不平衡，使得二叉查找树退化成一条链。解决这一问题的办法有两种：一种是每次交替删除前驱或后继：另一种是记录了树高度，总是优先在高度较高的一棵子树里删除结点**

## 二叉查找树的性质

**对二叉查找树进行中序遍历，遍历结果是有序的**

### 实例[PAT A1043]

[**1043** **Is It a Binary Search Tree**](https://pintia.cn/problem-sets/994805342720868352/problems/994805440976633856)

**对镜像树的先序遍历只需要在原树的先序遍历时交换左右子树的访问顺序即可，镜像树的后序遍历也是同理**

```cpp
#include<cstdio>
#include<vector>
using namespace std;

struct node {
    int data;
    node* lchild;
    node* rchild;
};

// origin为初始输入结点，pre为前序遍历，post为后序遍历
// preM为镜像的前序遍历，postM为镜像的后序遍历
vector<int> origin, pre, preM, post, postM;


void insert(node* &root, int x){
    if(root == NULL){
        node* mid = new node;
        mid->data = x;
        mid->lchild = mid->rchild = NULL;
        root = mid;
        return;
    }
    if(root->data > x){
        insert(root->lchild, x);
    }else{
        insert(root->rchild, x);
    }
}

void preOrder(node* root, vector<int> &pre){
    if(root == NULL) return;
    pre.push_back(root->data);
    preOrder(root->lchild, pre);
    preOrder(root->rchild, pre);
}

void postOrder(node* root, vector<int> &post){
    if(root == NULL) return;
    postOrder(root->lchild, post);
    postOrder(root->rchild, post);
    post.push_back(root->data);
}

void preOrderM(node* root, vector<int> &preM){
    if(root == NULL) return;
    preM.push_back(root->data);
    preOrderM(root->rchild, preM);
    preOrderM(root->lchild, preM);
}

void postOrderM(node* root, vector<int> &postM){
    if(root == NULL) return;
    postOrderM(root->rchild, postM);
    postOrderM(root->lchild, postM);
    postM.push_back(root->data);
}


int main(){
    int n;      // 结点数
    int data;   // 输入结点值
    node* root = NULL;
    scanf("%d", &n);
    for(int i = 0; i < n; i++){
        scanf("%d", &data);
        origin.push_back(data);
        insert(root, data);
    }
    preOrder(root, pre);
    postOrder(root, post);
    preOrderM(root, preM);
    postOrderM(root, postM);
    if(origin == pre){
        printf("YES\n");
        for(int i = 0; i < post.size(); i++){
            printf("%d", post[i]);
            if(i < post.size() - 1) printf(" ");
        }
    }else if(origin == preM){
        printf("YES\n");
        for(int i = 0; i < postM.size(); i++){
            printf("%d", postM[i]);
            if(i < postM.size() - 1) printf(" ");
        }
    }else{
        printf("NO\n");
    }
    return 0;
}

```

# 平衡二叉树AVL

对于二叉查找树，可能遇到长链型的树，此时，对这棵树的结点进行查找复杂度将为`O(n)`，为了保持在`log(n)`级别，引入了`AVL`(2个科学家名字缩写)

![Snipaste_2020-04-14_15-39-58](https://tva1.sinaimg.cn/large/005tpOh1gy1gdtc802elkj309u05cjrw.jpg)

`AVL树`：

- 仍然是一个二叉查找树
- 所以平衡是指，对AVL树的任意结点来说，其左子树与右子树的高度之差的**绝对值不超过1**，其中左子树与右子树的高度之差称为`平衡因子`

![Snipaste_2020-04-14_15-41-01](https://tva4.sinaimg.cn/large/005tpOh1gy1gdtc89wohcj30gf04caae.jpg)



**平衡二叉树的结构体**

```cpp
struct node {
    int data, height;			// data为结点权值，height为以当前结点为子树的树高
    node *lchild, *rchild;
}
```

**基本函数**

```cpp
// 生成一个新结点，v为结点权值
node* newNode(int v){
    node* Node = new node;
    Node-> v = v;
    Node->lchild = Node->rchild = NULL;
    Node->height = 1;		// 结点高度初始为1
    return Node;	
}

// 获取以root为根结点的子树的当前高度
int getHeight(node* root){
    if(root == NULL) return 0;
    return root->height;
}

// 计算结点root的平衡因子
int getBalanceFactor(node* root){
    // 左子树的高度减去右子树的高度
    return getHeight(root->lchild) - getHeight(root->rchild);
}
```

**无法通过子树的平衡因子来计算该结点的平衡因子，而是需要借助子树的高度间接求得**

**显然，结点root所在子树的height等于其左子树的height与右子树的height的较大值加1**，所以通过下面函数更新height

```cpp
void updateHeight(node* root){
    // max(左孩子的height,右孩子的height) + 1
    root->height = max(getHeight(root->lchild), getHeight(root->rchild)) + 1;
}
```

## 平衡二叉树的基本操作

### 查找操作

```cpp
void search(node* root, int x){
    if(root == NULL) return;
    if(x == root->data){
        printf("%d\n", root->data);
    }else if(x < root->data){
        search(root->lchild, x);
    }else{
        search(root->rchild, x);
    }
}
```

## 插入操作

### 左旋与右旋的概念

**🛑在二叉查找树的基础上**

- **🅰左旋**

![Snipaste_2020-04-14_15-58-39](https://tva1.sinaimg.cn/large/005tpOh1gy1gdtcqm38nsj30ck05974y.jpg)

由于`A<🔹<B`，左旋后，A为B的左子树，🔹为A的右子树

**步骤**

1. 让B的左子树🔹成为A的右子树
2. 让A成为B的左子树
3. 将根结点设定为结点B

![Snipaste_2020-04-14_15-58-47](https://tvax4.sinaimg.cn/large/005tpOh1gy1gdtcuiyn6jj30ld06wwgg.jpg)

```cpp
// 左旋Left Rotation
void L(node* &root){
    node* temp = root->rchild;
    root->rchild = temp->lchild;
    temp->rchild = root;
    updateHeight(root);			// 更新结点A的高度
    updataHeight(temp);			// 更新结点B的高度
    root = temp;
}
```



- 🅱**右旋**

![Snipaste_2020-04-14_16-47-27](https://tvax4.sinaimg.cn/large/005tpOh1gy1gdte57ulrmj30du05a752.jpg)

**步骤**

1. 让A的右子树🔹成为B的左子树
2. 让B成为A的右子树
3. 将根结点设定为结点A

![Snipaste_2020-04-14_16-47-37](https://tvax4.sinaimg.cn/large/005tpOh1gy1gdte6mzk3xj30kk072abz.jpg)

```cpp
// 右旋Right Rotation
void R(node* &root){
    node* temp = root->lchild;
    root->lchild = temp->rchild;
    temp->rchild = root;
    updataHeight(root);
    updateHeight(temp);
    root = temp;
}
```

### 插入类型

对于失衡结点，其平衡因子只能是2或者-2.

只要把最靠近插入结点的失衡结点调整到正常，路径上的所有结点就都会平衡

1. LL型

<img src="https://tvax4.sinaimg.cn/large/005tpOh1gy1gdtedr13exj30hs08ggn4.jpg" alt="Snipaste_2020-04-14_16-55-34" style="zoom:80%;" />

2. RR型

<img src="https://tvax3.sinaimg.cn/large/005tpOh1gy1gdteepoa5nj30gp085dh8.jpg" alt="Snipaste_2020-04-14_16-56-39" style="zoom:80%;" />

3. LR型

<img src="https://tva2.sinaimg.cn/large/005tpOh1gy1gdtefn342fj30j707rwgg.jpg" alt="Snipaste_2020-04-14_16-57-39" style="zoom:80%;" />

4. RL型

<img src="https://tva1.sinaimg.cn/large/005tpOh1gy1gdtege5ifsj30lc082jtd.jpg" alt="Snipaste_2020-04-14_16-58-20" style="zoom:80%;" />

![Snipaste_2020-04-14_17-00-45](https://tva1.sinaimg.cn/large/005tpOh1gy1gdteivx2zkj30nu07yjue.jpg)

```cpp
void insert(node* &root, int data){
    if(root == NULL){		// 到达空结点
        root = NewNode(data);	// 插入
        return;
    }
    if(data < root->data){			// data比根结点的权值小
        insert(root->lchild, data);	// 往左子树插入	
        updateHeight(root);			// 更新树高
        if(getBalanceFactor(root) == 2){		// 失衡结点值为2是
            if(getBalanceFactor(root->lchild == 1)){	// LL型
                R(root);
            }else if(getBalanceFactor(root->lchild == -1)){	// LR型
                L(root->lchild);
                R(root);
            }
        }
    }
    else{
        insert(root->rchild, data);
        updateHeight(root);
        if(getBalanceFactor == -2){
            if(getBalanceFactor(root->rchild) == -1){	// RR型
                L(root);	
            }else if(getBalanceFactor(root->rchild) == 1){	// RL型
                R(root->rchild);
                L(root);
            }
        }
    }
}
```

## AVL树的建立

```cpp
node* create(int data[], int n){
    node* root = NULL;
    for(int i = 0; i < n; i++){
        insert(root, data[i]);
    }
    return root;
}
```

