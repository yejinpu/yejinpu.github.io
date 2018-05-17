---
title: hexo + github搭建个人博客
date: 2018-05-17 17:32:01
tags: 
- hexo
- github
---
&emsp;&emsp;本文仅仅讲述了怎么搭建一个基础的博客，并没有涉及博客内部的功能介绍，比如搜索，评论，访问量统计，SEO之类的...大家自己探索吧。
# 1、准备工作  
&emsp;&emsp;github账号、 hexo博客框架、next主题、还有git之类的工具、、、  

# 2、初始化hexo博客
* 本地打开git，初始化`hexo init XXX`,生成基础文件。
* 执行`cd XXX`进入hexo项目。
* 执行`npm install hexo --save `,将hexo依赖安装到项目环境。
* 执行`npm install`安装项目所有依赖。
* 运行`hexo s`，在浏览器输入`localhost:4000/`可以看到博客在本地已经搭建完成。  

# 3、在github创建代码库，并生成gitub页面
&emsp;&emsp;进入`settings`，找到`GitHub Pages`，生成首页。  

# 4、关联本地hexo博客和github页面
* 回到管理hexo博客的文件执行`git init`，初始化项目。
* 添加项目地址`git remote add origin 项目地址`.
* 运行`ssh-keygen`生成项目公钥和私钥，并在github上配置公钥。
* 执行`eval $(ssh-agent -s)`.
* 本地添加私钥`ssh-add 私钥`。
* 本地配置`.gitignore`文件，将公钥和私钥加入忽略。
* 将本地hexo博客推送到github。  

# 5、部署博客
* 在`_comfig.yml`中配置下面项  
```
  deploy:
  type: git
  repo: git@github.com:XXX.git
  branch: maste
```
* 执行`hexo g -d`完成部署，部署会延迟几十秒，稍后打开github页面即可查看。  

&emsp;&emsp;到此博客搭建完成！感谢！  


  
* **如果部署失败提示`ERROR Deployer not found: git`，是有依赖没有安装的原因，执行`cnpm install --save hexo-deployer-git`，再重新部署即可。**