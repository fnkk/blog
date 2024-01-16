---
title: Algorithm_01
author: Fnkk
avatar: /images/24_01/snake.png
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
comments: true
date: 2024-01-16 10:17:46
tags:
keywords:
description:
photos: /images/24_01/wzx_01.jpg
---
# 前言 
在数字时代，算法的重要性日益凸显。意识到这一点，我计划利用闲暇时间练习算法题目，以提高我的编程技巧和逻辑思维。这不仅是对个人技能的提升，也是对未来职业发展的投资。解决算法问题的过程既富有挑战性又充满乐趣，每次突破都让我获得巨大的成就感。

我将记录我的学习历程和解题思路，希望这些笔记不仅能帮助自己回顾和巩固知识，同时也能为同样对算法感兴趣的朋友提供参考和启发。面对算法的复杂性和多样性，我相信持续的练习和分享是理解和掌握它们的最佳途径。
## 全排列
**题目：**给定一个不含重复数字的数组 nums，返回其所有可能的全排列。你可以按任意顺序返回答案。
- 示例 1：
    输入：nums = [1,2,3]
    输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
- 示例2：
    输入：nums = [0,1]
    输出：[[0,1],[1,0]]
- 示例3：
    输入：nums = [1]
    输出：[[1]]
- 提示：
    1 <= nums.length <= 6
    -10 <= nums[i] <= 10
    nums 中的所有整数 互不相同
### 思路
- 数组nums不含重复的数字，所以只要每个元素排列的顺序不同，就不会出现重复的结果。因此只需从前往后，把每种可能性遍历出来即可。
- 使用递归的方式，迭代出所有的可能性。
### 代码实现

```
var permute = function (nums) {
    let res = []
    dfs(nums,[])
    function dfs(nums, arr) {
        if (nums.length == 0) {
            res.push(arr)
            return
        }
        for (let i = 0; i < nums.length; i++) {
            let temp = [...nums.slice(0, i), ...nums.slice(i + 1)]
            let newArr = arr
            newArr.push(nums[i])
            dfs(temp, newArr)
        }
    }
    return res
};
```
- 我最开始是这样写的，但是输出的结果的长度有问题，检查后推测是newArr这里可能引用了同样的地址，导致后面的结果继续push到已经存入res的结果中，于是改为下面的写法，成功通过了测试。


```
var permute = function (nums) {
    let res = []
    dfs(nums,[])
    function dfs(nums, arr) {
        if (nums.length == 0) {
            res.push(arr)
            return
        }
        for (let i = 0; i < nums.length; i++) {
            let temp = [...nums.slice(0, i), ...nums.slice(i + 1)]
            dfs(temp, [...arr,nums[i]])
        }
    }
    return res
};
```