---
category: 面试题
tags:
  - css
date: 2023-4-3 
title: 垂直居中的方式
vssue-title: Hello, world!
---
## 问题
说出几种显示垂直水平居中的方法
## 回答
1. 简单flex:
  一对嵌套块元素中，将父元素的display设置为flex，子元素的margin设置为auto。
2. flex:
父元素的display设置为flex，justify-content 和 align-items 都设置为center 

3. absolute + transform: 父元素的position设置为relative，子元素的position设置为absolute，top和left设置为50%，transform设置为translate(-50%,-50%)



## 代码和原理

```HTML
<div class="outer">
  <div class="inner"></div>
</div>
```
1. 
```css
.outer{
  height: 100%;
  width: 100%;
  display: flex;
}
.inner{
  height: 40vh;
  width: 40vw;
  margin: auto;
  background-color: red;
}
```
> flex 格式化上下文中，设置了 margin: auto 的元素，在通过 justify-content 和 align-self 进行对齐之前，任何正处于空闲的空间都会分配到该方向的自动 margin 中去

2. 
```css
.outer{
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.inner{
  height:40vh;
  width: 40vw;
  background-color: red;
}
```
> justify-content 和 aligh-items

3.

```css
.outer{
  height: 100%;
  width: 100%;
  position: relative;
}
.inner{
  height: 40vh;
  width: 40vw;
  background-color: red;

  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```
> 定位中，top和left设置百分比是以父元素为基准的，而transform属性的translate设置百分比是以子元素的宽和高为基准的
