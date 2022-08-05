---
title: 跨域资源共享CORS
urlname: hgk4e6
date: '2021-04-14 16:04:40 +0800'
tags: []
categories: []
---

## **代理请求**

如果做到这一步，你应该非常欣喜。新技能 get! 但是这里有一个问题被回避了：跨域问题。

请重新审视我们的请求：
const endPointURI = 'https://08ad1pao69.execute-api.us-east-1.amazonaws.com/dev/random_joke';
这里我们直接调用了一个「非本地」地址。在实际开发中是比较罕见的。这里能够成功，是因为被调用的 API 做了额外的人为设置，允许一个「非同域」的 ajax 请求。[跨域资源共享 CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 涉及的知识点比较多。我们这里做简单的介绍，目的是让你知道如何在本地开发服务器上增加代理请求的功能。

我们在浏览器中看到的页面是从一个本地开发服务器所伺服的。这个本地开发服务器的地址就是 [http://localhost:8000/](http://localhost:8000/) 。当我们调用 getRandomPuzzle 时，此时发送 ajax 请求页面的域就是 [http://localhost:8000](http://localhost:8000/)，但是请求的数据在另外一台服务器 [https://08ad1pao69.execute-api.us-east-1.amazonaws.com](https://08ad1pao69.execute-api.us-east-1.amazonaws.com/)。一个是 http 一个是 https，路径也不同，端口也不同（https 是 443）。任意这三个东西有一个不同，就认为是资源请求「跨域」了。http 的 _默认_ 安全规则是不允许「跨域」请求。

值得注意的是，发送 ajax 请求的是你的浏览器，所谓 User Agent，而「跨域」的限制 **仅仅在浏览器和服务器之间**。我们不能强制远程服务器都像例子中那样允许「跨域」请求，所以我们能做的就是不要使用浏览器发送请求。所以在前端开发中，一种常见的规避跨域的方法就是：把 ajax 请求发送到你的本地开发服务器，然后本地开发服务器再把 ajax 请求转发到远端去，从网络拓扑上看本地开发服务器起着「反向代理」的作用。本地服务器和远端服务器是「服务器和服务器间的通信」，就不存在跨域问题了。

配置代理也很简单，只需要您在配置文件 config/config.js 中与 routes 同级处增加 proxy 字段，代码如下，
routes: [ // ... ],

- proxy: {
- '/dev': {
- target: 'https://08ad1pao69.execute-api.us-east-1.amazonaws.com’,
- changeOrigin: true,
- },
- },
  配置的含义是：去往本地服务器 localhost:8000 的 ajax 调用中，如果是以 /dev 开头的，那么就转发到远端的 [https://08ad1pao69.execute-api.us-east-1.amazonaws.com](https://08ad1pao69.execute-api.us-east-1.amazonaws.com/) 服务器当中，/dev 也会保留在转发地址中。

比如：

/dev/random_joke 就会被转发到 [https://08ad1pao69.execute-api.us-east-1.amazonaws.com/dev/random_joke](https://08ad1pao69.execute-api.us-east-1.amazonaws.com/dev/random_joke)。
所以 end point URI 也需更改为：

const endPointURI = '/dev/random_joke';

重启 dev server。我们的页面功能没有任何变化，但是发送的 http request 变化了，
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1618387494710-fa76011c-81cf-4845-ab4c-866306ebfb0e.png#align=left&display=inline&height=595&margin=%5Bobject%20Object%5D&name=image.png&originHeight=595&originWidth=1333&size=139317&status=done&style=none&width=1333)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1618387494742-8ef5a05f-25ac-461c-ab14-f1af634e162b.png#align=left&display=inline&height=1588&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1588&originWidth=1218&size=214483&status=done&style=none&width=1218)
