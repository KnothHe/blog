---
title: LeetCode 147 链表的插入排序
date: 2019-10-28 15:44:09
updated: 2019-10-28 15:44:09
tags:
    - LeetCode
    - Online Judge
    - Algorithm
    - Linked List
categories:
    - Programming
---

题目描述：

> Sort a linked list using insertion sort.
> Algorithm of Insertion Sort:
> 1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
> 2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
> 3. It repeats until no input elements remain.

<!-- more -->

单链表的定义：

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

想法：

使用两条链表，一条是已经有序的链表 A，一条是待排序的链表 B。将节点从待排序的链表 B 依次插入已经有序的链表 A。

关键点在于想到使用两条链表。

代码：

```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if (head == null || head.next == null) { return head; }

        ListNode fakeHead= new ListNode(0); // because we need insert before head
        ListNode cur = head; // current node will be insert
        ListNode pre = null; // previous node of insert node
        ListNode next = null; // next node of current insert node

        while (cur != null) {
            // save next position
            next = cur.next;

            // find insert position
            pre = fakeHead;
            while (pre.next != null && pre.next.val < cur.val) {
                pre = pre.next;
            }

            // insert cur between pre and pre.next
            cur.next = pre.next;
            pre.next = cur;

            // move point forward
            cur = next;
        }

        return fakeHead.next;
    }
}
```