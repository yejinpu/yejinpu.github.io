---
title: 移动端使用mint-ui组件loadmore填坑
date: 2018-02-02 17:51:09
tags: mint-ui
categories: 移动端
---
# 1、去`mint-ui`官网下载安装对应js文件和css文件  
&emsp;&emsp;**链接地址为 `https://mint-ui.github.io/docs/#/en2/loadmore`**
# 2、在`vue`中注册对应组件loadmore
&emsp;&emsp;**具体代码位置如下**（也可使用全局注册）
```
      new Vue({
        el:'#app',
        data:{
          //...
        },
        methods:{
          //...
        },
        components:{
          loadmore
        }
      })
```
# 3、在`view`结构中写标签`<loadmore></loadmore>`
&emsp;&emsp;**代码如下：**
```
      <div style="height:94vh;overflow:scroll;"> //这里必须加高度，加滚动条
        <loadmore>
          <div slot="top"></div> //提示文案必须紧靠<loadmore>标签写
          <div style='overflow: scroll;height:90%;min-height:94vh;'> //这里
            //..
          </div>
          <div slot="bottom"></div> //同理
        </loadmore>
      </div>
```
# 4、配置完成，运行实现转码
&emsp;&emsp;**下周再写吧**
