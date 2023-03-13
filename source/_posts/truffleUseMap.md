---
title: truffleUseMap
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/honjun/cdn@1.6/img/custom/avatar.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-1-30 09:38:01
comments: true
tags: 
 - web
keywords: truffle
description: truffle使用笔记
photos: https://static.2heng.xin/wp-content/uploads//2019/02/wallhaven-672007-1-1024x576.png
---
## Truffle使用笔记

### geth相关
#### geth常用命令
- geth启动命令 
    `geth --datadir data --networkid 666 --http --http.port 8545 --allow-insecure-unlock console 2>output.log console`
- geth启动命令(开启WS服务,默认端口8456) 
    `geth --datadir data --networkid 666 --ws --http --http.port 8545 --allow-insecure-unlock console 2>output.log console`
- geth解锁账号 
    `personal.unlockAccount('account')` 然后输入密码 1234
- 定义一定金额 单位Wei 
这里的数字采用字符串的形式输入，避免精度丢失
    `amt_1 = web3.utils.toWei('1', 'ether');`
- geth转账ETH
    `eth.sendTransaction({from:a,to:b,value:web3.toWei(20,"ether")})`
- wei转ether
    `web3.fromWei(numer,'ether')`
- data 文件夹大小 5.35/5.44MB  5.10/7.62MB    8.76/8.82MB

### truffle相关
#### truffle部署测试流程
1. 编译好合约后，使用命令truffle migration或者truffle depoly迁移（部署）合约，需要启动geth客户端，同时解锁账户，开始挖矿
2. 部署完成后，使用命令turflle console进入js 控制台，控制台中有migrations里定义的对象，可以调用对象的deploy方法的回调测试合约的函数，如
`EcommerceStore.deployed().then(function(i) {i.getProduct.call(1).then(function(f) {console.log(f)})})`
    
### solidity相关
- 高版本的solidity中，强制要求所有string类型后面加上memory
- now关键字已废用，采用block.timestamp代替
- 需要transfer的地址要加上payable关键字
-  keccak256用法`result =  keccak256(abi.encodePacked(value, fake, secret))`


### truffle-box-react相关
- 合约交互代码写在ContractBtns组件中
- Take a look at client/src/contexts/EthContext. This context maintains a global state and provides web3.js functionalities to the rest of the app.


### node + mongo 的链下生态
-  第二个视频 19分 定义mongoose相关
- 

### 研究方法
1. 基于ERC-721的非同质化货币铸造的研究。
数字藏品交易系统中的数字藏品就是一个非同质化货币，ERC721是一种以太坊代币标准，它是一种非同质化代币智能合约标准接口。因此，只要重写该接口的方法，就能初步完成铸造，使数字藏品获得唯一的标识符(token ID)，和确权交易等功能，可以在此基础上完成盲盒、空投、拍卖等其他复杂功能。
2. 基于geth客户端的区块链环境。
geth是以太坊客户端之一，通过运行geth节点，用户可以访问以太坊网络，并可以进行交易、挖矿、部署智能合约等操作。通过geth客户端在本地生成一条以太坊私链，用于部署智能合约。geth还提供了一些常用的命令行工具，如管理账户、查询交易历史、查看节点同步状态等。
3. 基于IFPS的资源去中心化存储。
由于数字藏品交易系统有大量的图片资源，这些占用大量存储空间的资源直接存入区块链的话效率低且消耗大量gas。IFPS是一个点对点的分布式文件系统，它的设计理念和技术架构对于实现分布式的、去中心化的应用程序非常有帮助。我们将这些占用大量存储空间的资源上传到IFPS中，它会返回一个哈希，我们只将这个哈希存到区块链中，在前端页面中通过这个哈希去找到对应的资源。