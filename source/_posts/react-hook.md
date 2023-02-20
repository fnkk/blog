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
photos: https://cdn.jsdelivr.net/gh/fnkk/resource@0.0.2/img/mayi.jpg
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
1. 把需要父子组件通信的数据使用createContext函数创建
2. 在父组件中使用.Provider包裹子组件，把需要传递的数据通过父组件的value属性传递
3. 在子组件中使用useContext方法获取到value传递的值
### useEffect
- React在完成对DOM的更新后会执行，默认情况下，React会在每次渲染后调用副作用函数，包括第一次渲染的时候
- useEffect有两个参数，第一个参数是副作用的处理函数，第二参数是在与该副作用关联的状态或属性依赖数组
- 第二个参数为空数组时，表示该副作用函数不依赖任何值的变化，只在组件渲染后执行一次
- useEffect相当于Vue生命周期中的mouted，在组件渲染后执行，它的return函数相当于destroy在下次渲染前执行
### useReducer
- useReducer接收3个参数，第一个参数是处理状态更新的reducer，第二个参数是状态初始值，第三个参数是状态初始化函数`const [state, dispathch] = userReducer(reducer, initialArg, init);`
- 指定初始state
    1. 将初始state作为第二个参数传入
    ```
    const [state, dispatch] = useReducer(
        reducer,
        {count: initialCount}
    )
    ``` 
    2. 惰性初始化， 将第二个参数作为第三个函数入参传入
    ```

    ```
### useCallback
-  
### useMemo 
- useMemo传入的函数内部需要有返回值
- useMemo只能声明在函数式组件内部

### react-route
