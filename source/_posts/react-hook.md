---
title: react hook learning notes
author: Fnkk
avatar: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
authorLink: fnkk.io
authorAbout: Fnkk
authorDesc: Fnkk
categories: 技术
date: 2023-2-1 09:38:02
comments: true
keywords: react, hook
description: react hook学习笔记
photos: https://static.2heng.xin/wp-content/uploads//2019/02/wallhaven-672007-1-1024x576.png
---
## react hook学习笔记
### useState
- 需要使用变量时，通过解构的方式使用useState()，参数为变量的初始值`const [productId, setProductId] = useState();`
- setProductId(更新数据)的两种方式
    1. 直接调用函数传入新值，如`setProductId(newVal)`
    2. 使用函数式更新，如`setProductId(()=>{return newVal})`
    3. 如果定义的是引用类型变量如`const [teacher, setTeacher] = useState({name:'Ryan',sex:'man'})`,更新时可以使用函数式更新并借助拓展运算符，如`setTeacher(()=>{return {...teacher,name:'Chris'}})`，把新的属性合并到新的对象中
### useState惰性初始化
- 惰性state
    initialState 参数只会在组件的初始渲染中起作用，后续渲染会被忽略
- 复杂初始state的定义
    初始state需要通过复杂计算获得，则可以传入一个函数，在函数中计算并返回初始的 state
    **如果更新的数据与上次一样，React 将跳过子组件的渲染及 effect 的执行**
### useContext
- 
