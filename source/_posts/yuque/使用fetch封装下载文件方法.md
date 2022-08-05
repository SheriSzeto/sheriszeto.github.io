---
title: 使用fetch封装下载文件方法
urlname: 7a0cdba4ee7b5214604c90dfa67281e4
date: '2021-04-15 10:45:31 +0800'
tags: []
categories: []
---

```javascript
import fetch from 'dva/fetch’;
import { stringify } from 'qs’;
import { message } from 'antd’;
import { getFormData } from './index’;

/**
 * Requests a URL, returning a promise.
 *
 * @param  {string} url       The URL we want to request
 * @param  {object} [options] The options we want to pass to "fetch"
 * @return {object}           An object containing either "data" or "err"
 *
/* message.config({
  top: 10,
  duration: 5,
  getContainer: () => window.document.getElementById('root'),
});*/
export default async function request(url, options) {
  let newOptions;
  let newUrl = url;
  let responseBody = {};
  let data = {};
  const { body, params } = options;
  // 判断请求类型
  if ((!(typeof body === 'string')) && body) {
    newOptions = Object.assign({}, options, { body: getFormData(body) });
    responseBody = {
      credentials: 'same-origin',
      ...newOptions,
    };
  } else {
    newOptions = options;
    responseBody = {
      credentials: 'same-origin',
      headers: {
        'Content-Type': 'application/json',
      },
      ...newOptions,
    };
  }
  // get
  if (params) {
    newUrl += `?${stringify(params)}`;
  }
  const response = await fetch(newUrl, responseBody);
  // 报表导出 Content-Type为application/msexcel时，为文件流，进行下载操作
  if (response.headers.get('Content-Type').indexOf('application/msexcel') > -1) {
    response.blob().then((blob) => {
      const a = window.document.createElement('a');
      const downUrl = window.URL.createObjectURL(blob);// 获取 blob 本地文件连接 (blob 为纯二进制对象，不能够直接保存到磁盘上)
      const filename = response.headers.get('Content-Disposition').split('filename=')[1].split('.');
      a.href = downUrl;
      a.download = `${decodeURI(filename[0])}.${filename[1]}`;
      a.click();
      window.URL.revokeObjectURL(downUrl);
    });
    return data;
  }
  if (response.status !== 200) {
    message.error('网络或服务器异常！');
    return {
      data: {
        code: response.status,
      },
    };
  }
  data = await response.json();
  if (data.code !== '200') {
    // 状态码不为304或302的时候报错
    if (data.code !== '304' && data.code !== '302') {
      console.log(data.msg);
      message.error(data.msg);
    }
    // 状态码为304的时候，请求登录接口
    if (data.code === '304') {
      request('/login', {
        method: 'post',
        body: {
          ...data.data,
          redirect_uri: encodeURIComponent(window.location.href),
        },
      });
    }
    // 状态码为302的时候，进行跳转
    if (data.code === '302') {
      window.location = data.msg;
    }
  }
  return { data };
}
```
