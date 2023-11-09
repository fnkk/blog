---
title: 杭州黑客松类friend.tech系统的搭建
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-10-14 14:26:01
comments: true
keywords: block chain
description: 设计思路
photos: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
---
# 类friend.tech系统设计的思路
## 铸造NFT(nft信息可能还需要铸造时间、作者等信息)
- 项目地址
- 简介

## 首页
- 所有项目的列表
    1. 包括字段：项目地址、简介、长度（发行了多少个NFT）、资金池、项目方总收益
- 购买相关的项目NFT
- ？相关的一些图表
## 个人中心
- 我购买的所有项目相关信息
    1. 我的位次
    2. 我的收益（包括回本预期，还需要多少位人入场）
    3. 操作（怒退）
- 怒退按钮（实现怒退功能）

## 发布项目
- 发布页面
    1. 提交表单：需要项目地址、简介。


# 开发细节
## 确定项目
### 前端
- day0
1. 确定使用的技术栈：react
2. 搭建页面的layout和静态页面的编写
3. 使用etherjs连接钱包
- day1
1. 使用etherjs连接部署至Scroll的合约
2. 调用合约的接口（initToken，mint等）完成相应的功能
3. 读取合约信息完善页面
### 后端

