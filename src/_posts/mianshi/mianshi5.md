---
category: 面试题    
tags:
  - css
date: 2023-4-10
title: css定位属性
---


## 问题
介绍下position属性，他们的值之间的不同

## 回答
position属性这几个值分别是：
static(默认值) relative absolute fixed sticky

- static是默认值，不脱离文档流，top left等和z-index属性不起作用
- relative不脱离文档流（所以offset不会影响其他元素的位置情况，页面布局就像默认值static一样），相对于自身进行定位
- absolute直接脱离文档流，相对于离自己最近的一个position值不是static的祖先为基准进行定位
- fixed也会脱离文档流，一般相对于浏览器视图窗口定位
- sticky 当处于视图窗口中时，其显示效果和默认static一样，但是当滚动出视图窗口时，其显示特性和fixed一样，根据设置的top，left等属性相对于浏览器视图窗口定位。





## 其他
关于文档流，还是通读一遍css世界


