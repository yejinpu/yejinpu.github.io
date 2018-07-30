---
title: less的简单使用
date: 2018-07-30 13:46:47
tags: 
- less
- css
---
&emsp;&emsp;less是预处理语言，简化开发流程，缩短开发时间，便于维护和扩展css。  
# 1、安装less  
* 全局安装less  

```  
npm install -g less
```
* 直接使用命令行将less文件转编译为css文件   

```  
lessc styles.less styles.css
```
# 2、浏览器端使用  
* 在全局设置  

```  
<link rel="stylesheet/less" type="text/css" href="styles.less" />

<!-- set options before less.js script -->

<script>
  less = {
    env: "development",
    async: false,
    fileAsync: false,
    poll: 1000,
    functions: {},
    dumpLineNumbers: "comments",
    relativeUrls: false,
    rootpath: ":/a.com/"
  };
</script>

<script src="less.js"></script>
```
# 3、可能会需要的gulp配置
```
var less = require('gulp-less');
//定义一个testLess任务（自定义任务名称）
gulp.task('testLess', function () {
    gulp.src('app/css/*.less')  //该任务针对的文件
        .pipe(less()) //该任务调用的模块
        .pipe(gulp.dest('app/css')); //将会在css下生成index.css
});
gulp.task('startless', function() {
  gulp.watch('app/css/*.less', ['testLess'])
});
```
# 4、常用语法
```  
@color:red; //全局定义变量，所有地方都可以使用
.body{
    background:@color;
}
a{
    .body
}

.one{ //嵌套
    color:@color;
    .two{
        marign:0px;
        .three{
            padding:0px;
        }
    }
}


.four{//局部定义变量，只有该作用域可以使用
    margin: 11px+20px;
    @c:122px;
    padding:@c;
    &:hover{
      color:red;//属性访问器 
    }
}

@var: red; //变量和混合器首先在本地查找，如果没有找到，编译器将查看父范围，依此类推。
#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
// 设置空白宽度
.setwidth(@w, @n:1) when (@n <= 1){
    .blank-w@{w}-add{
        width:@w+0px;
    }
    .setwidth(@w, (@n+1));
}
// 调用
.setwidth(20);
```