---
title: vite+ant design vue创建vue 3项目
urlname: wl9u7h
date: '2022-05-18 14:49:52 +0800'
tags: []
categories: []
---

1. 安装模板文件

```javascript
yarn create vite
```

选择 vue-ts 生成 ts 项目。

2. 安装 ant design vue 的 UI 框架

```javascript
yarn add ant-design-vue
```

> 注意，使用 vite 按需引入 ant-design-vue 的组件，需要安装 unplugin-vue-components ，此目前版本需要 node 14 版本以上支持

3. 安装 unplugin-vue-components，vite 按需引入 ant-design-vue

```javascript
// vite.config.js
import Components from "unplugin-vue-components/vite";
import { AntDesignVueResolver } from "unplugin-vue-components/resolvers";

export default {
  plugins: [
    /* ... */
    Components({
      resolvers: [AntDesignVueResolver()],
    }),
  ],
};
```

4. 安装 vue-router@4，引入路由

```javascript
yarn add vue-router@4
```

新建 router 文件夹，新建 index.js 和 routes.js，

```javascript
// index.ts
import { createRouter, createWebHistory } from "vue-router";
import routes from "./routes";

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

```javascript
// routes.ts
const routes = [
  {
    name: "Option1",
    path: "/Option1",
    component: () => import("../views/Option1.vue"),
  },
  {
    name: "Option2",
    path: "/Option2",
    component: () => import("../views/Option2.vue"),
  },
];

export default routes;
```

在 main.js 中引入，

```javascript
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

createApp(App).use(router).mount("#app");
```
