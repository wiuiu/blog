---
category: 面试题    
tags:
  - js
date: 2023-4-11
title: js中new运算符
---
## 问题
说说new运算符都做了什么？

## 回答
1. 在内存中创建一个新对象，新对象的内部特性[[pototype]]（也就是新对象的原型对象）被赋值为构造函数的prototype属性。
2. 构造函数的内部this会指向这个新对象。
3. 执行构造函数内部代码，也就是给这个新对象添加属性
4. 如果构造函数返回了 非空对象，则返回该对象。否则返回创建的新对象