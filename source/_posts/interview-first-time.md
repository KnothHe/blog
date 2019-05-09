---
title: 初次笔试题
date: 2019-05-09 10:57:00
update: 2019-05-09 11:43:00
tags:
    - Interview
---

昨天参加了学校的一个企业实习的宣讲会，结束后做了一套笔试题。原本在签到的时候我选择的应聘岗位是后端开发，但是在选择笔试试卷是还是选择了 C/C++ 开发。和编程相关的题目总共是 4 道题。现场做题的时候有很多的细节没有考虑到，而且我更习惯使用电脑写代码，没有多少在纸上写代码的经历，最后只写了 3 道，一道空白。我的感觉是这些面试题中有一道题并不适合笔试题这种场合。最重要的是很基础的题目我并没有能够做到 bug-free。我的算法的基础还需要继续巩固。特记录如下。

<!-- more -->

题目记录如下：

## C 语言 `strcat` 的实现，和库函数 `strcat` 并不相同，声明如下：

```c
char* strcat(const char* a, const char* b)
```

大致上的思路是先计算出 a 和 b 的长度，然后根据 a 和 b 的长度计算出需要分配给返回的字符串的内存空间的大小，最后将 a 和 b 的内容依次拷贝到返回字符串中。

代码如下：

```c
#include <stdio.h>
#include <stdlib.h>

char* strcat_m(const char* a, const char* b)
{
    int aLen = 0;
    for (; a[aLen] != '\0'; ++aLen) {
        ; 
    }
    int bLen = 0;
    for (; b[bLen] != '\0'; ++bLen) {
        ;
    }
    int len = aLen + bLen;
    char* str = malloc(sizeof(char) * (len+1));
    char* p = str;
    for (int i = 0; i < aLen; ++i) {
        *p = a[i];
        ++p;
    }
    for (int i = 0; i < bLen; ++i) {
        *p = b[i];
        ++p;
    }
    *p = '\0';
    return str;
}

int main(void)
{
    char a[10] = "123";
    char b[10] = "456";
    char *str = strcat_m(a, b);
    printf("%s\n", str);
    free(str);

    return 0;
}
```

最后在写的过程中发现 C 语言中对字符串的遍历操作还是指针比较方便。

## 链表逆序

这个是对链表操作的基础的考察。

代码如下：

```c
#include <stdio.h>
#include <stdlib.h>

struct node {
    char ch; 
    struct node* next;
};

typedef struct node Node;

void printLinkedList(Node* head)
{
    Node* p = head;
    while (p != NULL) {
        printf("%c", p->ch);
        p = p->next;
    }
    printf("\n");
}

Node* helper(Node* p)
{
    if (p == NULL) return NULL;
    Node* n = p->next;
    n = helper(n);
    if (n != NULL) {
        n->next = p;
    }
    return p;
}

Node* reverse(Node* head)
{
    if (head == NULL) return NULL;
    Node* h = head;
    while (h->next != NULL) {
        h = h->next;
    }
    Node* last = helper(head);
    last->next = NULL;
    return h;
}

int main(void)
{
    Node* head = malloc(sizeof(Node));
    head->ch = 'a';
    head->next = NULL;

    Node* p = head;
    for (int i = 1; i <= 3; i++) {
        Node* t = malloc(sizeof(Node));
        t->ch = 'a' + i;
        t->next = NULL;
        p->next = t;
        p = p->next;
    }
    printLinkedList(head);
    head = reverse(head);
    printLinkedList(head);
    
    return 0;
}
```

在上述代码写完之后，我想到的是如果可以使用栈的话，只要先将所有节点压入栈，在弹出节点指针的过程中，先将第一个出栈的节点记录为头节点，之后依次链接，最后一个节点链接 `NULL`，这样程序的逻辑就会更加简单，缺点就是引入了额外的数据结构。

当然，上述的递归代码也是隐式地使用了函数调用栈。总的来说，我的考虑中链表逆序是必须要使用到栈的。

## 快速排序

这道题就是我觉得不应该出现在面试题中的题目。快排的话，会和不会都是在笔试之前就已经确定了的，并且并不太能体现处应试者的编程（算法）水平。毕竟，没有多少人会没事写个快排。

虽说如此，还是写了。代码如下：

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

int partition(vector<int> &vec, int beg, int end)
{
    int p = beg;
    ++beg;
    while (beg <= end) {
        while (vec[beg] <= vec[p] && beg <= end) {
            ++beg;
        }
        while (vec[end] >= vec[p] && end >= beg) {
            --end;
        }
        if (beg >= end) break;
        swap(vec[beg], vec[end]);
    }
    swap(vec[p], vec[end]);
    return end;
}

void helper(vector<int> &vec, int beg, int end)
{
    if (beg >= end) return;
    int p = partition(vec, beg, end);
    helper(vec, beg, p-1);
    helper(vec, p+1, end);
}

void quicksort(vector<int> &vec)
{
    helper(vec, 0, vec.size()-1);
}

void printVec(vector<int> &vec)
{
    for (const auto &v : vec) {
        cout << v << " ";
    }
    cout << endl;
}

int main(void)
{
    vector<int> vec = {4, 2, 1, 5, 6, 0, 9, 7, 8 ,3};
    printVec(vec);
    quicksort(vec);
    printVec(vec);
    
    return 0;
}
```

基本的步骤就是三步：

1. 切分
2. 递归排序前半部分
3. 递归排序后半部分

最后，需要注意递归终止的条件。

这三步中最重要的就是切分这一步。

## 要求打印出汉诺塔的移动序列

最初接触到这道题是我大二开始学习计算机算法最初接触递归的时候。再次碰到的时候只考虑到了移动次数，内心觉得移动序列可能比较麻烦，就没再考虑。笔试时也没有多加思考就放弃了。

在回宿舍的路上大致想明白了思路（还有缺陷）：

1. 递归打印 `N-1`
2. 打印 `N`
3. 递归打印 `N-1`

没有考虑到的是需要将 `A B C` 三个柱子看作三个区域，目标是将整个汉诺塔依照要求从 `A` 移动到 `C`。这就需要考虑每一步移动时将 `A B C` 分别看作是 `源(from)`、`缓冲区(buffer)` 和 `目的(to)`。

思路如下：

1. 递归将 `N-1` 从 `from` 以 `to` 为缓冲区移动到 `buffer`
2. 将 `N` 从 `from` 移动到 `to`
3. 递归将 `N-1` 从 `buffer` 以 `from` 为缓冲区移动到 `to`

关键代码如下：

```c
helper(N-1, from, to, buffer);
printf("Move disk %d from %c to %c\n", N, from, to);
helper(N-1, buffer, from, to);
```

完整代码如下：

```c
#include <stdio.h>

void helper(int N, char from, char buffer, char to)
{
    if (N <= 0) return;

    helper(N-1, from, to, buffer);
    printf("Move disk %d from %c to %c\n", N, from, to);
    helper(N-1, buffer, from, to);
}

void print_hannoi(int N)
{
    helper(N, 'A', 'B', 'C');
}

int main(void)
{
    print_hannoi(1);
    printf("\n");
    print_hannoi(2);
    printf("\n");
    print_hannoi(3);
    printf("\n");

    return 0;
}
```

## 总结

第一次笔试没经验。

但是这家公司的 C/C++ 工程师的 4 道笔试题中有三道是需要用到递归实现的……