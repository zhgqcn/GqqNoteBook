---
title: 算法笔记--标准模板库STL--string
date: 2020-03-27 16:00:00
categories: 算法与数据结构
tags: 
	- 算法
	- STL
id: Gqq
---

# string 的常见用法详解

**头文件**

```cpp
#include<string>
using namespace std;
```

<!--more-->

# string 的定义

```cpp
string str;

string str = "Gqq";
```



# string的内容访问

## 通过下标访问

```cpp
#include<cstdio>
#include<string>
using namespace std;

int main(){
    string str = "gqq";
    for(int i = 0; i < str.length(); i++){
        printf("%c", str[i]);
    }
    return 0;
}
```

**如果要读入和输出整个字符串，只能用cin和cout**

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str;
    cin >> str;
    cout << str;
    return 0;
}
```

**用printf输出string时，用c_str()将string类型转换为字符数组进行输出**

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str;
    cin >> str;
    printf("%s", str.c_str());
    return 0;
}
```

## 通过迭代器访问

**定义迭代器**

```cpp
string::iterator it;
```

**通过迭代器\*it来访问string里的每一位**

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str;
    cin >> str;
    for(string::iterator it = str.begin(); it != str.end(); it++){
        printf("%c", *it);
    }
    return 0;
}
```

**`string`和`vector`一样，支持直接对迭代器进行加减某个数字，如`str.begin() + 3`**

# string 常用函数

## operator+=

> string的加法， 将两个string直接拼接起来

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq";
    string str2 = " loves apples!";
    string str3 = str1 + str2;
    str1 += str2;   // str2直接拼接在str1上
    cout << str3 << endl;
    cout << str1 << endl;
    return 0;
}
```

```shell
Gqq loves apples!
Gqq loves apples!
```

## compare operator

> 两个string类型可以直接使用`==`、`!=`、`<`、`<=`、`>`、`>=`比较大小，比较规则是字典序

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "aa";
    string str2 = "aaa";
    string str3 = "abc";
    string str4 = "gqq";
    if(str1 < str2) printf("ok1\n");
    if(str1 != str3) printf("ok2\n");
    if(str4 >= str3) printf("ok3\n");
    return 0;
}
```

## length( ) / size( )

> length( ) 返回string的长度，即存放的字符数，时间复杂度为O(1)。size( )和length( )基本相同

```
string str = "gqq";
printf("%d" %d\n", str.length(), str.size());
```

```shell
3 3
```

## insert( )

1. **insert( pos, string)**， pos号位置插入字符串string

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq apples";
    string str2 = " loves";
    str1.insert(3, str2);
    printf("%s", str1.c_str());
    return 0;
}
```

```shell
Gqq loves apples
```

2. **insert(it1, it2, it3)**，`it1`为原字符串的欲插入位置，`it2`和`it3`为待插字符串的`首尾迭代器`，用来表示串`[it2, it3)`将被插在`it1`的位置上

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq apples";
    string str2 = " loves";
    str1.insert(str1.begin() + 3, str2.begin(), str2.end());
    cout << str1 << endl;
    return 0;
}
```

```shell
Gqq loves apples
```

## erase( )

1. **删除单个元素**

> `str.erase(it)`用于删除单个元素，`it`为需要删除的元素的迭代器

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq loves apples";
    str1.erase(str1.begin() + 2);
    cout << str1 << endl;
    return 0;
}
```

```shell
Gq loves apples
```

2. **删除区间内的所有元素**

> `str.erase(first, last)`，其中first为需要删除的区间的其实迭代器，而last则为需要删除区间的末尾迭代器的下一个地址，即`[first, last)`

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq loves apples";
    str1.erase(str1.begin() + 2, str1.begin() + 9);
    cout << str1 << endl;
    return 0;
}
```

```shell
Gq apples
```

> `str.erase(pos, length)`，`pos`为需要开始删除的起始位置，`length`

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq loves apples";
    str1.erase(3, 6);	// 删除从3号位开始的6个字符
    cout << str1 << endl;
    return 0;
}
```

```shell
Gqq apples
```

## clear( )

> clear( ) 用以清空string中的数据，时间复杂度为O(1)

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq loves apples";
    str1.clear();
    cout << str1.length() << endl;
    return 0;
}
```

**Result ： 0**

## substr( )

> substr(pos, len) 返回从pos号位开始，长度为len的子串，时间复杂度为O(len)

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str1 = "Gqq loves apples";
    string str2 = str1.substr(0, 3);
    cout << str2 << endl;
    return 0;
}
```

```shell
Gqq
```

## string::npos

> string::npos是一个常数，其本身的值为-1， 但是由于是unsigned_int类型，因此实际上也可以认为是unsigned_int类型的最大值。string::npos用以作为find函数失配时的返回值。

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    if(string::npos == -1)
        cout << "-1 is true" << endl;
    if(string::npos == 4294967295)
        cout << "4294967295 is true" << endl;
    return 0;
}
```

```shell
-1 is true
4294967295 is true
// 可以认为string::npos就是可以等于-1或者4294967295
```

## find( )

> str.find(str2)，当str2是str的子串时，返回其在str中第一次出现的位置
>
> 如果str2不是str的子串，返回string::npos

> str.find(str2, pos)，从str的pos号位开始匹配str2，返回值与上面相同

**时间复杂度为O(nm)，其中n和m分别是str和str2的长度**

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str = "Thank you for your smile";
    string str2 = "you";
    string str3 = "me";
    if(str.find(str2) != string::npos){
        cout << str.find(str2) << endl;
    }
    if(str.find(str2, 7) != string::npos){
        cout << str.find(str2, 7) << endl;
    }
    if(str.find(str3) != string::npos){
        cout << str.find(str3) << endl;
    }else{
        cout << "I know there is no position for me." << endl;
    }
    return 0;
}
```

```shell
6
14
I know there is no position for me.
```

## replace( )

> `str.replace(pos, len, str2)`把`str`从`pos`号位开始，长度为`len`的子串替换为`str2`
>
> `str.replace(it1, it2, str2)`把`str`的迭代器`[it1, it2)`范围的子串替换为`str2`

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string str = "Thank you for your smile";
    string str2 = "the dinner";
    string str3 = "Thanks";
    cout << str.replace(14, 10, str2) << endl;
    cout << str.replace(str.begin(), str.begin() + 9, str3);

    return 0;
}
```

```shell
Thank you for the dinner
Thanks for the dinner
```

 # 练习 PAT A1060

```shell
题目描述：
	If a machine can save only 3 significant digits, the float numbers 12 300 and 12 358.9 are considered equal since they are both saved as 0.123*10^5 with simple chopping. Now given the number of significant digits on a machine and two float numbers, you are supposed to tell if they are treated equal in that machine.

输入格式:
	Each input file contains one test case which gives three numbers N, A and B, where N (<100) is the number of significant digits, and A and B are the two float numbers to be compared. Each float number is non-negative, no greater than 10100, and that its total digit number is less than 100.

输出格式:
	For each test case, print in a line “YES” if the two numbers are treated equal, and then the number in the standard form “0.d1…dN*10^k” (d1>0 unless the number is 0); or “NO” if they are not treated equal, and then the two numbers in their standard form. All the terms must be separated by a space, with no extra space at the end of a line.
Note: Simple chopping is assumed without rounding.

输入样例1: 
3 12300 12358.9

输出样例1: 
YES 0.123*10^5

输入样例2: 
3 120 128

输出样例2: 
NO 0.120*10^3 0.128*10^3
```

**解题思路**

我们可能遇到大于0或者小于0的两种情况数字，如

- 0.0001 = 0.100 * 10^-3  指数为小数点后0的个数的相反数
- 123.01 = 0.123 * 10^3   指数为小数点前数字的个位数

但是，由于可能隐含的出现数字前面有多个零，如

- 🅰00000.0001
- 🅱00123.01

所以，对于求解，有两个分类：

- 对于🅰情况，将前导零、小数点、第一个非零位前面的0全部删除，只保留第一个非零位开始的部分
- 对于🅱情况，将前导零、小数点删除

通过上面步骤，即可得到指数e，之后根据精度将字符串拼接即可，代码如下：

```cpp
#include<iostream>
#include<string>
using namespace std;
int n;                      // 有效位数
string deal(string s, int& e){
    int k = 0;              // S的下标
    while(s.length() > 0 && s[0] == '0'){
        s.erase(s.begin()); // 去掉s的前导零
    }
    if(s[0] == '.'){        // 去掉前导零后是小数点，说明s小于1
        s.erase(s.begin()); // 去掉小数点
        while(s.length() > 0 && s[0] == '0'){
            s.erase(s.begin());
            e--;
        }
    } else {    // 去掉前导零后不是小数点，则找到后面的小数点删除
        while(k < s.length() && s[k] != '.'){
            k++;
            e++;
        }
    }
    if(k < s.length()){ // while结束后k < s.length()，说明碰到了小数点
        s.erase(s.begin() + k); // 删除小数点
    }
    if(s.length() == 0){
        e = 0;  // 如果去除前导零后s的长度变为0，说明这个数是0
    }
    int num = 0;
    k = 0;
    string res;
    while(num < n){             // 只要精度还没有到n
        if(k < s.length()) res += s[k++];   // 只要还有数字，就加到res末尾
        else res += '0';    // 否则res末尾添加0
        num++;              // 精度加1
    }
    return res;
}
int main(){
    string s1, s2, s3, s4;
    cin >> n >> s1 >> s2;
    int e1 = 0, e2 = 0;
    s3 = deal(s1, e1);
    s4 = deal(s2, e2);
    if(s3 == s4 && e1 == e2)
        cout << "YES 0." << s3 << "*10^" << e1 << endl;
    else
        cout << "NO 0." << s3 << "*10^" << e1 << " 0." << s4 << "*10^" << e2 << endl;
    return 0;
}
```

**结果**

```shell
3 12300 12358.9
YES 0.123*10^5
```

