---
category: 面试题    
tags:
  - js
date: 2023-4-
title: js数组方法
---

## 问题 
你都用过哪些数组的方法

## 回答
数组的方法从遍历、增删改查几个方面来描述
1. 增
- push()-返回值：新的数组长度。接受任意数量的参数，讲这些参数添加到数组末尾
- unshift()-返回值：新的数组长度。接受任意数量参数，将这些参数添加到数组开头
- concat()-[*不改变原数组*]，返回值：新的数组。接受任意数量的参数，先创建一个原数组的副本（浅拷贝），再把所有参数添加到副本的末尾，最后返回新构建的数组。
2. 删
- pop()-返回值：被删除的项。删除数组最后一项
- shift()-返回值：被删除的项。删除数组最后一项
- slice()-[*不改变原数组*]，返回值：包含所有删除元素的数组。参数是（startIndex，endIndex），前闭后开。将原数组的开闭区间内的元素进行浅拷贝返回新的数组。
3. 改
splice(),比较特殊，该方法可以实现增、删、改三种操作。
[*改变原数组*]
参数格式：splice(startindex, [deleteCount, item1, item2])    [可选参数]
返回值是，删除的元素组成的数组。
- 参数只有 startIndex，则删除startIndex（包括startIndex）后的所有元素
- 实现增 splice(startIndex, 0, items...)
- 实现删 splice(startIndex, deleteCount)
- 实现改 splice(startIndex, deleteCount, items...)

startIndex: 如果startIndex超出数组长，则为数组末尾开始。如果为负值则倒着数。如果负值且超出数组长度，则表示为数组第0位

> [splice-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

1. 查
- indexOf()-返回值：查找元素的下标，如果没有该元素，返回-1。参数是要查找的元素
- includes()-返回值：true/false。参数是要查找的元素
- find()-返回值：第一个回调函数返回true的元素。参数是回调函数（item, index, array）
1. 遍历
> 遍历方法都不会改变原数组. 参数都是一个回调函数，函数格式为 (item, index, array) => {}
- some()-返回值：true/false，如果至少有一个元素使回调函数返回true。这个方法就会返回true
- every()-返回值：true/false，如果所有的元素都使回调函数返回true。这个方法就返回true
- forEach()-无返回值，对每一个元素执行回调函数
  > forEach的回调函数体内，可以修改引用类型的属性，不可以修改基本数据类型（或者直接改变引用对象）。
- filter()-返回值：数组，将所有回调函数返回true的元素组成一个新数组返回
- map()-返回值：数组，将所有回调函数的返回值组成一个新数组返回

1. 排序
2. 转换



