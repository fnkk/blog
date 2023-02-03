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
##### 数组 Array
- 数组（Array）是solidity常用的一种变量类型，用来存储一组数据（整数，字节，地址等等）。数组分为固定长度数组和可变长度数组两种：
    - 固定长度数组：在声明时指定数组的长度。用T[k]的格式声明，其中T是元素的类型，k是长度，例如：
    ```
    // 固定长度 Array
    uint[8] array1;
    bytes1[5] array2;
    address[100] array3;
    ```
    - 可变长度数组（动态数组）：在声明时不指定数组的长度。用T[]的格式声明，其中T是元素的类型，例如：
    ```
    // 可变长度 Array
    uint[] array4;
    bytes1[] array5;
    address[] array6;
    bytes array7;
    ```
**注意：** bytes比较特殊，是数组，但是不用加[]。另外，不能用byte[]声明单字节数组，可以使用bytes或bytes1[]。在gas上，bytes比bytes1[]便宜。因为bytes1[]在memory中要增加31个字节进行填充，会产生额外的gas。但是在storage中，由于内存紧密打包，不存在字节填充。
- ###### 数组创建过程
在solidity里，创建数组有一些规则：
   - 对于memory修饰的动态数组，可以用new操作符来创建，但是必须声明长度，并且声明后长度不能改变。例子：
```
    // memory动态数组
    uint[] memory array8 = new uint[](5);
    bytes memory array9 = new bytes(9);
```
   - 数组字面常数(Array Literals)是写作表达式形式的数组，用方括号包着来初始化array的一种方式，并且里面每一个元素的type是以第一个元素为准的，例如[1,2,3]里面所有的元素都是uint8类型，因为在solidity中如果一个值没有指定type的话，默认就是最小单位的该type，这里int的默认最小单位类型就是uint8。而[uint(1),2,3]里面的元素都是uint类型，因为第一个元素指定了是uint类型了，我们都以第一个元素为准。 下面的合约中，对于f函数里面的调用，如果我们没有显式对第一个元素进行uint强转的话，是会报错的，因为如上所述我们其实是传入了uint8类型的array，可是g函数需要的却是uint类型的array，就会报错了。
```
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract C {
    function f() public pure {
        g([uint(1), 2, 3]);
    }
    function g(uint[3] memory) public pure {
        // ...
    }
}
```
   - 如果创建的是动态数组，你需要一个一个元素的赋值。
```
    uint[] memory x = new uint[](3);
    x[0] = 1;
    x[1] = 3;
    x[2] = 4;
```
- ###### 数组成员
    - length: 数组有一个包含元素数量的length成员，memory数组的长度在创建后是固定的。
    - push(): 动态数组和bytes拥有push()成员，可以在数组最后添加一个0元素。
    - push(x): 动态数组和bytes拥有push(x)成员，可以在数组最后添加一个x元素。
    - pop(): 动态数组和bytes拥有pop()成员，可以移除数组最后一个元素。
##### 结构体 Struct
- Solidity支持通过构造结构体的形式定义新的类型。创建结构体的方法：
```
    // 结构体
    struct Student{
        uint256 id;
        uint256 score; 
    }
```
#### 映射类型
#### 常数 constant和immutable
