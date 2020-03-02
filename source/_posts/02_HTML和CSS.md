---
title: 前端知识_HTML和CSS
---
整理掘金上的文章 一名【合格】前端工程师的自检清单: https://juejin.im/post/5cc1da82f265da036023b628 的答案

## HTML(12.30)

### 1.从规范的角度理解HTML，从分类和语义的角度使用标签

语义化标签: 
```javascript
<header> <footer> <nav> <section> <article> <aside>等
```
- 让页面呈现清晰的结构
- 屏幕阅读器(如果访客有视障)会完全根据你的标记来"读"你的网页
- 搜索引擎的爬虫依赖标签确定上下文和权重问题
- 便于团队开发和维护

标签分类: 
```javascript
文档标签(10 个)：<html>、<head>、<body>、<title>、<meta>、<base> 、<style>、<link>、<script>、<noscript>

表格标签(10 个)：<table>、<thead>、<tbody>、<tfoot>、<tr>、<td>、<th> 、<col>、<colgroup>、<caption>

表单标签(10 个)：<from>、<input>、<textarea>、<button>、<select> 、<optgroup>、<option>、<label>、<fieldset>、<legend>

列表标签(6个)：<ul>、<ol>、<li>、<dl>、<dt>、<dd>

多媒体标签(5个)：<img>、<map>、<area>、<object>、<param>

文章标签：<h1> - <h6> 、<p>、<br>、<span>、<bdo>、<pre>、<acronym>、<abbr>、<blockquote>、<q>、<ins>、<del>、<address>

字体样式标签：<tt>、<i>、<b>、<big>、<small>、<em>、<strong>、<dfn>、<code>、<samp>、<kbd>、<var>、<cite>、<sup>、<sub>

在不同的场景使用不同的标签，更能显示清晰的结构。
```

### 2.常用页面标签的默认样式、自带属性、不同浏览器的差异、处理浏览器兼容问题的方式

```javascript
<head> 标签用于定义文档的头部, 它是所有头部元素的容器. <head> 中的元素可以引用脚本, 指示浏览器在哪里找到样式表, 提供元信息等等, 可以包含的标签有: <base>, <link>, <meta>, <script>, <style>, 以及<title>.

<title>定义文档的标题, 它是head部分中唯一必需的元素.

<meta> 元素可提供有关页面的元信息(meta-information), 比如针对搜索引擎和更新频度的描述和关键词.
```
使用方法参考: [meta标签详解](https://blog.csdn.net/weixin_38788347/article/details/78118739)

### 3.HTML5离线缓存原理

[HTML5离线缓存技术](https://www.jianshu.com/p/46db4d85ed89) 

### 4.可以使用Canvas API、SVG等绘制高性能的动画

[Canvas 进阶（一）二维码的生成与扫码识别](https://juejin.im/post/5d00b3626fb9a07ed74076a9)
[Canvas 进阶（二）写一个生成带logo的二维码npm插件](https://juejin.im/post/5d1c461f6fb9a07f070e4768)
[Canvas 进阶（三）ts + canvas 重写”辨色“小游戏](https://juejin.im/post/5d22af2b6fb9a07ea7133361)
[流动的SVG线条](https://juejin.im/post/5ca34029e51d45141f711797)

## CSS(1.4)

### 1.CSS盒模型，在不同浏览器的差异

- 标准w3c盒子魔性的范围包括 margin, border, padding, content, 并且content部分不包含其他部分
- ie盒子模型的范围也包括margin, border, padding, content, 和标准w3c盒子魔性不同的是: ie盒子魔性的content部分包含了border和 padding

### 2.CSS所有选择器及其优先级、使用场景，哪些可以继承，如何运用at规则

不同级别的优先级: !important > 行内样式 > ID选择器 > 类元素器 > 元素 > 通配符 > 继承 > 浏览器默认属性
相同级别的优先级: 内联(行内)样式 > 内部样式表 > 外部样式表 > 导入样式(@import)

可继承属性: 
```javascript
字体系列属性, font-family, font-weight, font-size, font-style...
文本系列属性, text-indent, text-align, line-heigh,　word-spacing, letter-spacing, text-transform, color
元素可见性：visibility, 光标属性：cursor
```
AT rule:

一、什么是 at-rules

eg：@charset "utf-8";

at-rule 是 CSS 样式声明，以 @ 开头，紧跟着是标识符（charset），最后以分号（;）结尾。

二、几个 at-rules

1、@charset —定义被样式表使用的字符集

2、@import ——告诉 CSS 引擎包含外部的 CSS 样式表

3、@namespace——告诉 CSS 引擎所有的内容都必须考虑使用 XML 命名空间前缀

4、嵌套at-rules

（1）@media——条件组规则。如果设备符合标准定义的条件查询则使用该媒体

（2）@font-face——描述了一个将从外部下载的字体

（3）@keyframes——描述了中间步骤在 CSS 动画的序列

（4）@page——描述了文件的布局变化，当要打印文档时。

（5）@supports——条件组规则，如果浏览器满足给出的规则，则把它应用到内容中

（6）@document——条件组规则，如果被用到文档的 CSS 样式表满足了给定的标准，那么将被应用到所有的内容中。

### 3.CSS伪类和伪元素有哪些，它们的区别和实际应用

伪类: 用于向某些选择器添加特殊的效果. :active, :focus, :link, :visited, :hover, :first-child
伪元素: 用于将特殊的效果添加到某些选择器. :before, :after, :first-line, :first-letter
伪类和微元素的根本区别在于: 他们是否创造了新的元素(抽象), 从我们模仿其意义的角度来看, 如果需要添加新元素加以标识的, 就是伪元素, 反之, 如果只需要在既有元素上添加类别的, 就是伪类.


### 4.HTML文档流的排版规则，CSS几种定位的规则、定位参照物、对文档流的影响，如何选择最好的定位方式，雪碧图实现原理


### 5.水平垂直居中的方案、可以实现6种以上并对比它们的优缺点


### 6.BFC实现原理，可以解决的问题，如何创建BFC


### 7.可使用CSS函数复用代码，实现特殊效果


### 8.PostCSS、Sass、Less的异同，以及使用配置，至少掌握一种


### 9.CSS模块化方案、如何配置按需加载、如何防止CSS阻塞渲染


### 10.熟练使用CSS实现常见动画，如渐变、移动、旋转、缩放等等


### 11.CSS浏览器兼容性写法，了解不同API在不同浏览器下的兼容性情况


### 12.掌握一套完整的响应式布局方案

## 手写(1.5)

### 1.手写图片瀑布流效果

### 2.使用CSS绘制几何图形（圆形、三角形、扇形、菱形等）

### 3.使用纯CSS实现曲线运动（贝塞尔曲线）

### 4.实现常用布局（三栏、圣杯、双飞翼、吸顶），可是说出多种方式并理解其优缺点





