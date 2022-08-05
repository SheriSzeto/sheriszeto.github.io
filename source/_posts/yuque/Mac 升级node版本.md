---
title: Mac 升级node版本
urlname: xmpbns
date: '2022-05-18 14:42:45 +0800'
tags: []
categories: []
---

# 一、查看当前版本

```javascript
node - v;
```

## 二、清除 node 的缓存

```javascript
sudo npm cache clean -f
```

# ** 三、使用 npm 安装 n 模块，**在这里我用的 Node.js 的多版本管理器 n 来升级的

```javascript
sudo npm install -g n
```

# **四、查看 node 的所有版本**

```javascript
npm view node versions
```

# 五、升级到稳定版本

```javascript
sudo n stable
```

# 六、查看版本，完成

```javascript
node - v;
```
