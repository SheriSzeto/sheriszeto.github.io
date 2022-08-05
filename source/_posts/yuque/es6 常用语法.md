---
title: es6 常用语法
urlname: cqi8e3
date: '2021-04-14 16:09:44 +0800'
tags: []
categories: []
---

# 1.常量

```javascript
const PI = "3.1415926";
//常量一旦创建，值将不可以改变，否则会报错
PI = 3; //报错
```

# 2.作用域

```javascript
// es5作用域
const callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks[i] = function () {
    return i * 2;
  };
}
console.table([callbacks[0](), callbacks[1]()]); //4 4
```

这里的 for 循环中的 i 会被提前至外部，i 变量为全局变量

```javascript
//es6的块级作用域，只在括号内有意义
const callback = [];
for (let j = 0; j < 2; j++) {
  callback[j] = function () {
    return j * 2;
  };
}

console.table([callback[0](), callback[1]()]); //0 2
```

# 3.箭头函数

值得注意的是，es6 中的箭头函数本没有自己的 this，他的 this 是继承而来，默认指向定义他所处的对象，此处指向的就是 factory 实例

```javascript
{
  function factory() {
    this.a = "a";
    this.b = "b";
    this.c = {
      a: "a+",
      b: () => {
        return this.a;
      },
    };
  }

  console.log(new factory().c.b()); //a
}
```

# 4.proxy 代理器

```javascript
var proxy = new Proxy(target, handler); //参数target表示要拦截的目标对象，handler用来定制拦截行为
eg: var ceshi = {
  get: function (target, key) {
    test(key, "get");
    return target[key];
  },
  set: function (target, key, value) {
    test(key, "set");
    if (key === "age") {
      if (0 < value < 100) {
        console.log(`年龄是${value}`);
      }
      if (value > 100) {
        console.log("输入的年龄无效！！！");
      }
      if (value < 0) {
        console.log("年龄不能为负数");
      }
    } else {
      console.log("你传入的参数不对");
    }
    return true;
  },
};
function test(key, action) {
  if (key[0] === "_") {
    return console.log("你传入的参数不对，请重新传入参数");
  }
}
var target = {};
var proxy = new Proxy(target, ceshi);

console.log(proxy.age); //undefined
console.log((proxy.age = 20)); //年龄是20  20
console.log((proxy.age = 120)); //年龄是120      输入的年龄无效！！！ 120
console.log(proxy._age); //undefined
```

# 5.变量的结构赋值

```javascript
let { a, b, c } = { a: 1, b: 2, c: 3 };
```

# 6.字符串拓展

```javascript
{
     var name='张三',age=20';
     console.log(`Hello ${name}, age is ${age}`)
}
```

# 7.Promise

1）创建一个 Promise 实例

```javascript
// 以画饼为例
const bing = new Promise((resolve, reject) => {
  if ("画饼成功") {
    resolve("happy together");
  } else {
    reject("有难同当");
  }
});
```

Promise 的三个状态

- pending: 执行中

- fulfilled: 完成状态

- rejected: 失败状态

2）订阅方式，通过 then，catch

```javascript
bing.then(
  (success) => {
    console.log("success");
  },
  (fail) => {
    console.log("fail");
  }
);
```

# 8.数组

- filter

语法：var filteredArray = array.filter(callback[,thisObject])
参数说明：callback：要对每个数组元素执行的回调函数
                    thisObject：在执行回调函数时定义的 this 对象

```javascript
// 过滤大于10的数组元素
function isBigEnough(ele, index, array) {
  return ele >= 10;
}
var filtered = [12, 5, 8, 100].filter(isBigEnough); // [12,100]
```

- map

```javascript
// 将所有的数组元素转换为大写
var strings = ["hello", "Array", "WORLD"];
function makeUpperCase(v) {
  return v.toUpperCase();
}
var uppers = strings.map(makeUpperCase);
```

- some（一真即真）

语法：对数组中的每个元素都执行一次指定的函数（callback），直到此函数返回 true，如果发现这个元素，some 返回 true，如果回调函数对每个元素执行后都返回 false，some 将返回 false。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。

```javascript
function isBigEnough(ele, index, array) {
  return ele >= 10;
}
var passed = [2, 5, 8, 1, 4].some(isBigEnough); // false
var passed1 = [12, 5, 1, 4].some(isBigEnough); // true
```

- every（一假即假）

```javascript
function isBigEnough(ele, index, array) {
  return ele >= 10;
}
var passed = [2, 5, 8, 1, 4].every(isBigEnough); // false
var passed1 = [12, 15, 11, 14].every(isBigEnough); // true
```
