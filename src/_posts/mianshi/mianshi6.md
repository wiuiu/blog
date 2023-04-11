---
category: 面试题    
tags:
  - js
date: 2023-4-10
title: js的浅拷贝和深拷贝
---
## 问题
介绍下js的浅拷贝和深拷贝

## 回答

### 概念
- 浅拷贝，在拷贝一个数据对象时，对于他的基本数据类型的属性，拷贝的就是该基本数据类型的值，但是对于引用数据类型的属性，拷贝的只是他的内存地址。所以浅拷贝中，拷贝出的新对象和原对象只有基本数据类型是分离的，引用类型的指向还是同一处内存空间。
- 深拷贝，在拷贝一个数据对象时，对于引用数据类型的属性，会在堆上重新开辟个内存空间，对引用类型指向的对象进行拷贝。所以深拷贝中，拷贝出来的对象和原对象是完全分离的，他们的引用类型属性指向的是地址不同的两个内存空间。


### 具体介绍

1. 浅拷贝
    一些js的方法就是浅拷贝的，比如：
    - Object.assign()
    - 数组的slice和concat方法


2. 深拷贝
深拷贝的实现方式，3种方式，
- 可以使用已有的一些库的方法，比如jquery和lodash都有实现深拷贝的方法。
- 使用JSON的stringify和parse方法  
    `const newObj = JSON.parse(JSON.stringify(obj))`
    缺点是，undefined symbol 和 函数会被忽略活着转化为null；同时如果存在循环引用，stringify方法会抛出错误
- 自己实现一个深拷贝的方法，实现的具体思路
>  在递归函数中定义newObj变量
   递归函数的出口是：使用typeof判断函数的参数（也就是要被拷贝的数据）是否是Object类型，如果不是就直接返回该数值。
   第一步判断参数是否是数组，如果是就将newObj赋值为一个空数组，不是就赋值为一个空对象
   接着利用for in循环，遍历函数参数的每一个属性，在循环体内部，利用hasOwnProperty方法判断该属性是否是自身的属性，如果是就将该属性值作为参数递归调用深拷贝函数，将结果赋值给newObj的该属性。
   通过这样的循环递归调用就实现了深拷贝。
   ---
   这个是比较简易的实现思路，有bug：如果遇到循环调用，会导致栈溢出
   还有一个更完善的实现思路，利用hashmap（对应js中的set类型）。具体运行的原理还没搞懂。。

### 不同
不同的就是，浅拷贝和深拷贝对于引用数据类型拷贝的处理上，浅拷贝只会拷贝内存地址，深拷贝会对引用数据类型指向的实际对象进行拷贝。

---
---
## 代码
### slice 浅拷贝
```javascript
let arr = [0,1, {
    a:1,
    b:2,
}]
let arrslice = arr.slice(0)
arrslice[0] =3
arrslice[2].a = 9
console.log(arr)        // [0,1, {a:9, b:2}]
console.log(arrslice)   // [3,1, {a:9, b:2}]
```
### concat方法 浅拷贝
```javascript
let arr = [0,1, {
    a:1,
    b:2,
}]
let arrslice = arr.concat()
arrslice[0] =3
arrslice[2].a = 9
console.log(arr)        // [0,1, {a:9, b:2}]
console.log(arrslice)   // [3,1, {a:9, b:2}]
```

### JSON 实现深拷贝

```javascript
let obj = {
    a:1,
    b: undefined,
    c:Symbol('属性c'),
    d:function(){
        return true
    },
    e:{
        eproperty:"e",
    }
}
let newObj = JSON.parse(JSON.stringify(obj))
newObj.e.eproperty = "a"
newObj.a = 3
console.log(obj)        //{ a: 1, b: undefined, c: Symbol(属性c), e: {eproperty: "e"}}, d: ƒ}
console.log(newObj)     //{ a: 3, e: {eproperty: "a"} }
```


### 手动实现深拷贝 
1. 简易版
```javascript
function deepclone(obj){
    if(!obj || typeof obj !=='object'){
        return obj
    }
    let newObj = Array.isArray(obj)? [] : {}

    for(let key in obj){
        if(obj.hasOwnProperty(key)){
            newObj[key] = deepclone(obj[key])
        }
    }
    return newObj
}
```

2. 完善版
```javascript
function deepCopy(obj, map = new Map()) {
    if (typeof obj !== 'object') {
        return obj
    }
    let res = Array.isArray(obj) ? [] : {};
    if(map.get(obj)){
        return map.get(obj);
    }
    map.set(obj, res);
    for(var i in obj){
        res[i] = deepCopy(obj[i], map);
    }
    return map.get(obj);
};
```
