---
category: 面试题    
tags:
  - css
date: 2023-4-3  
title: 经典布局
---

## 问题
你用过哪几种布局？怎么实现

## 回答
1. 双飞翼布局：
   - 布局效果是左中右三列，左右是固定宽度，中间自适应。
   - 结构是，左中右三列容器，中间的容器有一个子容器，
   - 样式为，首先将左中右的float属性都设为left，将中间宽度设置为100%，左右div都设置为固定宽度（如200px），将左div的margin-left设置为-100%，右div的margin-left设置为-200px，再把中间容器的子容器的左右外边距设置为200px

2. 圣杯布局
   - 布局效果：左中右三列，左右是固定宽度，中间自适应。
   - 结构，左中右三列容器
   - 实现方式：左中右列属性都设置为float，中间列设置为100%，左右两列设置为定宽200px，将父容器的左右内边距值设为200px，将左右列的定位设置为relative，左列marginleft设置为-100%，left设置为-200px，右列marginleft设置为-200px，right属性设置为-200px。

- 圣杯布局与双飞翼布局的不同之处，圣杯布局的的左中右三列容器没有多余子容器存在，通过控制父元素的 padding 空出左右两列的宽度。

## 代码和原理
1. 双飞翼
```css 
.container{
  min-width: 400px;
  height: 100%;
  & > div{
    float:left;
    height:100%;
  }
}
.middle{
  width: 100%;
  .middle-inner{
    margin: 0 200px;
  }
}
.left{
  width: 200px;
  background-color: orangered;
  margin-left: -100%;
}
.right{
  width: 200px;
  background-color: orange;
  margin-left: -200px;
}
.one{
  float: left;
  width: 200px;
  height: 200px;
  background-color: red;
}
```
> left,right,middle本来应该是在一条直线上的，但是content的宽度不够，就把left，right两个盒子挤下来啦 但是他们的十几位置还是在middle右边的。 通过margin-left 移动一个middle的宽度讲他的位置移到了content容我的最左端，left移走 此时right就来到了原来left的位置 我们也就给他设置一个margin-left -200px 这样right来到content盒子的最右边


2. 圣杯布局
```css
.s-container{
  min-width: 400px;
  height: 500px;
  padding: 0 200px;
  div{
    position: relative;
    float:left;
    height:500px;
  }
}

.s-middle{
  width: 100%;
}
.s-left{
  width: 200px;
  background-color: blue;
  margin-left: -100%;
  left: -200px;
}
.s-right{
  width: 200px;
  margin-left: -200px;
  background-color: darkblue;
  right: -200px;
}
```
> margin属性%值是以父元素的width属性为基准的。


### 一个margin为负值的问题

`It is possible to give margins a negative value. This allows you to draw the element closer to its top or left neighbour, or draw its right and bottom neighbour closer to it.` -- [Negative margins in CSS](https://www.quirksmode.org/blog/archives/2020/02/negative_margin.html)

1. top和left设置为负，自身会移动，
2. bottom和right设置为负，邻居会移动




