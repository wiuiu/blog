---
category: 面试题    
tags:
  - css
date: 2023-4-7
title: 盒子模型
---

## 问题
介绍一下盒子模型
## 回答
对文档进行布局的时候，浏览器会根据css盒子模型标准，将所有元素都表示为一个矩形盒子
这个矩形盒子有四部分组成：
1. content内容
2. padding内边距
3. border边框
4. margin外边距
有两种盒子模型：标准盒子模型和IE盒子模型
1. 标准盒子模型的width宽度属性是以content来计算的
2. IE盒子模型的width宽度属性是以content+padding+border来计算的

css中通过box-sizing属性来确定盒子模型的宽度计算方式，这个属性有两个值，一个是content-box，仅计算content的宽度值，另一个是box-sizing，是计算content+padding+border的宽度值。
