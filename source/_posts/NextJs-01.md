---
title: NextJs-01
author: Fnkk
avatar: /images/24_01/snake.png
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
comments: true
date: 2024-01-18 16:43:25
tags:
keywords:
description:
photos:
---
# 前言
之前学Next都是边用边学，现在打算重新系统的学习一遍，从读NextJs的官方文档开始
## CSS styling
### Global styles
```
import '@/app/ui/global.css';
```
直接引入全局样式，一般应用于顶部组件。
### Tailwind
一种CSS框架，直接在className里用Tailwind的规则直接写样式，这样写出来的标签好长，可能是没怎么用过，感觉看着好累，不喜欢。
```
<h1 className="text-blue-500">I'm blue!</h1>
```
### CSS Modules
将css文件命名为**xxx.module.css**的格式，在用如下方式引入使用。
```
import styles from '@/app/ui/home.module.css';
 
<div className={styles.shape}>
```
### clsx
可以用于切换ClassName来条件渲染样式，平常直接用一些判断语句来做这样的条件渲染，似乎这个功能不是很必须。
下面是一个列子：
```
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
```
## Optimizing Fonts and Images
- next/font
- next/image