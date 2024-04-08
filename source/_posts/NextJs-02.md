---
title: NextJs-02
author: Fnkk
avatar: /images/24_01/snake.png
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
comments: true
date: 2024-01-23 11:07:57
tags:
keywords:
description:
photos:
---
# 前言
之前学Next都是边用边学，现在打算重新系统的学习一遍，从读NextJs的官方文档开始
## Chapter 4: Creating Layouts and Pages
```
import SideNav from '@/app/ui/dashboard/sidenav';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
```
这是layout的示例代码，在app文件夹下，创建文件会自动生成与文件名路径相对应的路由路径（根文件名为page.tsx），子路由文件作为children参数传入，layout切换时不会重新渲染。
## Chapter 5: Navigating Between Pages