---
title: 移动端常见的一些兼容性问题
date: 2017-12-15 19:44:46
tags: 兼容问题
categories: 移动端
---
# 1、部分安卓系统的浏览器中背景图片显示模糊

**解决办法：**    

&emsp;&emsp;使用2倍大小的背景图代替img标签。例如一个div的宽高是100\*100，背景图必须是200\*200，然后使用
`background-size: contain;`这样显示出来的图片就比较清晰了。

# 2、图片加载缓慢  

**解决办法：**

&emsp;&emsp;使用加载`canvas`的方法来加载图片。具体的canvasAPI参见：  
[http://www.w3school.com.cn/tags/html_ref_canvas.asp](http://www.w3school.com.cn/tags/html_ref_canvas.asp)

# 3、防止手机中网页放大和缩小  

**解决办法：**

&emsp;&emsp;设置meta标签属性，代码如下:  

```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale = 1.0, user-scalable=0">
```

# 4、禁止选中文本  

**解决办法：**

```
    Element {
      -webkit-user-select:none;
      -moz-user-select:none;
      -khtml-user-select:none;
      user-select:none;
    }
```

# 5、圆角bug  

**解决办法：**

&emsp;&emsp;某些Android手机圆角失效，需要添加样式`background-clip: padding-box;`


# 6、IOS设置input按钮样式被默认样式覆盖  

**解决办法：**

```
    input{
      border: 0; 
      -webkit-appearance: none; 
    }
```

# 7、IOS键盘字母输入默认首字母大写  

**解决办法：**
```
    <input type="text" autocapitalize="off">
```

# 8、移动端点击300ms延迟  

**原因：**  

&emsp;&emsp;当页面过大出现滚动条，单击页面时，系统会暂缓执行click事件，用于等待300ms内是否存在二次单击，存在则取消执行click事件，反而执行页面缩放，这种机制会延缓click触发，造成事件响应延时，所以一般移动端页面会禁用页面缩放。

**解决办法：**

&emsp;&emsp;我们一般在移动端用tap事件来取代click事件。推荐两个js，一个是fastclick，一个是tap.js。

# 9、移动端 HTML5 audio autoplay 失效问题  

**原因：**

&emsp;&emsp;由于自动播放网页中的音频或视频，会给用户带来一些困扰或者不必要的流量消耗，所以苹果系统和安卓系统通常都会禁止自动播放和使用 JS 的触发播放，必须由用户来触发才可以播放。

**解决办法：**

&emsp;&emsp;先通过用户 touchstart 触碰，触发播放并暂停（音频开始加载，后面用`JS`再操作就没问题了）。
```
    document.addEventListener('touchstart',function() {
      document.getElementsByTagName('audio')[0].play();
      document.getElementsByTagName('audio')[0].pause();
    });
```
