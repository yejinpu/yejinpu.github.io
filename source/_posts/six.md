---
title: 移动端使用mint-ui组件loadmore填坑
date: 2018-02-02 17:51:09
tags: mint-ui
categories: 移动端
---
# 1、去`mint-ui`官网下载安装对应js文件和css文件  
&emsp;&emsp;**链接地址为 `https://mint-ui.github.io/docs/#/en2/loadmore` ,这里需要注意引入的方式，我这里是用cdn的方式引入的。请结合官方API阅读本文章。**
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
# 3、在view结构中写标签`<loadmore></loadmore>`
&emsp;&emsp;**代码如下：**
```
      <div style="height:94vh;overflow:scroll;"> //父级元素必须加高度，加滚动条
        <loadmore :top-method="loadTop"  //关联下拉刷新函数
                  @top-status-change="handleTopChange"  // 关联下拉刷新的自定义文案的状态 
                  @bottom-status-change="handleBottomChange" 
                  :bottom-method="loadBottom" 
                  :bottom-all-loaded="allLoaded"  //该值为true则不能上拉，所以要手动控制
                  :auto-fill="false"  //初次进入页面是否填满页面
                  ref="loadmore">  //绑定需要操作的ele
          <div slot="top"></div> //提示文案必须紧靠<loadmore>标签的内部写
          <div style='overflow: scroll;height:90%;min-height:94vh;'> 
            //这里加上最小高度保证没有数据的时候盒子也是撑开的
            //...
          </div>
          <div slot="bottom"></div> //同理也需要紧靠内部写
        </loadmore>
      </div>
      
      view部分就是这些了
```
# 4、viewModel部分需要使用自定义函数来配合
&emsp;&emsp;**需要自定义的函数有下面几个：**  
```
  new Vue({
    el:'#app',
    data:{
      topStatus:'',
      bottomStatus:'',
      allLoaded:false
    },
    methods:{
      loadTop(){
        //...
        this.$refs.loadmore.onTopLoaded();// 这里必须调用mint-ui的内置函数onTopLoaded()
      },
      loadBottom(){
        //...
        this.allLoaded = true; // 这里别忘了手动修改是否可以继续下拉
        this.$refs.loadmore.onBottomLoaded();// 这里必须调用mint-ui的内置函数onBottomLoaded()
      },
      handleTopChange(status){
        this.topStatus = status; // 这个变量也必须我们自己定义
        //这个函数主要是用来自定义下拉刷新的状态文字，
      },
      handleBottomChange(){
        // 同理这个是自定义上拉加载的状态文字
      }
    }
  })
```
# 5、结束
&emsp;&emsp;到这里已经结束了， 但是移动端就复杂在手机型号太多了，很多时候不能做到兼容所有手机,该组件也是无法兼容所有手机的，目前已知有问题的手机型号为oppo r11和小米Mix2。