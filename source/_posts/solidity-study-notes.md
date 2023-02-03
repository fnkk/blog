---
title: solidity study notes
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-2-1 17:59:02
comments: true
keywords: solidity, WTF
description: solidity学习笔记
photos: https://static.2heng.xin/wp-content/uploads//2019/02/wallhaven-672007-1-1024x576.png
---
## solidit 入门
### 数值类型
#### Solidity中的变量类型
- 数值类型(Value Type)：包括布尔型，整数型等等，这类变量赋值时候直接传递数值。
- 引用类型(Reference Type)：包括数组和结构体，这类变量占空间大，赋值时候直接传递地址（类似指针）。
- 映射类型(Mapping Type): Solidity里的哈希表。
- 函数类型(Function Type)：Solidity文档里把函数归到数值类型，但我觉得他跟其他类型差别很大，所以单独分一类。
#### 数值类型
1. 布尔型
取值为true和false
布尔值的运算符和js相似
2. 整型
常用包括
``
// 整型
    int public _int = -1; // 整数，包括负数
    uint public _uint = 1; // 正整数
    uint256 public _number = 20220330; // 256位正整数
``
3. 地址类型
地址类型(address)存储一个 20 字节的值（以太坊地址的大小）。地址类型也有成员变量，并作为所有合约的基础。有普通的地址和可以转账ETH的地址（payable）。其中，payable修饰的地址相对普通地址多了transfer和send两个成员。在payable修饰的地址中，send执行失败不会影响当前合约的执行（但是返回false值，需要开发人员检查send返回值）。balance和transfer()，可以用来查询ETH余额以及安全转账（内置执行失败的处理）。
``
    // 地址
    address public _address = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71;
    address payable public _address1 = payable(_address); // payable address，可以转账、查余额
    // 地址类型的成员
    uint256 public balance = _address1.balance; // balance of address
``
4. 定长字节数组
字节数组bytes分两种，一种定长（byte, bytes8, bytes32），另一种不定长。定长的属于数值类型，不定长的是引用类型（之后讲）。 定长bytes可以存一些数据，消耗gas比较少。
``
    // 固定长度的字节数组
    bytes32 public _byte32 = "MiniSolidity"; 
    bytes1 public _byte = _byte32[0]; 
``
MiniSolidity变量以字节的方式存储进变量_byte32，转换成16进制为：0x4d696e69536f6c69646974790000000000000000000000000000000000000000

_byte变量存储_byte32的第一个字节，为0x4d。
5. 枚举 enum
枚举（enum）是solidity中用户定义的数据类型。它主要用于为uint分配名称，使程序易于阅读和维护。它与C语言中的enum类似，使用名称来代替从0开始的uint：
``
    // 用enum将uint 0， 1， 2表示为Buy, Hold, Sell
    enum ActionSet { Buy, Hold, Sell }
    // 创建enum变量 action
    ActionSet action = ActionSet.Buy;
``
它可以显式的和uint相互转换，并会检查转换的正整数是否在枚举的长度内，不然会报错：
``
    // enum可以和uint显式的转换
    function enumToUint() external view returns(uint){
        return uint(action);
    }
``
#### 函数类型
1. function：声明函数时的固定用法，想写函数，就要以function关键字开头。

2. <function name>：函数名。

3. (<parameter types>)：圆括号里写函数的参数，也就是要输入到函数的变量类型和名字。

4. {internal|external|public|private}：函数可见性说明符，一共4种。没标明函数类型的，默认public。合约之外的函数，即"自由函数"，始终具有隐含internal可见性。

    - public: 内部外部均可见。
    - private: 只能从本合约内部访问，继承的合约也不能用。
    - external: 只能从合约外部访问（但是可以用this.f()来调用，f是函数名）。
    - internal: 只能从合约内部访问，继承的合约可以用。
    **Note 1:** 没有标明可见性类型的函数，默认为public。

    **Note 2:** public|private|internal 也可用于修饰状态变量。 public变量会自动生成同名的getter函数，用于查询数值。

    **Note 3:** 没有标明可见性类型的状态变量，默认为internal。

5. [pure|view|payable]：决定函数权限/功能的关键字。payable（可支付的）很好理解，带着它的函数，运行的时候可以给合约转入ETH。pure和view的介绍见下一节。

6. [returns ()]：函数返回的变量类型和名称。
#### 函数输出
#### 变量存储和作用域
#### 数据位置
- solidity数据存储位置有三类：storage，memory和calldata。不同存储位置的gas成本不同。storage类型的数据存在链上，类似计算机的硬盘，消耗gas多；memory和calldata类型的临时存在内存里，消耗gas少。大致用法：
    1. storage：合约里的状态变量默认都是storage，存储在链上。
    2. memory：函数里的参数和临时变量一般用memory，存储在内存中，不上链。
    3. calldata：和memory类似，存储在内存中，不上链。与memory的不同点在于calldata变量不能修改（immutable），一般用于函数的参数。
#### 引用类型

#### 映射类型
