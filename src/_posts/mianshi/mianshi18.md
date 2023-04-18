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
> 深拷贝函数的参数有两个，一个是待拷贝的对象，一个是哈希表（js中的map数据类型）
> 首先就是排除特殊情况，1. 内置数据类型Date或RegExp，返回创建的他们的新实例。2. 就是非对象类型（用typeof 来判断是否是"object"），值类型或者是函数，直接返回。
> 接着如果是一个对象，判断哈希表中是否有该对象的key，如果有就返回对应的value
>
> 先抓主干，再探究细节
> 主干就是：设计一个深拷贝函数，函数中遍历拷贝对象的每一个属性，如果是对象类型就递归调用该深拷贝函数。
> 递归函数的出口是 拷贝的数据不是对象类型，是个普通值或者是函数，就直接返回他的值。
> 补充说明：这样的设计有一个问题，就是无法解决循环引用。（因为没有循环引用的出口，循环引用会导致拷贝对象无限判断是对象类型，前文说是对象类型就递归调用，这样就导致了无限递归调用）
> 如何解决：利用哈希表，在深拷贝的函数参数中添加一个哈希表，这个哈希表的key存储待拷贝对象（地址值），value存储新创建的对象（地址值）
> 在每次判断待拷贝对象是否是对象类型前，先判断他是否在哈希表中有对应的键，如果有，就直接返回该键的值，没有就将该待拷贝的对象和新创建的对象设置为哈希表的键值对。

多看多理解这段代码。核心： 哈希表中存储的是指向对象实例的指针（地址） 
```javascript
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  //特殊情况，js内置引用类型Date和RegExp，创建一个新实例
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 如果是普通的值或者是函数，就直接返回值（虽然函数本质就是对象，但是typeof 函数 不等于 "object"）
  if (typeof obj !== "object") return obj;

  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj); //这返回的是对象吗，实际上返回的不就是指向对象实例的指针吗
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  // 别把这俩变量当做对象实例了，他俩只是存储对象实例的内存地址罢了。 这样就好理解一些了。
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash); 
      // 注意哈希表里存的是新对象和拷贝对象的地址，所以只要新的对象实例上有新增属性，哈希表里的新对象就会发生改变
    }
  }
  return cloneObj;
}
```



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
1. 简易版（不能解决循环引用的问题）
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
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  //特殊情况，js内置引用类型Date和RegExp，创建一个新实例
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 如果是普通的值或者是函数，就直接返回值（虽然函数本质就是对象，但是typeof 函数 不等于 "object"）
  if (typeof obj !== "object") return obj;

  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj); //这返回的是对象吗，实际上返回的不就是指向对象实例的指针吗
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  // 别把这俩变量当做对象实例了，他俩只是存储对象实例的内存地址罢了。 这样就好理解一些了。
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash); 
      // 注意哈希表里存的是新对象和拷贝对象的地址，所以只要新的对象实例上有新增属性，哈希表里的新对象就会发生改变
    }
  }
  return cloneObj;
}
```
