---
title: HTML5标准模板
urlname: nk8rsv
date: '2021-04-14 16:01:12 +0800'
tags: []
categories: []
---

# **1、HTML5 标准模板**

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <title>HTML5标准模版</title>
  </head>
  <body></body>
</html>
```

# **2、移动端模板**

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, shrink-to-fit=no"
    />
    <meta name="format-detection" content="telephone=no" />
    <title>移动端HTML模版</title>

    <!-- S DNS预解析 -->
    <link rel="dns-prefetch" href="" />
    <!-- E DNS预解析 -->

    <!-- S 线上样式页面片，开发请直接取消注释引用 -->
    <!-- #include virtual="" -->
    <!-- E 线上样式页面片 -->

    <!-- S 本地调试，根据开发模式选择调试方式，请开发删除 -->
    <link rel="stylesheet" href="css/index.css" />
    <!-- /本地调试方式 -->

    <link rel="stylesheet" href="http://srcPath/index.css" />
    <!-- /开发机调试方式 -->
    <!-- E 本地调试 -->
  </head>
  <body></body>
</html>
```

重置样式

```css
* {
  -webkit-tap-highlight-color: transparent;
  outline: 0;
  margin: 0;
  padding: 0;
  vertical-align: baseline;
}
body,
h1,
h2,
h3,
h4,
h5,
h6,
hr,
p,
blockquote,
dl,
dt,
dd,
ul,
ol,
li,
pre,
form,
fieldset,
legend,
button,
input,
textarea,
th,
td {
  margin: 0;
  padding: 0;
  vertical-align: baseline;
}
img {
  border: 0 none;
  vertical-align: top;
}
i,
em {
  font-style: normal;
}
ol,
ul {
  list-style: none;
}
input,
select,
button,
h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: 100%;
  font-family: inherit;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
a {
  text-decoration: none;
  color: #666;
}
body {
  margin: 0 auto;
  min-width: 320px;
  max-width: 640px;
  height: 100%;
  font-size: 14px;
  font-family: -apple-system, Helvetica, sans-serif;
  line-height: 1.5;
  color: #666;
  -webkit-text-size-adjust: 100% !important;
  text-size-adjust: 100% !important;
}
input[type="text"],
textarea {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}
```

# **3、PC 端模板**

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="keywords" content="your keywords" />
    <meta name="description" content="your description" />
    <meta name="author" content="author,email address" />
    <meta name="robots" content="index,follow" />
    <meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1" />
    <meta name="renderer" content="ie-stand" />
    <title>PC端HTML模版</title>

    <!-- S DNS预解析 -->
    <link rel="dns-prefetch" href="" />
    <!-- E DNS预解析 -->

    <!-- S 线上样式页面片，开发请直接取消注释引用 -->
    <!-- #include virtual="" -->
    <!-- E 线上样式页面片 -->

    <!-- S 本地调试，根据开发模式选择调试方式，请开发删除 -->
    <link rel="stylesheet" href="css/index.css" />
    <!-- /本地调试方式 -->

    <link rel="stylesheet" href="http://srcPath/index.css" />
    <!-- /开发机调试方式 -->
    <!-- E 本地调试 -->
  </head>
  <body></body>
</html>
```

重置样式

```css
html,
body,
div,
h1,
h2,
h3,
h4,
h5,
h6,
p,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
input,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
textarea,
article,
aside,
audio,
canvas,
figure,
footer,
header,
mark,
menu,
nav,
section,
time,
video {
  margin: 0;
  padding: 0;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: 100%;
  font-weight: normal;
}
article,
aside,
dialog,
figure,
footer,
header,
hgroup,
nav,
section,
blockquote {
  display: block;
}
ul,
ol {
  list-style: none;
}
img {
  border: 0 none;
  vertical-align: top;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
strong,
em,
i {
  font-style: normal;
  font-weight: normal;
}
ins {
  text-decoration: underline;
}
del {
  text-decoration: line-through;
}
mark {
  background: none;
}
input::-ms-clear {
  display: none !important;
}
body {
  font: 12px/1.5 \5FAE\8F6F\96C5\9ED1, \5B8B\4F53, "Hiragino Sans GB", STHeiti, "WenQuanYi Micro Hei",
    "Droid Sans Fallback", SimSun, sans-serif;
  background: #fff;
}
a {
  text-decoration: none;
  color: #333;
}
a:hover {
  text-decoration: underline;
}
```
