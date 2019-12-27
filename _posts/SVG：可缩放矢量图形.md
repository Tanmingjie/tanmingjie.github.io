---
layout: post
title: SVG: 可缩放矢量图形
subtitle: <Svg>
author: oliver
header-img: img/post-bg-keyboard.jpg
catalog: true
tags:
    - HTML
---
# 概述
SVG 是一种基于 XML 语法的图像格式

SVG 则是属于对图像的形状描述，所以它本质上是文本文件，体积较小，且不管放大多少倍都不会失真。

# 语法
## <svg>标签
SVG 代码都放在顶层标签<svg>之中，如下
```
<svg width="100%" height="100%">
  <circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
//<svg>的width属性和height属性，指定了 SVG 图像在 HTML 元素中所占据的宽度和高度。除了相对单位，也可以采用绝对单位（单位：像素）。如果不指定这两个属性，SVG 图像默认大小是300像素（宽） x 150像素（高）。
```
如果只想展示 SVG 图像的一部分，就要指定viewBox属性。
```
<svg width="100" height="100" viewBox="50 50 50 50">
  <circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
//<viewBox>属性的值有四个数字，分别是左上角的横坐标和纵坐标、视口的宽度和高度。上面代码中，SVG 图像是100像素宽 x 100像素高，viewBox属性指定视口从(50, 50)这个点开始。所以，实际看到的是右下角的四分之一圆。
```
注意，视口必须适配所在的空间。上面代码中，视口的大小是 50 x 50，由于 SVG 图像的大小是 100 x 100，所以视口会放大去适配 SVG 图像的大小，即放大了四倍。

如果不指定width属性和height属性，只指定viewBox属性，则相当于只给定 SVG 图像的长宽比。这时，SVG 图像的默认大小将等于所在的 HTML 元素的大小。

## <circle>标签
<circle>标签代表圆形
```
//定义3个圆。
<svg width="300" height="180">
  <circle cx="30"  cy="50" r="25" />
  <circle cx="90"  cy="50" r="25" class="red" />
  <circle cx="150" cy="50" r="25" class="fancy" />
</svg>
```
<circle>标签的cx、cy、r属性分别为横坐标、纵坐标和半径，单位为像素。**坐标都是相对于<svg>画布的左上角原点。**

...........省略若干标签

# JavaScript操作
## Dom操作

如果 SVG 代码直接写在 HTML 网页之中，它就成为网页 DOM 的一部分，可以直接用 DOM 操作。
```
<svg
  id="mysvg"
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 800 600"
  preserveAspectRatio="xMidYMid meet"
>
  <circle id="mycircle" cx="400" cy="300" r="50" />
<svg>

上面代码插入网页之后，就可以用 CSS 定制样式。

circle {
  stroke-width: 5;
  stroke: #f00;
  fill: #ff0;
}

circle:hover {
  stroke: #090;
  fill: #fff;
}
```
也可以用JavaScript代码操作SVG
```

var mycircle = document.getElementById('mycircle');

mycircle.addEventListener('click', function(e) {
  console.log('circle clicked - enlarging');
  mycircle.setAttribute('r', 60);
}, false);

```
上面代码指定，如果点击图形，就改写circle元素的r属性。

## 获取SVG DOM
使用<object>、<iframe>、<embed>标签插入 SVG 文件，可以获取 SVG DOM。

```
var svgObject = document.getElementById('object').contentDocument;
var svgIframe = document.getElementById('iframe').contentDocument;
var svgEmbed = document.getElementById('embed').getSVGDocument();
```
注意，如果使用\<img>标签插入 SVG 文件，就无法获取 SVG DOM。

## 读取SVG源码
由于SVG文件就是一段XML文本，因此可以通过读取XML代码的方式，读取SVG源码
```
<div id="svg-container">
  <svg
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xml:space="preserve" width="500" height="440"
  >
    <!-- svg code -->
  </svg>
</div>
```
使用XMLSerializer实例的serializeToString()方法，获取 SVG 元素的代码。

```
var svgString = new XMLSerializer()
  .serializeToString(document.querySelector('svg'));
```