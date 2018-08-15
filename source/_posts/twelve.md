---
title: GKA 简单的、高效的帧动画生成工具
date: 2018-08-15 12:13:11
tags:
- photo
categories: 图片处理
---
# 1、简介
  
  只需一行命令，快速图片优化、生成动画文件，支持效果预览。

- 一键式: 图片文件批量序列化重命名，生成帧动画文件，支持预览
- 性能佳: 支持相同图片复用✓ 图片空白裁剪✓ 合图优化✓ 图片压缩✓ 图片空白拆分优化✓ 图片像素差优化✓ 多倍图适配✓
- 多模板: 内置多种文件输出模板，支持自定义模板  

# 2、安装  

```
    npm i gka -g
```
# 3、使用
  
```
    gka <dir> [options]
```
# 4、Option参数选项
  
```
    -d, --dir <string> # 图片文件夹地址
    -u, --unique [boolean] # 开启相同图片复用优化
    -c, --crop [boolean] # 开启空白裁剪优化
    -s, --sprites [boolean] # 开启合图优化
    -m, --mini [boolean] # 开启图片压缩
    -p, --prefix [string] # 文件重命名前缀
    -t, --template <string> # 生成动画文件模板 默认 css ，可选模见 Templates 模板列表
    -f, --frameduration <number> # 每帧时长，默认 0.04
    -i, --info [boolean] # 开启输出信息文件
    -o, --output <string> # 指定生成目录地址
    -a, --algorithm <string> # 合图布局模式 默认 left-right，可选 binary-tree | top-down ..
    --bgcolor <string> # 为图片增加背景色，可选，支持格式：'rgb(255,205,44)'、 '#ffcd2c'
    --count <number> # 生成多合图，指定几张图片合成一张合图，可选
    --ratio <number> # 生成指定的N倍图动画，如 --ratio 2 支持retina屏幕的2倍图动画， 可选
    --split [boolean] # 开启图片空白拆分优化，与 -t canvas 结合使用
    --diff [boolean] # 开启图片像素差优化，与 -t canvas 结合使用
```
# 5、`Templates`模板列表
  
  **使用方式**  
```
    gka 图片目录 -t 模板名
```
  
**内置的模板列表**  

* css

	* 默认模板
	* 输出 css 动画文件
	* 结合 -ucs 支持 相同帧图片复用✓ 空白裁剪优化✓ 合图优化✓ (可选)
* canvas

  * 输出 canvas 动画文件
  * 结合 -ucs 支持 相同帧图片复用✓ 空白裁剪优化✓ 合图优化✓ (可选)
  * 结合 --diff 支持 图片像素差优化✓ (可选)
  * 结合 --split 支持 图片空白拆分优化✓ (可选)
* svg

  * 输出 svg 动画文件，支持 自适应缩放雪碧图✓
  * 结合 -ucs 支持 相同帧图片复用✓ 空白裁剪优化✓ 合图优化✓ (可选)
  

**内置的自定义模板列表**  

* percent

  * 输出 css 百分比动画文件
  * 使用该方案支持 移动端多倍图适配✓ 自适应缩放雪碧图✓
  * 结合 -u 支持 相同帧图片复用✓ (可选)
  * 默认开启 开启合图优化✓
  * Github 地址 [https://github.com/gkajs/gka-tpl-percent](https://github.com/gkajs/gka-tpl-percent)
* createjs

  * 输出 createjs 精灵图动画文件
  * 结合 -uc 支持 相同帧图片复用✓ 空白裁剪优化✓ (可选)
  * 默认开启 开启合图优化✓
  * Github 地址 [https://github.com/gkajs/gka-tpl-createjs](https://github.com/gkajs/gka-tpl-createjs)
* studiojs

  * 输出 studiojs 动画文件
  * 结合 -uc 支持 相同帧图片复用✓ 空白裁剪优化✓ (可选)
  * 默认开启 开启合图优化✓
  * Github 地址 [https://github.com/gkajs/gka-tpl-studiojs](https://github.com/gkajs/gka-tpl-studiojs)  
  

**增加模板**  

* 模板支持动态增加，只需安装需要的模板。即时安装，即刻可用。  
```
    npm i gka-tpl-模板名 -g
```

# 6、使用示例
* 对 E:\img 目录中的图片进行处理。

 * 快速生成帧动画  
```
    gka E:\img
```
 * 进行图片去重、合图优化，输出 css 动画文件
```
    gka E:\img -us
```
 * 进行图片去重、空白裁剪、合图优化，使用 canvas 模板，输出 canvas 动画文件
```
    gka E:\img -ucs -t canvas
```
# 7、结束


