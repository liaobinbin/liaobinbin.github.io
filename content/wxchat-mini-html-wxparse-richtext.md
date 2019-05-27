---
title: '微信小程序原生富文本渲染和wxParse'
date: 2019-05-27T20:21:58+08:00
---

# 微信小程序 富文本渲染

微信小程序本身提供`rich-text`的组件，直接富文本字符串丢进`nodes`属性即可。

> 你以为就这么就结束了？不，你错了。因为后台为了兼容`IE9`，我选用`wangEditor2`所
> 编辑的富文本你会发现好多标签都没有识别出来，特别是 HTML4 标准的。 好坑。

<!--more-->

`wangEditor2`所产生的生成的富文本标签，没有`style`属性。例如对齐居中是`align`属性

字体大小没有单位，只有`1 ~ 7`的数字来标识。还有一些具体的没有查明。

`rich-text`与`wangEditor2`的配合不怎么好，容易出现文字消失，文字布局消失，文字没有颜色之类的、图片不能放大预览等等奇奇怪怪的问题。

总之微信小程序还是推荐`wxParse`这个库。用起来比较简单。

## 克隆最新仓库

```bash
git clone git@github.com:icindy/wxParse.git
```

## 拷贝关键文件到你的项目内

我放在`libs`目录下

```bash
├── xxxxxxxx.js #其他的库
└── wxParse
    ├── html2json.js
    ├── htmlparser.js
    ├── showdown.js
    ├── wxDiscode.js
    ├── wxParse.js
    ├── wxParse.wxml
    └── wxParse.wxss

```

本来还有个`emoji`文件夹的。里面全是图片，洁癖患者直接删掉即可。

## 如何使用

### 某 Page ---- xxxx

#### xxxx.js

```javascript
//在使用的View中引入WxParse模块
const WxParse = require('../../wxParse/wxParse.js')

const article = '<div>我是HTML代码</div>'
/**
 * WxParse.wxParse(bindName , type, data, target,imagePadding)
 * 1.bindName绑定的数据名(必填)
 * 2.type可以为html或者md(必填)
 * 3.data为传入的具体数据(必填)
 * 4.target为Page对象,一般为this(必填)
 * 5.imagePadding为当图片自适应是左右的单一padding(默认为0,可选)
 */
const _this = this
WxParse.wxParse('article', 'html', article, _this, 5)
```

#### xxxx.wxss

```css
/*
在使用的Wxss中引入WxParse.css,可以在app.wxss
*/
@import '/wxParse/wxParse.wxss';
```

#### xxxx.wxml

```html
<!-- 引入模板 -->
<import src="你的路径/wxParse/wxParse.wxml" />

<!-- 这里data中article为bindName -->
<template is="wxParse" data="{{wxParseData:article.nodes}}" />
```

> 注：更多详细用法请参考[github](https://github.com/icindy/wxParse)

### wxParse 的坑

- 空格渲染不一定出的来
- 识别不到`wangEditor2`的居中属性

### 解决空格问题

修改`wxDiscode.js`文件中加入

```javascript
str = str.replace(/&nbsp;/g, '\xa0') // 这行解决了空格不显示的问题
```

### 处理不被识别的属性

修改`html2json.js`中的

```javascript
if (name == 'style') {
  console.dir(value)
  //  value = value.join("")
  node.styleStr = value
}
```

为

```javascript
// 这个属性可以根据自己富文本内容进行修改。来配合自己的富文本编辑器达到最好的效果
if (name == 'align') {
  if (!node.styleStr) node.styleStr = ''
  node.styleStr = node.styleStr + 'text-align: ' + value + ';'
}

if (name == 'style') {
  console.dir(value)
  //  value = value.join("")
  node.styleStr += value
}
```

## 最后

下回写改`wangEditor2`的一些小 BUG。
