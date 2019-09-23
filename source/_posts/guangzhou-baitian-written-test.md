---
title: 广州百田 2019 笔试题
date: 2019-09-22 22:05:54
update: 2019-09-22 22:05:54
tags:
    - Interview
categories:
    - Programming
---

趁着秋招，因为同学的推荐，投递了广州百田。这两天百田发送邮件，通知线上笔试。内容摘要如下：

> 百奥（广州百田）邀请您参加线上笔试，试卷名称是： 开发工程师-百奥2020届校园招聘笔试（笔试题根据网申岗位而定）
> 请务必在试卷开放时间 2019-09-21 9:00:00 至 2019-09-23 21:00:00 完成答卷，
> 请复制下方的试卷链接到浏览器中作答。（Web前端、游戏前端、Java后端开发均为同一套题）
> https://ks.youkaoshi.cn/doexam/xDoPQYQ2gv.html
> 祝福：信心是成功的一半，祝您考试顺利！

笔试题总共有三道，都是非常基础的题目，现记录如下。

<!-- more -->

注 1: 百田笔试题给定的某些题目描述是 C++，某些是 Java，而且答题语言不限。
我提供的题目和答案则都是 C++ 描述。

注 2: 在和同学讨论后，我们一致认为百田的线上笔试有漏洞。首先，和同学讨论过后得知，我们的测试题目相同。

从发送的邮件内容：

> （Web前端、游戏前端、Java后端开发均为同一套题）

也可得知，统一批次的线上笔试题应该相同。那么如果有两个认识的人参加同一轮线上笔试，
并且在不同时间点做题，那么先做的那个人就可以知道题目，然后就可以告知后做的人。
那么，后做题的人就可先行准备。因为我和同学是同时开始答题，则无法利用此漏洞。
当然，我们也并不打算利用此漏洞，只是觉得有必要说明一下。

## 分割链表

假定有一个链表，编写一个函数，将该按如下规则分成三个链表。

首先，给定的节点定义如下：
```cpp
struct Node {
    char content;
    Node* next;
};
```
然后，规则如下：

1. 如果该节点的 content 为大写字母，则插入第一条链表。
2. 如果该节点的 content 为数字，则插入第二条链表。
3. 其他则插入第三条链表。

最后，需要完成的函数定义如下：

```cpp
std::vector<Node*> split(Node *head);
```

题解：

```cpp
vector<Node*> split(Node *head) {
    vector<Node*> curs(3, NULL);
    vector<Node*> heads(3, NULL);
    Node *cur = head;
    while (cur != NULL) {
        Node *next = cur->next;
        cur->next = NULL;
        char c = cur->content;
        int which = 2;
        if (c >= 'A' && c <= 'Z') {
            which = 0;
        } else if (c >= '0' && c <= '9') {
            which = 1;
        }
        if (curs[which] == NULL) {
            curs[which] = cur;
            heads[which] = curs[which];
        } else {
            curs[which]->next = cur;
            curs[which] = curs[which]->next;
        }
        cur = next;
    }
    return heads;
}
```

测试

```cpp
#include <iostream>
#include <vector>
using namespace std;
struct Node {
    char content;
    Node *next;
    Node(char c) : content(c), next(NULL) {  }
};
void print_nodes(Node* head) {
    Node *cur = head;
    while (cur != NULL) {
        cout << cur->content;
        cur = cur->next;
    }
    cout << "\n";
}
void test() {
    string s = "A233CD@#SD2";
    Node *head = new Node(s[0]);
    Node *cur = head;
    for (int i = 1; i < s.size(); i++) {
        cur->next = new Node(s[i]);
        cur = cur->next;
    }
    vector<Node*> curs = split(head);
    for (int i = 0; i < curs.size(); i++) {
        print_nodes(curs[i]);
    }
}
int main() {
    test();
    return 0;
}
```

## 打乱数组

注： 题目给定的应该是 Java 形式的函数定义，为 `void shuffle(int[] orderArray)`

给定一个数组，打乱该数组。

需要完成的函数定义如下：

```cpp
void shuffle(vector<int> orderArray);
```

提供一个生成随机数的函数 `rand(min, max)` 返回值为 `[min, max)` 之间的一个整数。

题解：

```cpp
void shuffle(vector<int> &orderArray) {
    int len = orderArray.size();
    for (int i = len - 1; i >= 0; i--) {
        int r = rand(i, len);
        int t = orderArray[r];
        orderArray[r] = orderArray[i];
        orderArray[i] = t;
    }
}
```

测试：

```cpp
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;
#define rand(min, max) rand() % (max - min) + min
void test() {
    vector<int> orderArray(10);
    for (int i = 0; i < orderArray.size(); i++) {
        orderArray[i] = i;
    }
    shuffle(orderArray);
    for (int i = 0; i < orderArray.size(); i++) {
        cout << orderArray[i] << " ";
    }
    cout << "\n";
}
int main() {
    test();
    return 0;
}
```

## 计算前缀表达式的值

给定一个以前缀表达式表示的字符串，计算其结果。

例：
~~~
-*+1234
~~~
等价于下面的中缀表达式：
~~~
(1 + 2) * 3 - 4
~~~
其结果为 5。

给定的函数定义如下：

```cpp
int eval(string exp);
```

```cpp
int eval(string exp) {
    int len = exp.size();
    stack<int> st;
    for (int i = len - 1; i >= 0; i--) {
        char c = exp[i];
        if (c >= '0' && c <= '9') {
            st.push(c-'0');
        } else {
            int a = st.top(); st.pop();
            int b = st.top(); st.pop();
            int r;
            switch (c) {
                case '+':
                    r = a + b;
                    break;
                case '-':
                    r = a - b;
                    break;
                case '*':
                    r = a * b;
                    break;
            }
            st.push(r);
        }
    }
    return st.top();
}
```

测试：

```cpp
#include <iostream>
#include <stack>
using namespace std;
void test() {
    // (1 + 2) * 3 - 4 = 5
    string exp = "-*+1234";
    int val = calc(exp);
    if (val == 5) {
        cout << "test pass\n";
    } else {
        cout << "test fail\n";
    }
}
int main() {
    test();
    return 0;
}
```

## TODO

**题目解析等明天再弄，今天有点晚了。**