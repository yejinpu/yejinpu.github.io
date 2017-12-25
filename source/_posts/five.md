---
title: 使用gulp + babel实现es6编译es5
date: 2017-12-25 17:29:49
tags:
- ES6
- gulp
categories: 项目搭建
---
# 1、安装node环境  
&emsp;&emsp;**在nodejs官网下载稳定版客户端，链接地址为 `https://nodejs.org/en/`**
# 2、配置babel加载项
&emsp;&emsp;**使用node自带的npm包管理器在终端执行下列命令**（需要在项目文件夹中执行）
```
      npm install gulp -g
      npm install gulp-babel --save-dev
      npm install babel-present-es2015 --save-dev
      npm install babel-core --save-dev
      npm install gulp-watch -g
```
# 3、在gulpfile.js文件中添加代码
&emsp;&emsp;**以下代码中所有路径需要自行配置**
```
      var glulp = require('gulp'),
          babel = require('gulp-babel');
      gulp.task('js', function(){
          gulp.src(['app/js/**/*.js','!app/js/demo/**/*.js','!app/js/**/*.min.js'])
              .pipe(babel({
                  'presets':['es2015']
              }))
              .pipe(gulp.dest('./js'));
      });
      gulp.task('startWatch', function(){
          gulp.watch(['app/js/**/*.js','!app/js/demo/**/*.js','!app/js/**/*.min.js'], ['js']);
      });
```
# 4、配置完成，运行实现转码
&emsp;&emsp;**运行`gulp js`即可实现转码**
