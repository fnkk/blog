---
title: VuejsDesignAndImplementation
author: Fnkk
avatar: /images/24_01/snake.png
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
comments: true
date: 2024-02-01 14:32:31
tags:
keywords:
description:
photos:
---
# 第一章：权衡的艺术
1. 命令式与声明式
- 命令式：关注过程
- 声明式关注结果
2. 性能与可维护性的权衡
- 命令式效率高而声明式可维护性更强
- 效率对比：原生JavaScript、InnerHtml、虚拟DOM
    心智负担（做一件事需要做的额外的事）：虚拟DOM < innerHtml < 原生Javascript
    性能： innerHTML < 虚拟DOM < 原生Javascript（innerHtml更新时只能是全量更新）
    
    可维护性：原生Javascript < innerHtml < 虚拟DOM
3. 运行时 和 编译时
    - 运行时：runtime 利用render函数，直接把虚拟DOM转为真实DOM（没有编译过程，无法分析用户提供的内容）
    - 编译时：compiler 直接把template模板中的内容，转化为真实DOM（可以分析用户提供的内容，没有运行时理论上性能会更好）
    - 运行时+编译时
        1. 把template模板转为render函数。
        2. 再利用render函数，把虚拟dom转为真实dom。
    编译时能分析用户提供的内容，运行时提供足够的灵活性。
# 第二章 框架设计的核心要素
# 第三章 Vue.js 3的设计思路
# 第四章 响应系统的作用与实现
1. 副作用函数与响应式数据
- 副作用函数
    会产生副作用的函数
    ```
        let val = 1
        function effect () {
            val = 2
        }
    ```
    这样的函数就可以被称之为副作用函数
- 响应式数据
    会导致视图变化的数据
2. 响应式数据的实现
- 核心逻辑
    getter行为和setter行为
3. 调度系统
- 响应式的可调度性
    当数据更新的动作触发副作用函数重新执行时，有能力决定：副作用函数的执行的时机、次数以及方式
- 实现原理
    1. 异步： Promise
    2. 队列： jobQueue
    基于Set构建队列jobQueue，利用Promise的异步特性，控制执行顺序。
4. 计算属性（computed）
5. 惰性执行（lazy）
6. watch监听器
7. 过期的副作用
- 竞态问题
- 解决方法 onInvalidate
# 第五章 非原始值（对象）的响应式方案
1. Proxy
2. Reflect
# 第六章 原始值（非对象）的响应式方案

# 第七章 渲染器的设计