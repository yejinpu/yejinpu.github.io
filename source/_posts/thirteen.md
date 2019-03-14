---
title: 汇总vue项目基础知识
date: 2018-10-17 17:55:07
categories: vue
---
**安装vue-cli**  
```  
  npm install -g @vue/cli
```
如果已经安装了旧版本的vue-cli,需要使用`npm uninstall vue-cli -g`来卸载掉旧版本。  
安装后可以输入`vue --version`来查询安装是否成功。  

**启动服务**  
需要的仅仅是一个App.vue文件，然后在这个App.vue文件目录下运行  
```  
  vue serve
```  
要使用该命令，需要先安装一个扩展插件  
```  
  npm install -g @vue/cli-service-global
```  
**打包文件**  
```  
  vue build
```  

**创建一个新的项目**  
```  
  vue create 项目名称
```  
会提示你选择一个默认带有`eslint`，`bable`的项目还是手动进行配置项目。使用`vue create --help`可以查看更多帮助命令。  

**图形界面**  
```  
  vue ui
```  
在浏览器打开图形界面页面。