---
category: 面试题    
tags:
  - css
date: 2023-4-5  
title: flex属性
---

## 问题
介绍一下flex布局

## 回答
### 基本概念
使用flex布局的元素，为flex容器，他的子元素成为容器成员。容器中，默认存在两个轴，主轴和副轴，互相垂直，容器成员自动以主轴方向排列
### 相关属性
与flex布局相关的属性可以分为 flex容器想关的属性，以及容器成员相关的属性
#### 6个flex容器相关的属性
  1. flex-direction 可以设置主轴方向（row，column，row-reverse，column-reverse）
  2. justify-content 可以设置主轴方向上的排列方式，（flex-start，flex-end，space-around，space-between）
  3. align-items 可以设置副轴方向的排列（flex-start，flex-end，center，baseline，stretch）
  4. flex-wrap 设置如果容器成员默认沿主轴方向一行排列（一条轴线），如果成员超出了主轴的长度，设置成员是否换行 2
  5. align-content 在有多条主轴时(也就是容器成员出现了换行)，设置每条主轴的排列方式，（主轴的排列方式自然是副轴方向的）
  6. flex-flow 为flex-direction与flex-wrap组成的复合属性，默认值是：row nowrap
  
#### 6个容器成员的属性
  > 容器宽度和成员宽度总和的关系有以下三种
  > 容器宽度 > 成员宽度总和 flex-grow
  > 容器宽度 = 成员宽度总和
  > 容器宽度 <br 成员宽度总和 flex-shrink
  > 首先应明确，在flex容器宽度和成员宽度的总和相等的情况下，flex-grow和flex-shrink属性都不会起作用

  1. flex-grow 只有在flex容器宽度大于成员宽度总和时起作用，该属性决定每个容器成员如何瓜分flex容器多出来的宽度。默认值为0，也就是宽度保持不变。

  2. flex-shrink 只有在flex容器宽度小于成员宽度总和时起作用，该属性决定每个容器成员该如何进行缩放以适应较小的容器宽度。默认值为0，也就是宽度不缩放。

  3. flex-basis
    有俩值（<length> auto）
    浏览器是根据这个属性值（而不是width值）来计算主轴方向是否有多余空间的。</br>
    默认值是auto，也就是width属性的值，如果该成员没有设置width，那么这个值根据成员的内容宽度来设置。
    如果设置了长度值，那么width就是不起作用的。
    flex伸缩值（grow或者shrink）计算的基准都是以flex-basis来的。而不是width（ps：那后边的有关flex-grow和flex-shrink计算方法中的width都应该替换为flex-basis才对，只不过是flex-basis默人auto就是width的值，所以他们值是一样的）
    特殊情况，flex-basis值为0，根据第一句话“浏览器是根据这个属性值（而不是width值）来计算主轴方向是否有多余空间的”。这时候，主轴方向的宽度全都是剩余空间，所以成员的宽就完全由成员的flex-grow的比例来决定了。
    对于flex-shrink的计算方式还不太确定，最后

  4. flex 
    flex是（按顺序）flex-grow flex-shrink flex-basis 三个属性的复合属性。默认值为（0 1 auto）
    > 常用的值</br>
    > flex: 1;  ==> flex: 1 1 0
    > flex: auto; ==> flex: 1 1 auto
    > flex: none; ==> flex: 0 0 auto
  5. align-self 设置该值会覆盖容器的align-items属性，允许该成员单独进行排列。（flex-start，flex-end，center，baseline，stretch）
  6. order 设置成员在主轴方向上的排列顺序，（数值越小排列越靠前），默认值为0
### 应用场景
可以很容易的实现垂直居中，同时一些自适应的布局也可以很容易的使用flex来实现


## 原理和例子
```HTML
<div class="container">
    <div class="item one">
        item_one
    </div>
    <div class="item two">
        item_two
    </div>
</div>
```
1. flex-grow的计算方式
以下边的例子，每个成员多出来的宽度的计算方式如下，
```css
.container{
    width: 400px;
    height: 400px;
    display: flex;
  .item{       
    height: 100px;
    background-color: red;
  } 
  .one{
    width: 100px;
    flex-grow: 2;
  }
  .two{
    width:200px
    flex-grow: 1;
  }
}
```
flex容器宽度为400px,成员宽度总和为300px，则容器多出的宽度为100px。</br>
而成员的flex-grow的总和为3，则有：</br>
成员one分配的多余宽度为 （100px / 3）* 2 = 66.666。 最终宽度为100+66.666 = 166.666px</br>
成员two分配的多余宽度为 （100px / 3）* 1 = 33.333。 最终宽度为200+33.333 = 233.333px</br>

2. flex-shrink的计算方式，相比于flex-grow复杂一些
```css
.container{
    width: 200px;
    height: 400px;
    display: flex;
  .item{       
    height: 100px;
    background-color: red;
  } 
  .one{
    width: 100px;
    flex-shrink:2
  }
  .two{
    width:200px
    flex-shrink: 1;
  }
}
```
每个成员减少宽度的计算方式：</br>
flex容器宽度为200px，成员总宽度为300px。所以差值为100px</br>
flex-shrink每个成员减少宽度的权重计算方式是：flex-shrink * width</br>
所以本例子中 one的权重为 100 * 2 = 200</br>
two的权重为 200 * 1 = 200。总权重为200+200=400</br>
故one减少的宽度为  （200/400）* 100px = 50px。最终宽度为 100 - 50 = 50px</br>
同理two减少的宽度也为50px。最终宽度为 200 - 50 = 150px

> 好像计算方式不正确，和浏览器实际显示的结果不一样。



## ref
> - [面试官：说说flexbox（弹性盒布局模型）,以及适用场景？](https://vue3js.cn/interview/css/flexbox.html#%E4%BA%8C%E3%80%81%E5%B1%9E%E6%80%A7)
> - [详解 flex-grow 与 flex-shrink](https://zhuanlan.zhihu.com/p/24372279)