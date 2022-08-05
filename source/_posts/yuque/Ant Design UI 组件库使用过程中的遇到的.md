---
title: Ant Design UI 组件库使用过程中的遇到的
urlname: 1718307cf704d8c2acb43ac51f9a0eae
date: '2021-04-15 10:45:31 +0800'
tags: []
categories: []
---

### 1.Upload 组件，需要限制上传类型为 Excel 表类型，那么需要增加 accept 属性

accept="application/vnd.ms-excel, // xls
application/vnd.openxmlformats-officedocument.spreadsheetml.sheet” //xlsx

上传类型为 zip：

accept=“application/zip"

### 2.umi-request 处理 get 请求参数

传参时，参数字段必须是 params，例如：{method: 'GET', params: data}
