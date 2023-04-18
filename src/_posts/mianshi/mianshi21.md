---
category: 面试题    
tags:
  - js
date: 2023-4-17
title: instanceof与typeof的不同
---

## 问题
instanceof和typeof原理和不同

## 回答
### instanceof
instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上
实现原理，设运算符左边的实例对象为obj，运算符右边的构造函数为Constructor  
instanceof会沿着obj的原型链，查找其原型对象是否是Constructor的prototype属性指向的对象，如果是就返回true；如果直到原型链的末尾，也就是原型对象为null时，就返回false
