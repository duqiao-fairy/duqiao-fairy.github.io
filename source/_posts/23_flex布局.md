---
title: flex布局
---

[参考](https://juejin.im/post/5ac2329b6fb9a028bf057caf)

## 1. 基本概念
采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cb210abb4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
Flex项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
Flex属性分为两部分，一部分作用于容器称容器属性，另一部分作用于项目称为项目属性。下面我们就分部的来介绍它们.

## 2. 容器属性(父容器)
使用flex布局首先要设置父容器: display: flex, 然后再设置justify-content: center实现水平居中, 最后设置: align-items: center 实现垂直居中.

```javascript
  #dad {
    display: flex;
    justify-content: center; // 水平居中
    align-items: center; // 垂直居中
    flex-direction: row; // 决定主轴的方向（即项目的排列方向）
    flex-wrap: nowrap; // 是否换行排列
  }
```
![image](https://lc-gold-cdn.xitu.io/933e6f0857399ccf7e83.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.1 flex容器定义
**基本语法:**
```javascript
  .box {
    display: flex; /* 或者 inline-flex */
  }
```

### 2.2 flex-direction
**基本语法:**
```javascript
  .box {
      flex-direction: row | row-reverse | column | column-reverse;
  }
```

* row 表示从左向右排列
* row-reverse 表示从右向左排列
* column 表示从上向下排列
* column-reverse 表示从下向上排列

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cd0b76595?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.3 flex-wrap
缺省情况下，Flex项目都排在一条线（又称"轴线"）上。我们可以通过flex-wrap属性的设置，让Flex项目换行排列。

**基本语法:**
```javascript
  .box {
      flex-wrap: nowrap | wrap | wrap-reverse;
  }
```

* nowrap(缺省)：所有Flex项目单行排列
* wrap：所有Flex项目多行排列，按从上到下的顺序
* wrap-reverse：所有Flex项目多行排列，按从下到上的顺序

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cb12db292?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.4 flex-flow
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap

**基本语法:**
```javascript
  .box {
      fflex-flow: <‘flex-direction’> || <‘flex-wrap’>
  }
```

### 2.5 justify-content
justify-content属性定义了项目在主轴上的对齐方式及额外空间的分配方式。

**基本语法:**
```javascript
  .box {
      justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
  }
```
* flex-start(缺省)：从启点线开始顺序排列
* flex-end：相对终点线顺序排列
* center：居中排列
* space-between：项目均匀分布，第一项在启点线，最后一项在终点线
* space-around：项目均匀分布，每一个项目两侧有相同的留白空间，相邻项目之间的距离是两个项目之间留白的和
* space-evenly：项目均匀分布，所有项目之间及项目与边框之间距离相等

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cb20616af?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.6 align-items
align-items属性定义项目在交叉轴上的对齐方式。
**基本语法:**
```javascript
  .box {
      jalign-items: stretch | flex-start | flex-end | center | baseline;
  }
```
* stretch(缺省)：交叉轴方向拉伸显示
* flex-start：项目按交叉轴起点线对齐
* flex-end：项目按交叉轴终点线对齐
* center：交叉轴方向项目中间对齐
* baseline：交叉轴方向按第一行文字基线对齐

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cb18083e3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.7 align-content
align-content属性定义了在交叉轴方向的对齐方式及额外空间分配，类似于主轴上justify-content的作用。
**基本语法:**
```javascript
  .box {
      align-content: stretch | flex-start | flex-end | center | space-between | space-around ;
  }
```
* stretch (缺省)：拉伸显示
* flex-start：从启点线开始顺序排列
* flex-end：相对终点线顺序排列
* center：居中排列
* space-between：项目均匀分布，第一项在启点线，最后一项在终点线
* space-around：项目均匀分布，每一个项目两侧有相同的留白空间，相邻项目之间的距离是两个项目之间留白的和

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cb17eb348?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 项目属性(子容器)
### 3.1 order
缺省情况下，Flex项目是按照在代码中出现的先后顺序排列的。然而order属性可以控制项目在容器中的先后顺序。
**基本语法:**
```javascript
  .item {
    order: <integer>; /* 缺省 0 */
  }
```
按order值从小到大顺序排列，可以为负值，缺省为0。

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cd362d2ce?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3.2 flex-grow
flex-grow属性定义项目的放大比例, flex-grow值是一个单位的正整数, 表示放大的比例. 默认为0, 即如果存在额外空间, 也不放大, 负值无效.

如果所有项目的flex-grow属性都为1, 则它们将等分剩余空间(如果有的话). 如果一个项目的flex属性为2, 其他项目都为1, 则前者占据剩余空间将比其他项多一倍

**基本语法:**
```javascript
  .item {
    flex-grow: <number>; /* 缺省 0 */
  }
```

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cd4e13d3f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3.3 flex-shrink
flex-shrink属性定义了项目的缩小比例, 默认为1, 即如果空间不足, 该项目将缩小. 0表示不缩小, 负值无效.

**基本语法:**
```javascript
  .item {
    flex-shrink: <number>; /* 缺省 1 */
  }
```

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695ce473e24c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3.4 flex-basis
flex-basis属性定义项目在分配额外空间之前的缺省尺寸. 属性值可以是长度(20%, 10rem等) 或者关键字auto. 它的默认值为auto, 即项目的本来大小.

**基本语法:**
```javascript
  .item {
    flex-basis: <length> | auto; /* 缺省 auto */
  }
```

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cee00902d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3.5 flex
flex属性是flex-grow, flex-shrink和flex-basis的简写, 默认值为0 1 auto. 后两个是可选属性.

**基本语法:**
```javascript
  .item {
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
  }
```

### 3.6 align-self
align-self属性定义项目的对齐方式, 可覆盖align-item属性. 默认值为auto, 表示继承父元素的align-items属性, 如果没有父元素, 则等用于stretch

**基本语法:**
```javascript
  .item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
  }
```
除了auto值以外，align-self属性与容器的align-items属性基本一致。

![image](https://user-gold-cdn.xitu.io/2018/4/2/1628695cf0a7bb39?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


### 在主轴上如何伸缩: flex
![image](https://lc-gold-cdn.xitu.io/089d48122453e9fc372c.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

子容器是有弹性的 (flex即弹性), 它们会自动填充剩余空间, 子容器的伸缩比例由 flex 属性确定.

flex 的值可以是无单位数字（如：1, 2, 3），也可以是有单位数字（如：15px，30px，60px），还可以是 none 关键字。子容器会按照 flex 定义的尺寸比例自动伸缩，如果取值为 none 则不伸缩。
  虽然 flex 是多个属性的缩写，允许 1 - 3 个值连用，但通常用 1 个值就可以满足需求，它的全部写法可参考下图。

![image](https://lc-gold-cdn.xitu.io/78e9030183f686e0b6ed.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





