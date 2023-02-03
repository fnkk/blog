---
title: solidity advanced study notes
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-2-3 13:35:02
comments: true
keywords: solidity, WTF
description: solidity进阶学习笔记
photos: https://static.2heng.xin/wp-content/uploads//2019/02/wallhaven-672007-1-1024x576.png
---
## 接收ETH
- Solidity支持两种特殊的回调函数，receive()和fallback()，他们主要在两种情况下被使用：
    1. 接收ETH
    2. 处理合约中不存在的函数调用（代理合约proxy contract）
    **注意：**在solidity 0.6.x版本之前，语法上只有 fallback() 函数，用来接收用户发送的ETH时调用以及在被调用函数签名没有匹配到时，来调用。 0.6版本之后，solidity才将 fallback() 函数拆分成 receive() 和 fallback() 两个函数。
### 接收ETH函数 receive
- receive()只用于处理接收ETH。一个合约最多有一个receive()函数，声明方式与一般函数不一样，不需要function关键字：receive() external payable { ... }。receive()函数不能有任何的参数，不能返回任何值，必须包含external和payable。

当合约接收ETH的时候，receive()会被触发。receive()最好不要执行太多的逻辑因为如果别人用send和transfer方法发送ETH的话，gas会限制在2300，receive()太复杂可能会触发Out of Gas报错；如果用call就可以自定义gas执行更复杂的逻辑（这三种发送ETH的方法我们之后会讲到）。

我们可以在receive()里发送一个event，例如：
```
    // 定义事件
    event Received(address Sender, uint Value);
    // 接收ETH时释放Received事件
    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
```
有些恶意合约，会在receive() 函数（老版本的话，就是 fallback() 函数）嵌入恶意消耗gas的内容或者使得执行故意失败的代码，导致一些包含退款和转账逻辑的合约不能正常工作，因此写包含退款等逻辑的合约时候，一定要注意这种情况。
### 回退函数 fallback
- fallback()函数会在调用合约不存在的函数时被触发。可用于接收ETH，也可以用于代理合约proxy contract。fallback()声明时不需要function关键字，必须由external修饰，一般也会用payable修饰，用于接收ETH:fallback() external payable { ... }。

我们定义一个fallback()函数，被触发时候会释放fallbackCalled事件，并输出msg.sender，msg.value和msg.data:
```
    // fallback
    fallback() external payable{
        emit fallbackCalled(msg.sender, msg.value, msg.data);
    }
```
## 发送ETH
### transfer
- 用法是接收方地址.transfer(发送ETH数额)。
- transfer()的gas限制是2300，足够用于转账，但对方合约的fallback()或receive()函数不能实现太复杂的逻辑。
- transfer()如果转账失败，会自动revert（回滚交易）。
代码样例，注意里面的_to填ReceiveETH合约的地址，amount是ETH转账金额：
```
// 用transfer()发送ETH
function transferETH(address payable _to, uint256 amount) external payable{
    _to.transfer(amount);
}
```
### sender
- 用法是接收方地址.send(发送ETH数额)。
- send()的gas限制是2300，足够用于转账，但对方合约的fallback()或receive()函数不能实现太复杂的逻辑。
- send()如果转账失败，不会revert。
- send()的返回值是bool，代表着转账成功或失败，需要额外代码处理一下。
代码样例：
```
// send()发送ETH
function sendETH(address payable _to, uint256 amount) external payable{
    // 处理下send的返回值，如果失败，revert交易并发送error
    bool success = _to.send(amount);
    if(!success){
        revert SendFailed();
    }
}
```
### call
- 用法是接收方地址.call{value: 发送ETH数额}("")。
- call()没有gas限制，可以支持对方合约fallback()或receive()函数实现复杂逻辑。
- call()如果转账失败，不会revert。
- call()的返回值是(bool, data)，其中bool代表着转账成功或失败，需要额外代码处理一下。
代码样例：
```
// call()发送ETH
function callETH(address payable _to, uint256 amount) external payable{
    // 处理下call的返回值，如果失败，revert交易并发送error
    (bool success,) = _to.call{value: amount}("");
    if(!success){
        revert CallFailed();
    }
}
```
### 总结
- call没有gas限制，最为灵活，是最提倡的方法；
- transfer有2300 gas限制，但是发送失败会自动revert交易，是次优选择；
- send有2300 gas限制，而且发送失败不会自动revert交易，几乎没有人用它。
