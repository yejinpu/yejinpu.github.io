---
title: vue实现跨域方法总结
date: 2017-12-18 16:32:39
tags: 跨域
categories: vue
---
**这里总结了三个方法：**  

1、让后台人员更改服务器header信息

2、引入jquery的jsonp请求

3、在vue项目的config下index.js文件中修改proxytable配置，具体代码如下
```
    proxyTable: {
    '/api': { //使用"/api"来代替"http://f.apiplus.c"
    target: 'http://f.apiplus.cn', //源地址
    changeOrigin: true, //改变源
    pathRewrite: {
      '^/api': 'http://f.apiplus.cn' //路径重写
      }
    }
    }
```