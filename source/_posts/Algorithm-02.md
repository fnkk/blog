---
title: Algorithm-02
author: Fnkk
avatar: /images/24_01/snake.png
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
comments: true
date: 2024-01-17 09:45:30
tags:
keywords:
description:
photos:
---
## 旋转链表
**题目：** 给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。
- 示例 1
    输入：head = [1,2,3,4,5], k = 2
    输出：[4,5,1,2,3]
- 示例 2
    输入：head = [0,1,2], k = 4
    输出：[2,0,1]
## 思路
    只要找到旋转后链表头位置记为J，把原来的链表头尾相连，在从J位置断开即可。
## 代码实现
```
var rotateRight = function(head, k) {
    if (!head || k === 0) {
        return head;
    }
    let length = 1;
    let tail = head;
    while (tail.next) {
        tail = tail.next;
        length++;
    }
    k = k % length;
    if (k === 0) {
        return head;
    }
    let newTail = head;
    for (let i = 0; i < length - k - 1; i++) {
        newTail = newTail.next;
    }
    let newHead = newTail.next;
    newTail.next = null;
    tail.next = head;

    return newHead;
};
```
## 感受
中等难度的题目写起来还是比较顺手的，但是算不上得心应手，花费的时间挺多的，要再巩固多几题，再尝试困难的题目。