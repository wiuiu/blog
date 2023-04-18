---
category: 面试题
tags:
  - js
date: 2023-4-
title: js实现继承的几种方式
---

## 问题 
JavaScript有哪些实现继承的方式

## 回答

### 前置知识：
关于JavaScript对象属性的设置和屏蔽
在设置一个js对象的属性时，`obj.a = "xxx"`
有下列几种情况（前提是obj的属性a是可写的（writable=true））
1. obj对象存在该属性，那就直接设置该属性
2. obj对象不存在该属性
   1.  原型链也不存在该属性   
      那就在obj上设置该属性
   2.  原型链存在该属性  
      还是在obj上设置该属性（主要是这种情况，之前一直以为会重新设置obj原型上的属性）

> 详细参考于：[JavaScript——对象的属性设置和屏蔽](https://zhuanlan.zhihu.com/p/38098272)

### 第一类
#### 利用原型链
将父类的实例对象作为子类构造函数的原型。这样通过子类构造函数创建的新对象的原型就是父类的实例对象，利用原型链，子类实例就继承了父类实例上的属性和方法以及父类原型链上的属性和方法。
- 缺点
  主要问题出现在，原型中包含引用类型数据时，在利用原型实现继承时，父类构造函数中给实例添加的属性，变成了子类实例的原型属性，这样就导致了一个问题：如果这个属性是引用类型的话，子类的所有实例都会共享这个原型属性，一个实例对其进行修改，所有实例的属性都会收到影响。

#### 盗用构造函数
在子类的构造函数中，使用call方法，执行父类的构造函数，这样父类的构造函数中的this指向了子类的实例，子类实例就得到了父类实例的属性和方法。
- 缺点
  1. 子类只能继承父类的属性和方法，不能继承父类实例的原型属性和方法
  2. 子类想要继承的方法，只能在父类构造函数中定义，不能在父类的原型上定义，所以函数不能被重用。

---
（没有致命缺点）
#### 组合继承
综合了原型链和盗用构造函数，思路是利用原型链继承父类的原型上的属性和方法，利用盗用构造函数继承父类实例属性。  
这样既可以将方法定义到父类原型上，以实现函数的重用；又可以让每个实例都有各自的父类实例的属性。

- 效率问题
  父类的构造函数执行了两次；子类通过盗用构造函数得到的父类实例属性从而覆盖了其原型（父类实例）上的实例属性，
  这样产生了两组父类的实例属性。

```javascript
function Parent3 () {
    this.name = 'parent3';
    this.play = [1, 2, 3];
}

function Child3() {
    // 第二次调用 Parent3()
    Parent3.call(this);     // 盗用父类构造函数继承父类属性
    this.type = 'child3';
}

// 第一次调用 Parent3()
Child3.prototype = new Parent3(); // 利用原型链，继承父类实例及其原型链
```

### 第二类
#### 原型式继承
> 前置知识： Object.create()方法  
> 该方法有两个参数，第二个参数可选，`Object.create(proto, [propertiesObject])`  
> 返回一个新对象，使用现有的对象(第一个参数proto)来作为新创建对象的原型  
> 仅考虑有第一个参数的情况，大概就是做了这件事：  
> ```javascript
> function create(proto){
>   function Son(){}
>   Son.prototype = proto
>   return new Son()
> }
> ```
> 第二个参数和Object.defineProperties()的第二个参数一样，以属性描述对象的方式添加的对象，会屏蔽原型上的同名属性
> 参考mdn: [Object.create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

利用Object.create(),该方法返回一个新对象，将传入对象作为新对象的原型。
该继承方式适用于不需要单独创建构造函数，但是仍然想要在对象之间共享信息的情况。

#### 寄生式继承
利用工厂模式，在Oject.create()继承对象的基础上，在新对象上增添属性方法，返回新对象实现继承。

### 寄生式组合继承
子类构造函数中通过盗用父类构造函数，实现对父类属性的继承；  
再通过寄生式继承实现对父类原型链的继承。
```javascript
function clone (parent, child) {
    // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
    child.prototype = Object.create(parent.prototype);
    child.prototype.constructor = child;
}

function Parent6() {
    this.name = 'parent6';
    this.play = [1, 2, 3];
}

function Child6() {
    Parent6.call(this); //盗用构造函数继承父类属性
    this.friends = 'child5';
}

clone(Parent6, Child6);   // 寄生式继承实现对父类原型链的继承

```