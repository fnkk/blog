---
title: WTF study notes
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-6-29 14:48:52
comments: true
keywords: block chain, solidity
description: 学习solidity的错题集
photos: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
---
## Solidity 101
### 引用类型
- 以下关于数组的说法中，正确的是  c   误选 b
    a. 固定长度数组 和 bytes拥有 push() 成员，可以在数组最后添加一个0元素。
    b. 数组字面常数，例如 [uint(1),2,3]，需要声明第一个元素的类型，不然默认用存储空间最大的类型。
    c. 内存数组的长度在创建后是固定的。
    d. 对于memory可变长度数组，可以用new操作符来创建，并且不用声明长度，例如uint[] memory array = new uint[];。
    - 解析：
        a. 动态数组和bytes 拥有push()
        b. 默认用存储空间最小的类型
        d. new关键字创建需要声明长度
-  有如下一段合约代码，执行 initStudent 方法后，student.id 和 student.score 的值分别为
        contract StructTypes {
            struct Student{
                uint256 id;
                uint256 score;
            }
           Student student;
           function initStudent() external{
                student.id = 100;
                student.score = 200;
                Student storage _student = student;
                _student.id = 300;
                _student.score = 400;
            }
        }
    a. 300 400
    b. 100 200
    c. 300 200
    d. 100 400
    - 解析：
        正确答案a，创建新的Student _student是引用类型，地址指向不变，会改变指向地址内部的值。
### 映射类型
- Mapping的存储位置可以是？
    只能是storage
- 给映射变量map新增键值对的方法？
    map[_Key] = _Value;
### 变量初始化
- 字节的默认值是 0x00
### 常数 constant和immutable
- constant（常量）和immutable（不变量）。状态变量声明这个两个关键字之后，不能在合约后更改数值；并且还可以节省gas。另外，只有数值变量可以声明constant和immutable；string和bytes可以声明为constant，但不能为immutable。
### 继承
#### 多重继承
solidity的合约可以继承多个合约。规则：

继承时要按辈分最高到最低的顺序排。比如我们写一个Erzi合约，继承Yeye合约和Baba合约，那么就要写成contract Erzi is Yeye, Baba，而不能写成contract Erzi is Baba, Yeye，不然就会报错。 如果某一个函数在多个继承的合约里都存在，比如例子中的hip()和pop()，在子合约里必须重写，不然会报错。 重写在多个父合约中都重名的函数时，override关键字后面要加上所有父合约名字，例如override(Yeye, Baba)。 例子：
        ```
        contract Erzi is Yeye, Baba{
            // 继承两个function: hip()和pop()，输出值为Erzi。
            function hip() public virtual override(Yeye, Baba){
                emit Log("Erzi");
            }

            function pop() public virtual override(Yeye, Baba) {
                emit Log("Erzi");
            }
        }
        ```
我们可以看到，Erzi合约里面重写了hip()和pop()两个函数，将输出改为”Erzi”，并且还分别从Yeye和Baba合约继承了yeye()和baba()两个函数。
### 抽象合约和接口
- 接口的定义
    1. 不能包含状态变量
    2. 不能包含构造函数
    3. 不能继承除接口外的其他合约
    4. 所有函数都必须是external且不能有函数体
    5. 继承接口的合约必须实现接口定义的所有功能
### 接收ETH
#### 接收ETH函数receive
- receive()只用于处理接收ETH。一个合约最多有一个receive()函数，声明方式与一般函数不一样，不需要function关键字：receive() external payable { ... }。receive()函数不能有任何的参数，不能返回任何值，**必须**包含external和payable。
#### 回退函数fallback
- fallback()函数会在调用合约不存在的函数时被触发。可用于接收ETH，也可以用于代理合约proxy contract。fallback()声明时不需要function关键字，必须由external修饰，**一般**也会用payable修饰，用于接收ETH:fallback() external payable { ... }。
### 发送ETH
- transfer
    目标地址.transfer(金额)
    gas限制是2300
    转账失败会自动回滚
    没有返回值
- send
    目标地址.send(金额)
    gas限制2300
    转账失败不会revert
    send()的返回值是一个bool，代表转账成功或失败
- call
    目标地址.call(value:金额)
    没有gas限制
    转账失败不会revert
    call()的返回值是(bool,data)，bool代表转账成功或失败
