---
title: ff数字藏品交易系统的搭建
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-3-31 15:51:01
comments: true
keywords: block chain
description: 设计思路
photos: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
---
# 毕业设计系统设计的思路
## 铸造NFT(nft信息可能还需要铸造时间、作者等信息)
- WTF的铸造NFT合约的铸造函数mint() 只需要两个参数，tokenId和生成后NFT的地址。这显然是不符合我的需求的，需要改造。
1. 生成NFT的tokenId自动生成，自增，容量上限为10000个。
2. 生成NFT后的默认地址为使用的用户的地址
3. 生成NFT需要一些额外的参数：图片信息（最重要的信息，应该是一个IPFS的地址哈希值），一些描述（用于展示NFT时描述，如：作者、名称等，不作为生成的条件）。
#### 修改ERC721.sol 思路(已完成)
1. tokenId 数据类型从uint改为string,从_mint函数的入参获得变为 图片地址哈希+nft名称+简介(或者简介存入ipfs的哈希) 通过sha256生成的值,不在设最大token数量(或者添加一个tokenNumber记录当前nft数量,并定义最大数量)。
5. tokenId 的数据类型只能是uint 所以tokenId改为数字自增,
2. _mint函数入参改变为 用户地址+图片哈希+name+简介 通过computedTokenId()函数获取tokenId的值,后续逻辑不变
3. 定义一个mapping(tokenId => tokenDetail),tokenDetail为一个struct,包含tokenId,picUrl,introduction,name四个字段。
4. 定义一个getTokenDetail函数 输入tokenId获取tokenDetail的内容

## 追溯NFT
- 查询交易记录的功能
1. 输入tokenID查询这个NFT的所有被交易信息。
2. 输入一个账号地址，查询该账号的所有交易记录。
3. 需要利用事件存储交易信息到数据库,再在数据库中操作查询交易记录(node.js和mongoDb的操作,数据库如何设计,是否有必要记录其他更多的数据,实现完整的链下生态)
4. planB: 前端直接通过事件拿到所有信息,然后在前端进行数据处理展示,这样可以跳过node和mongo,但是逻辑可能也很复杂。

## 首页
- 一个走马灯,一个背景图,然后其他几个页面跳跳算了。

## 个人中心
- 查看本人持有的所有NFT(已完成)
- 本人所有交易记录,功能上隶属于追溯(不一定做,大概率放弃)
- 每个nft有两个按钮,分别是转赠和拍卖,分别跳转至转赠和新增拍卖页面。

## 数字博物馆
- 展示所有NFT作品，无限滚动、懒加载
- 还是采用分页的方式,简单一点,重写分页的方法即可。
- 和个人列表一致,采用card的形式展示,点击查看NFT作品详情。多一个该nft所有的交易记录信息,作者,持有人,创建时间

## 数字藏品拍卖
- 用户可以提交自己持有的NFT进行拍卖
- 拍卖所展示被拍卖的NFT 分类分列：待开始、进行中、已结束。
- 拍卖流程同e-bay

## 数据库设计
- nft表
tokenId name introduction picUrl createdTime author owner transferSum
查询owner获取个人所有nft
查询所有信息获取所有nft
- 交易记录表
tokenId from to transferTime
查询tokenId返回所有记录就是追溯的结果。
- 拍卖表
有点多晚点再说
- 合约事件设计
只要一个新增NFT的事件即可,transfer事件既新增交易记录表,又更新nft表

# 毕业论文大纲
- 目录
0. 摘要
1. 绪论
    - 研究背景及意义
    - 国内外研究现状
        1. 区块链现状
        2. 数字藏品现状
    - 本文主要工作
2. 相关理论技术介绍
    - 区块链技术概述
    - node.js和mongodb概述
    - IPFS概述
    - react和web3.js概述
3. 数字藏品交易系统的分析与设计

4. 数字藏品交易系统的实现