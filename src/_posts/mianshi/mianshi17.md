---
category: 面试题
tags:
  - js
date: 2023-4-16
title: 关于call,apply,bind方法
---

> 函数本质是一个对象，所以也有他自己的属性和方法，方法有（call，apply和bind）。同时函数也有一个内置对象this，在运行时决定this的指向：this的指向有下列几种情况：
> 全局环境 ➡️ window
> 普通函数 ➡️ window 或 undefined(严格模式下)
> 构造函数 ➡️ 构造出来的实例
> 箭头函数 ➡️ 定义时外层作用域中的 this
> 对象的方法 ➡️ 该对象
> call()、apply()、bind() ➡️ 第一个参数
> [js函数的this指向](https://juejin.cn/post/6982597927269040135/)

call,apply,bind都是用于改变函数运行时的this的指向。

## call 与 apply
call与apply方法调用后会直接调用函数运行，且改变的this指向仅在该次运行中起作用。</br>
call与apply方法只有一个不同点，就是参数的传递上：
1. apply只接受两个参数，第一个参数是this的指向对象，第二个参数就是函数接受的参数。
2. call接受多个参数，第一个参数是this的指向对象，后边传入参数列表（多个参数）

## bind
想要手写bind，首先要理解柯里化，并可以自己写出柯里化的转换函数
> [函数的柯里化](https://zhuanlan.zhihu.com/p/120735088)
bind的参数和call一样，但是bind与 `call 和 apply` 是不同的，bind返回一个函数，而不是立刻执行目标函数，同时bind实现了函数柯里化，可以分批传入参数。</br>
bind的实现

> 如果不传入第一个参数，或者传入的是null或undefined，函数的this会指向window（非严格模式下），严格模式就是undefined 或null
