---
title: javascript设计模式以及运用
date: 2020-11-20 14:55:49
tags:
    - javascript
    - 设计模式
categories: javascript
top_img: false 
cover: https://cdn.jsdelivr.net/gh//SheriSzeto/image@main/blog/design-pattern.png
---

<!-- toc -->

# 创建型
## 1.抽象工厂模式
## 2.工厂方法模式

``` javascript
// 工厂方法模式
// 工厂方法模式关键核心代码就是工厂里面的判断this是否属于工厂，也就是做了分支判断，这个工厂只做我能生产的产品，如果你的产品我目前做不了，请找其他工厂代加工；
function factory(role) {
  if (this instanceof factory) {
    var a = new this[role]
    return a
  } else {
    return new factory(role)
  }
}

factory.prototype = {
  "superAdmin": function () {
    this.name = "超级管理员";
    this.viewPage = ["首页", "发现页", "通讯录", "应用数据", "权限管理"];
  },
  "admin": function () {
    this.name = "管理员";
    this.viewPage = ["首页", "发现页", "通讯录", "应用数据"];
  },
  "user": function () {
    this.name = "普通用户";
    this.viewPage = ["首页", "发现页", "通讯录"];
  }
}

let superAdmin = factory("superAdmin");
console.log(superAdmin);
let admin = factory("admin");
console.log(admin);
let user = factory("user");
console.log(user);
```

## 3.建造者模式

``` javascript
// 建造者模式
// 定义：将一个复杂的对象分解成多个简单的对象来进行构建，将复杂的构建层与表现层分离，使相同的构建过程可以创建不同的表示模式；
// 模式作用：1.分步创建一个复杂的对象 2.解耦封装过程和具体创建组件 3.无需关心组件如何组装
/*
　　某土豪想建一个房子，某土豪只需要找包工头，包工头再去找施工团队来建造房子，而不需要土豪自己去一个个的找工人搭建施工团队开始施工；包工头知道土豪的需求，也知道哪里能找到工人搭建施工团队，工人可以直接干活，中间节省了土豪直接和工人沟通的成本；土豪不需要知道房子该怎么建，土豪只需要最后能验收到房就行；

在写代码之前我们先分析一下：

    1、产出的东西是房子

    2、包工头调用工人进行开工 而且他要很清楚工人们具体的某一个大项

    3、工人是盖房子的 工人可以建卧室  建客厅 建厨房

    4、包工头只是一个接口，他只对外说盖房子，他不用做事情；
*/
function Fangzi() {
  this.woshi = '';
  this.keting = '';
  this.chufang = ''
}

function Baogongtou() {
  this.jianfangzi = function(gongren) {
    gongren.jian_woshi();
    gongren.jian_keting();
    gongren.jian_chufang();
  }
}

function Gongren() {
  this.jian_woshi = function() {
    console.log('卧室建好了');
  }
  this.jian_keting = function() {
    console.log('客厅建好了');
  }
  this.jian_chufang = function() {
    console.log('厨房建好了');
  }
  this.wangong = function() {
    var fangzi = new Fangzi()
    fangzi.woshi = 'ok';
    fangzi.keting = 'ok';
    fangzi.chufang = 'ok';
    return fangzi;
  }
}

let gongren = new Gongren()
let baogongtou = new Baogongtou()
baogongtou.jianfangzi(gongren)
var my_fangzi = gongren.wangong()
console.log(my_fangzi);
```

## 4.原型模式

``` javascript
// 原型模式：原型实例指向创建对象的种类，并通过拷贝这些原型创建新的对象，是一种用来创建对象的模式，也就是创建一个对象作为另一个对象的prototype属性；
var vehiclePrototype = {
  init: function (carModel) {
      this.model = carModel || "保时捷";
  },
  getModel: function () {
      console.log('车辆模具是：' + this.model);
  }

};

function vehicle(model) {
  function F() { };
  F.prototype = vehiclePrototype;    
  var f = new F();
  f.init(model);
  return f;
}
var car = vehicle('法拉利');
car.getModel();  // 车辆模具是：法拉利
```
## 5.单例模式

``` javascript
// 单例模式
let SingleTon = (function(){
  let instance = null
  return function(name) {
    this.name = name
    if (instance) {
      return instance
    }
    return instance = this
  }
})()
SingleTon.prototype.getName = function() {
  console.log(this.name);
}

let winner = SingleTon.getInstance('winner')
console.log(winner.getName()); // winner
let sunner = SingleTon.getInstance('sunner');
console.log(sunner.getName()); // winner
```

``` javascript
// 惰性单例模式
// 场景：页面多次调用都有弹窗提示，只是提示内容不一样；
let getSingleton = function(fn) {
    var result;
    return function() {
        return result || (result = fn.apply(this, arguments)); // 确定this上下文并传递参数
    }
}
let createAlertMessage = function(html) {
    var div = document.createElement('div');
    div.innerHTML = html;
    div.style.display = 'none';
    document.body.appendChild(div);
    return div;
}

let createSingleAlertMessage = getSingleton(createAlertMessage);
    document.getElementById('loginBtn').onclick=function(){
        let alertMessage = createSingleAlertMessage('看来真的是个咸鱼');
        alertMessage.style.display = 'block';
}
```

> 🔔🔔🔔 常规开发常用的是简单工厂和工厂方法模式


简单工厂模式示例：

``` javascript
// 简单工厂模式
function factory(role) {
  function User(opt) {
    this.name = opt.name;
    this.viewPage = opt.viewPage;
  }

  switch(role) {
    case 'superAdmin':
      return new User({name: 'superAdmin', viewPage: ['首页', '发现页', '通讯录', '应用数据', '权限管理']})
      break
    case 'admin':
      return new User({name: 'admin', viewPage: ['首页', '发现页', '通讯录', '应用数据']})
      break
    case 'user':
      return new User({name: 'user', viewPage: ['首页', '发现页', '通讯录']})
      break    
  }
}

let superAdmin = factory('superAdmin')
console.log(superAdmin.name);
let admin = factory('admin')
console.log(admin);
let user  = factory('user')
console.log(user);
```

> 大白话解释：简单工厂模式就是你给工厂什么，工厂就给你生产什么；

> 工厂方法模式就是你找工厂生产产品，工厂是外包给下级分工厂来代加工，需要先评估一下能不能代加工；能做就接，不能做就找其他工厂；

> 抽象工厂模式就是工厂接了某项产品订单但是做不了，上级集团公司新建一个工厂来专门代加工某项产品；

----

# 结构型
## 1.适配器模式
```javascript
// 适配器用来解决两个已有接口之间不匹配的问题，它并不需要考虑接口是如何实现，也不用考虑将来该如何修改；适配器不需要修改已有接口，就可以使他们协同工作；
var googleMap = {
  show: function(){
      console.log( '开始渲染谷歌地图' );
  }
};
var baiduMap = {
  display: function(){
      console.log( '开始渲染百度地图' );
  }
};
var baiduMapAdapter = {
  show: function(){
      return baiduMap.display();

  }
};

renderMap( googleMap ); // 输出：开始渲染谷歌地图
renderMap( baiduMapAdapter ); // 输出：开始渲染百度地图
```
## 2.桥接模式
## 3.组合模式
## 4.装饰者模式
## 5.外观模式
## 6.享元模式
## 7.代理模式

```javascript
// 代理模式：为一个对象提供一个代用品或占位符，以便控制对它的访问！
/*
场景：你作为一个追星狂魔，是某明星的忠诚粉丝；刚好某明星近期要过生日了，你准备送上礼物代表你的心意，你选择了买花寄给她，希望她能感受到你的心意；但是往往理想很丰满，现实很骨感！别忘了还有经纪人，因为签收你的礼物的往往不是明星本人而是经纪人：
经纪人就是一个代理
*/
var Fans = {
  flower(){
      Agent.reception("花");
  }
}

var Agent = {
  reception:function(gift){
      console.log("粉丝送的:"+gift);   //粉丝送的：花
      star.reception("花");
  }
}
var star = {
  reception:function(gift){
      console.log("收到粉丝的:"+gift);
  }
}

Fans.flower();    //收到粉丝的：花
```

----

# 行为型
## 1.职责链模式
## 2.命令模式
## 3.解释器模式
## 4.迭代器模式
## 5.中介者模式
## 6.备忘录模式
## 7.观察者模式（发布/订阅模式）

```javascript
// 可以参考vue的双向绑定
```
## 8.状态模式
## 9.策略模式
```javascript
// 策略模式：值根据不同的策略来执行不同的方法
/* 使用场景：年终将至，某公司决定提前发年终奖，但是年终奖的计算是有一定的规则的，年终奖的多少跟绩效考核密切相关；所以某公司的年终奖方案是这样的：

　　　　绩效考核为S的员工，年终奖是个人月工资的4倍；

　　　　绩效考核为A的员工，年终奖是个人月工资的3倍；

　　　　绩效考核为B的员工，年终奖是个人月工资的2倍；
*/
function calculateBonus(level,salary){
  if(level === 'S'){
      return salary*4;
  }
  
  if(level === 'A'){
      return salary*3
  }

  if(level === 'B'){
      return salary*2
  }
}

console.log(calculateBonus("S",14000));  //56000
console.log(calculateBonus("A",10000)); //30000
console.log(calculateBonus("B",5000));  //10000
```
## 10.访问者模式
## 11.模板方法模式
