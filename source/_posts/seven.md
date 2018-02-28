---
title: 一行命令安装项目缺失模块
date: 2018-02-27 19:59:30
tags:
- npm
- node.js
categories: 命令行
---
&emsp;&emsp;今天总结之前工作的笔记，发现了一个好东西可以自动检测安装项目package.json中缺失的模块，并且是一行命令全部安装。省时又省力的好东西当然要分享给大家了。O(∩_∩)O哈哈~下面开始介绍。
# 1、首先需要有node环境  
&emsp;&emsp;在nodejs官网下载稳定版客户端，链接地址为 `https://nodejs.org/en/`
# 2、需要安装npm-install-missing工具
执行以下代码：
```  
  npm install -g npm-install-missing
```
这里要注意npm源是否需要切换，如果安装失败可能涉及到需要（翻\*\*墙），顺便介绍下如何查看当前所有源和如何切换到对应源。  
```
  执行npm install -g nrm安装源的管理工具
  nrm ls列出所有可选源  
  nrm test测试所有源的连接时间  
  nrm use cnpm 切换源
```
# 3、成功后直接在缺失模块项目的根目录执行下面命令即可一次安装所有缺失模块
执行以下代码：
```  
  npm-install-missing
```
# 4、结束！
