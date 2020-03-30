---
title: 前端学习思路
date: 2020-03-24 14:49:02
tags:
    - React
    - 思路
    - Vue
categories: 思路   
---

![](https://blog-1257457106.cos.ap-chengdu.myqcloud.com/mind.png)

<!-- more -->
## React

### 生命周期

#### constructor

初始化state

#### getDerivedStateFromProps

静态函数，可以在

#### render

将组件的状态属性挂载到DOM上

#### componentDidMount

状态属性有更新，或者异步请求数据处理在这个周期调用

#### getDerivedStateFromProps

#### shouldComponentUpdate

#### getSnatshotBeforeUpdate

#### componentDidUpdate

#### componentWillUnmout

卸载组件

### 虚拟DOM

本质是一个JavaScript对象，当页面的状态属性值发生变化时，会通过diff比较新树与旧树的差异，最后把差异部分重新渲染到真正的DOM上

#### 优点：更新更快，操作DOM简单，减少内存消耗

### refs

#### 1、在render函数中可以访问react 元素或者DOM节点的方法

#### 2、可以访问到子组件元素，强制修改子组件

#### 3、React.createRef()

### 受控组件与非受控组件

#### 受控组件，通过绑定state定义的属性值，调用onChange事件后，重新setState，视图更新

#### 非受控组件，只能通过操作真实DOM获取该元素上的值，性能损耗

### setState是异步还是同步？

#### 在合成事件和钩子函数中是异步的

#### 在原生事件和setTimeout是同步的

### redux（状态管理）

#### store，一个存储空间

#### state，所有状态数据集合

#### action，View通过dispatch发布action，通知修改state

#### reducer，接收action和当前state作为参数，计算完后返回新的state

### 高阶组件

#### 在一个组件中返回另一个组件函数

### 无状态组件

### 组件之间的通信方式

## Vue

### 生命周期

#### beforeCreate

#### created

#### beforeMount

#### mounted

#### beforeUpdate

#### update

#### beforeDestroy

#### destroyed

### MVVM模型

### 双向绑定原理（2.0和3.0的区别）

#### 2.0  初始化数据时，会使用Object.defineProperty重新定义data里面的属性，当页面使用对应属性，会先进行依赖收集（收集当前的watcher）,如果属性发生变化则通知相关依赖进行更新操作（发布订阅）

#### 3.0 由Proxy替代Object.defineProperty，可以直接监听到对象和数组的变化，并且有多达13种拦截方法。

### nextTick

#### 在下次DOM更新循环结束之后执行延迟回调

### 组件之间的通信方式

#### 父组件通过props传递，子组件通过$emit向上级抛

#### 兄弟组件可创建event bus

#### vuex状态值

#### ref 获取实例的方式调用组件的属性和方法

### vuex

#### state

#### action

#### mutation

## 性能优化

## 移动端

## 打包工具

### webpack

### gulp

