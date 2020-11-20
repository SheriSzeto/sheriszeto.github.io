---
title: React生命周期理解
tags: 
  - react 
  - 生命周期
categories: react
top_img: false  
cover: https://cdn.jsdelivr.net/gh//SheriSzeto/image@main/blog/react-lifecycle.jpg
---

<!-- more -->
<!-- toc -->


### 组件Initialization 初始化阶段

* 类的构造方法（constructor()），继承了react Component基类，也继承react基类，才能有render()，生命周期
  > 注意：函数式组件没有生命周期，也没有state

* super(props)将父组件的props传递给子组件，让子组件读取

* constructor() 初始化组件的状态，如定义this.state的内容

### 组件Mounting 挂载阶段

#### ~~componentWillMount~~（17正式删除）

* 在组件挂载到DOM前调用，且只调用一次，调用this.setState不会引起组件的重新渲染

#### render

* 根据组件的state和props,return一个react元素，由React自身根据此元素去渲染出页面DOM。

* 是纯函数，不能在里面执行this.setState

* 若state和props发生变化，会重新调用render

#### componentDidMount 

* 组件挂载到DOM后调用，且只调用一次

### 组件Updation 更新阶段

setState引起的state更新或父组件重新render引起的props更新，更新后的state和props都将引起子组件的重新render。

造成组件更新有以下情况：
* 父组件重新render
  > 父组件的重新render导致重传的props，子组件将直接跟着重新渲染，无论props是否有变化，可通过shouldComponentUpdate方法优化
  ```javascript
  class Child extends Component {
    shouldComponentUpdate(nextProps) {
      if (nextProps.someThings === this.props.someThings) { // 当重新渲染前后值没发生改变，则子组件不要重新渲染
        return false
      }
    }

    render() {
      return (
        <div>{this.props.someThings}</div>
      )
    }
  }
  ```
  > 在componentWillReceiveProps,将props换成state
  ```javascript
  class Child extends Component {
    constructor(props) {
      super(props);
      this.state = {
        someThings: props.someThings
      }
    }

    componentWillReceiveProps(nextProps) {// 父组件重传props就会调用这个方法, props会判断是否发生变化，变化才会重新setState
      this.setState({someThings: nextProps.someThings})
    }

    render() {
      return (
        <div>{this.props.someThings}</div>
      )
    }
  }
  ```
* 组件本身调用setState，无论state有没有变化，也可通过shouldComponentUpdate方法优化
  > 
  ```javascript
  class Child extends Component {
    constructor(props) {
      super(props);
      this.state = {
        someThings: 1
      }
    }

    shouldComponentUpdate(nextState) {// 当state真的发生变化才重新渲染
      if (nextStatee.someThings === this.state.someThings) {
        return false
      }
    }

    handleClick = () => {
      const preSomeThings = this.state.someThings;
      this.setState({
        someThings: preSomeThings
      })
    }

    render() {
      return (
        <div onClick={this.handleClick}>{this.state.someThing}</div>
      )
    }
  }
  ```
 
#### ~~componentWillReceiveProps~~(17正式删除)

子组件接收到父组件传递来的参数，父组件render函数重新被执行时执行

 - 子组件接收到父组件传递过来的参数，父组件render函数重新被执行，这个生命周期就会被执行
 - 组件第一次存在于DOM，函数不会被执行，如果已经存在于DOM，函数才会被执行


#### <font color=red>（新增）getDerivedStateFromProps(nextProps, prevState)</font>

* 在组件创建时和更新时的render方法之前调用，它应该返回一个对象来更新状态，或者返回null来不更新任何内容。

* 在声明之前要加static关键字，无法访问class的this

#### shouldComponentUpdate(nextProps, nextState)

通过对比props和state是否真的发生变化，返回true则当前组件将继续执行更新过程，返回false则当前组件停止更新

#### ~~componentWillUpdate~~(17正式删除)

组件即将更新

#### render

挂载渲染

#### <font color=red>（新增）getSnapshotBeforeUpdate(prevProps, prevState)</font>

render之后执行，执行时DOM元素还没被更新，用来获取DOM信息，获得snapshot作为componentDidUpdate的第三个参数传入，此声明周期返回的任何值都被作为参数传递给componentDidUpdate()

#### componentDidUpdate

组件更新完成


### 组件unmounting 卸载阶段

#### componentWillUnmount
