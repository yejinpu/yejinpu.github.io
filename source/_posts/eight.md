---
title: 网上比较好的移动端兼容问题处理方法
date: 2018-2-24 16:05:49
tags: 兼容问题
categories: 移动端
---
**下面是在网上搜索到的一些移动端兼容性问题，写的不错，所以copy过来有空看一看。**  

**一. css部分**

&emsp;&emsp;1、body如果设置height:100%;overflow:hidden是依然可以滑动的，如果需禁止，要再加一层div设置 height:100%加overflow：hidden（html,body加height:100%） ，这样元素超出body的高度也不能滑动了。
或者同时给html，body加height:100%;overflow:hidden  

&emsp;&emsp;2、meta标签  
`<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no"/>`  
&emsp;&emsp;移动端加上这个标签才是真正的自适应，不加的话，假如你把一个980px宽度(手机端常规是980)的PC网页放在手机上显示，倒也能正常显示不出现滚动条，不过是移动设备对页面 做了缩小优化，所以字体等都相应缩小了（980px是相对于手机像素的，我的是超过1000px多一些就出现滚动条了，这个没具体研究）。关于  initial-scale=1 ，这个参照iphone5的尺寸320*568，如果你页面按照640*1136做的话，scale就设为0.5  
`<meta content="yes" name="apple-mobile-web-app-capable">`   IOS中safari允许全屏浏览   
`<meta content="black" name="apple-mobile-web-app-status-bar-style">`  IOS中Safari顶端状态条样式  
`<meta content="telephone=no" name="format-detection">`  忽略将数字变为电话号码
`<meta content="email=no" name="format-detection">`   忽略识别email

&emsp;&emsp;3、做全屏显示的图片时，一般为了兼容大部分的手机，图片尺寸一般设为640*960（我是觉得这个尺寸好，也看不少的图片也是这个尺寸，视情况而定）  

&emsp;&emsp;4、去除webkit的滚动条 

```  
    element::-webkit-scrollbar{
    
          display: none;
    
    }
```
&emsp;&emsp;5、去除button在ios上的默认样式

```  
    -webkit-appearance: none;
    
    border-radius: 0 
```
&emsp;&emsp;6、不想让按钮touch时有蓝色的边框     -webkit-tap-highlight-color:rgba(0,0,0,0);

&emsp;&emsp;7、如果要用到fixed譬如导航等，可以用absolute达到一样的效果，把body设为100%；将元素absolute到body上即可，不过这样不能让body滚动，如果需要有滚动的地方，就放在div中滚动

&emsp;&emsp;8、在移动端做动画效果的话，如果通过改变绝对定位的top，或者margin的话做出来的效果很差，很不流畅，而使用css3的transition或者animation的效果将会非常棒（这一方面IOS比android又要好不少）。
如果用translate3d来实现动画，会开启GPU加速，硬件配置差的安卓用起来会耗性能，需慎用（借用1楼评论的）

&emsp;&emsp;9、使用图片时，会发现图片下总是有大概4px的空白，（原因据说图片是inline，触发baseline什么的。。。）常用解决方法有

```  
    img{display:block}；
    
    img{vertical-align:top}也可取其他几个值，视情况而定
```
其他解决办法  见http://blog.sina.com.cn/s/blog_4990aec60100rckt.html

&emsp;&emsp;10、学会使用display:-webkit-box的布局，能很好地帮我们做到页面自适应，譬如 导航栏 这些个人觉得特别适合，具体使用方法此处不叙述

&emsp;&emsp;11、使用 a 标签的话，尽量让 a 标签 block ，尽量让用户可点击区域最大化

&emsp;&emsp;12、两个页面使用了transform之后，页面下的z-index有时就会失效，我遇到过，但没去认真探究，只是把z-index提高就好了，如果遇到这个问题的，详细可以   看这

&emsp;&emsp;13、在iOS中，当你点击比如 input 准备输入时，虚拟键盘弹出，整个视窗的 高度 就会变为 减去键盘 的高度，加入你在底部有fixed的元素比如btn，这个元素就会跑上来，一般都不会太美观。我是当focus时就把它设为absolute，视情况而定

&emsp;&emsp;14、禁止用户选中文字   -webkit-user-select:none

&emsp;&emsp;15、当你把input设为 width:100%时，有时可能会遇到input的宽度超出了屏幕，这时可对input加一个属性 box-sizing:border-box  
&emsp;&emsp;关于box-sizing:border-box，此属性是把边框的高宽包含在盒子的高宽中，假如你设置两个元素float且各占50%，又都有border时，用这个属性就可以完美地让它们能显示在同一行

&emsp;&emsp;16、要table的td用col设置了宽度后超出部分隐藏的话给table加属性table-layout:fixed（固定宽度布局）

&emsp;&emsp;17、如果想改变 placeholder  里的样式，需要用css伪类。    如  ::-webkit-input-placeholder{color:#ccc}

&emsp;&emsp;18、做一个方向箭头比如  “>” 时，可以用一个正方形的div，设置border:1px 1px 0 0；然后rotate(45deg)
&emsp;&emsp;19、CSS权重：style是1000，id是100，class是10，普通如li、div和伪类是1，通用如*是0；

&emsp;&emsp;20、使用rem时，设某个div比如的height:3rem;line-height:1.5rem;overflow:hidden;时，在某些android手机上可能会出现显示不止两行,第三行会显示点头部。 解决：利用js获取文字line-height再去设置div高度  

**二. JS部分**  

&emsp;&emsp;1、想要在touchmove:function(e,参数一)加一个参数，结果直接使用e.preventDefault()就会 e 报错，处理方法为

```  
    touchmove:function(e,参数一){
    　　var e=arguments[0]
          e.preventDefault()
    }
```
&emsp;&emsp;2、HTML5的新js API有新的选择器，比如querySelector(".class #id")和querySelectorAll(".class  element")。

&emsp;&emsp;3、点击一个元素时，使用touchstart会立即触发，而使用click则用有大概0.3s的延迟

&emsp;&emsp;4、用e.preventDefault()会阻止的scroll，click等事件

**三. 微信部分**

&emsp;&emsp;1、微信浏览器里均不能打开下载的链接，需在浏览器打开

&emsp;&emsp;2、如果在网页里嵌套一个iframe，src为其他的网址等，当在微信浏览器打开时，如果irame里的页面过大，则iframe的src不能加载（具体多大不知道，只是遇到过）

&emsp;&emsp;3、微信升级到6.0后，在微信网页里需要用到设置分享的标题等，需要用到JSSDK，同时去微信公众平台里绑定里放置网页的域名。
&emsp;&emsp;不过JSSDK也不是所有问题都解决了，在android里点击分享到朋友圈时就触发了分享成功的回调函数，即使取消分享也已经触发了成功的函数（现在不知道是否有修复，如果遇到了这类问题，应该就是这个原因）