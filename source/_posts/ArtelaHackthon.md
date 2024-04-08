---
title: ArtelaHackthon
author: Fnkk
avatar: /images/24_01/snake.png
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
comments: true
date: 2024-01-24 15:01:44
tags:
keywords:
description:
photos:
---
# ArtelaHackthon 的一些想法
## 第一次开会讨论的想法
1. Aspect可以查询跨链数据的话，能衍生什么功能
2. 有没有什么方法可以快速的判断用户的画像/属性
## 进一步的想法
- 最开始我只是想做给单个应用能区分用户角色的功能，开会后，元元指出，这样的功能完全可以用前端记录等方式实现。我突然想到Web3区分web2的一个重要理念，数据归用户所有而非中心化机构所有。如下图：
![web2应用](/images/ArtelaHackthon/web2.png)![web3应用](/images/ArtelaHackthon/web3.png)
会员的机制（以QQ音乐为例）变成app读取地址上是否有NFT，而不是与链下记录的会员地址对比。
甚至变成，我购买周杰伦的**烟花易冷**NFT，然后有一个Dapp能读取这个资产并解析，我就可以直接听**烟花易冷**这首歌了。
同样的，文章/视频/游戏资产等 变成用户授权给应用的方式。

## 背景
    1. 受到会员玩法的启发，会员本身相当于用户购买的一种资产，然后将这种资产的价值是更多的服务。
    2. 传统APP的会员似乎是记录在中心化的厂商那里，我们希望这种资产的所有权到用户自己手中。
    3. 衍生：我们希望不只是会员NFT，其他的用户资产如文章、音乐、图片、视频等，所有权回到用户手中。未来的WBE3应用能够成为一个，数据在用户手中，应用获取用户数据授权的方式。
    4. web2中，我们的数字资产都在运营商手中而非自己手里，这让人很不舒服。
## 实现方式
1. 首先要有一个存储个人数据的合约。
    - 个人数据存储合约 可以登记自己的资产，如NTF的合约地址，让aspect能查询到。
    - 个人数据存储合约 可以让用户记录自己的资产，如发布文章、视频等（可以全部上链，或者存IPFS的哈希等方式，而且要满足一定的协议，让数据能被不同的应用获取）。
    - 个人数据存储合约 可以让用户记录自己的资产授权/取消授权 给某Dapp。
2. 具体应用的Dapp：创作者用户授权个人数据存储合约的信息，Dapp进行展示；浏览者可以查看信息。
    - 某DeFi，用户登录后，Aspect检索用户资产中有改DeFi发行的会员NFT。该用户可以有某些更优惠的功能。
    - 某去中心化论坛，绑定Aspect，检索到有用户授权自己的文章给某论坛，改论坛就可以对该文章进行发布，供其他用户阅读。
## 更多可能
1. 基于应用级Dapp上的浏览量，追溯到内容传作者，给予创作激励的新型创作者经济。
2. 构建未来Web3应用生态：在这个生态中，数据属于用户，应用仅在获得用户授权的情况下才能访问这些数据，实现一个真正去中心化的数据管理和服务体验。
3. 很多NFT是一张图片/文字/视频等资源，本身无法得到应用，通过这样授权的方式，可以让用户使用他们购买的图片NTF作为自己头像，音乐NFT作为自己的背景音乐等方式，使NFT拥有更多的使用价值。
4. Aspect能进行跨链的绑定查询信息的功能，能否让数字资产能够跨链的得以应用。


# 项目背景
在当前的数字时代，会员制度常常被视为用户对服务的一种投资，换取更多的服务和便利。然而，这种模式通常存在于中心化的平台中，意味着所有权和控制权仍旧掌握在服务提供者手中。我们的项目灵感来源于将会员制度转变为一种真正属于用户的资产，通过区块链技术，将资产的所有权和控制权交还用户手中。

## 创新点
用户资产所有权回归：不仅限于会员NFT，我们期望将文章、音乐、图片、视频等用户资产的所有权也能回归到用户手中。
构建未来Web3应用生态：在这个生态中，数据属于用户，应用仅在获得用户授权的情况下才能访问这些数据，实现一个真正去中心化的数据管理和服务体验。
# 技术实现
为了实现上述目标，我们设计了以下两个主要的技术组件：

1. 个人数据存储合约（合约1）
资产登记：用户可以通过合约1登记自己的NFT等资产，包括合约地址，使之能被不同应用查询。
资产记录：用户能记录自己的数字资产，例如发布的文章或视频。这些资产可以完整上链或存储其IPFS哈希，同时遵循特定协议以便不同应用访问。
资产授权管理：用户可以通过合约1对自己的资产进行授权或取消授权给特定的Dapp。
2. 应用级Dapp实现
创作者与浏览者交互：创作者将自己的资产信息授权给合约1，Dapp可基于此信息展示内容；浏览者则可以通过Dapp查看这些内容。
## 案例应用：
DeFi平台：用户登陆后，如果检索到用户资产中包含特定DeFi平台发行的会员NFT，则用户可享受更多优惠功能。
去中心化论坛：通过绑定合约1检索到用户授权的文章，论坛可发布这些文章供其他用户阅读。
通过这种方式，我们不仅提升了用户资产的自主性和安全性，也为构建去中心化应用生态奠定了基础。


# Project Background
In today's digital era, membership schemes are often seen as an investment by users for more services and conveniences. However, this model usually exists within centralized platforms, meaning ownership and control remain in the hands of the service providers. Our project is inspired by the transformation of membership schemes into a real asset owned by users. Through blockchain technology, we aim to return the ownership and control of assets back to the users themselves.

## Innovations
Returning Asset Ownership to Users: Beyond membership NFTs, we aim to return the ownership of user assets such as articles, music, photos, and videos back to the users.
Building a Future Web3 Application Ecosystem: In this ecosystem, data belongs to the users. Applications can only access this data with the user's consent, achieving a truly decentralized data management and service experience.
# Technical Implementation
To achieve the above objectives, we have designed the following two main technical components:

1. Personal Data Storage Contract (Contract 1)
Asset Registration: Users can register their assets, including NFTs and contract addresses, through Contract 1, making them queryable by different applications.
Asset Recording: Users can record their digital assets, such as published articles or videos. These assets can be fully on-chain or stored as IPFS hashes, while following specific protocols for access by different applications.
Asset Authorization Management: Users can authorize or revoke authorization of their assets to specific Dapps through Contract 1.
2. Application-Level Dapp Implementation
Creator and Viewer Interaction: Creators authorize their asset information to Contract 1, allowing Dapps to display this information; viewers can access these contents through Dapps.
## Application Examples:
DeFi Platforms: Upon login, if a user's assets include a membership NFT issued by a specific DeFi platform, the user may enjoy additional benefits.
Decentralized Forums: By binding to Contract 1 to retrieve user-authorized articles, forums can publish these articles for other users to read.
Through this approach, we not only enhance the autonomy and security of user assets but also lay the foundation for building a decentralized application ecosystem.