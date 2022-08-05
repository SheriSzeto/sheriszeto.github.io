---
title: react hooks
urlname: 2db63c359e50ee762c38ec54db55122c
date: '2021-04-15 10:43:18 +0800'
tags: []
categories: []
---

`背后是类组件和函数组件的认知区别`
`函数组件会捕获 render 内部的状态，这是两类组件最大的不同。`

####

#### 1、useState

```
// 基础用法
const [state, setState] = useState(initialState);
```

---

注意：

```
if(showSex){
	const [ sex , setSex ] = useState('男')
	showSex=false
}
上面写法会报错，原因是：React Hooks不能出现在条件判断语句中，因为它必须有完全一样的渲染顺序。
```

####

#### 2、useEffect 允许函数组件执行副作用操作

`useEffect代替生命周期`

```
// 基本用法
// @params callBack 回调函数 [] 依赖函数
useEffect(callBack, [])
```

#####

##### 每一次渲染都执行

```
useEffect(callBack)
```

#####

##### 仅在挂载阶段执行一次

```
useEffect(callBack, [])
```

#####

##### 仅在挂载和卸载阶段执行一次

```
useEffect(( ) => {
	return ( ) => { }
}, [ ])
```

！！！面试题 为什么需要 React-Hooks

- 告别难以理解的 Class；
- 解决业务逻辑难以拆分的问题；
- 使状态逻辑复用变得简单可行；
- 函数组件从设计思想上来看，更加契合 React 的理念。

###

### [useEffect 两个注意点]

1. React 首次渲染和之后的每次渲染都会调用一遍 useEffect 函数，而之前我们要用两个生命周期函数分别表示首次渲染(componentDidMonut)和更新导致的重新渲染(componentDidUpdate)。
1. useEffect 中定义的函数的执行不会阻碍浏览器更新视图，也就是说这些函数时异步执行的，而 componentDidMonut 和 componentDidUpdate 中的代码都是同步执行的。个人认为这个有好处也有坏处吧，比如我们要根据页面的大小，然后绘制当前弹出窗口的大小，如果时异步的就不好操作了。
1. useContext 实现父子元素通信
1. useReducer

useContext：可访问全局状态，避免一层层的传递状态。这符合 Redux 其中的一项规则，就是状态全局化，并能统一管理。 useReducer：通过 action 的传递，更新复杂逻辑的状态，主要是可以实现类似 Redux 中的 Reducer 部分，实现业务逻辑的可行性。
