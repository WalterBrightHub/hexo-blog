---
title: 《ES6函数式编程入门经典》读书笔记
date: 2019-03-17 01:43:55
tags: 
- 函数式编程
- 入门
categories:
- 编程范式
- 函数式编程
permalink: beginning-functional-javascript
---

初入函数式编程的坑，试试这本书吧。
<!-- more -->

## 函数式编程简介

### 概念

函数的概念。函数是通过其名字调用的代码，它可以传递参数并返回值。
函数式编程主张至少接受一个参数，并且返回一个值。
和方法的区别：方法是通过其名称与关联的对象来调用。

### 函数的两个特性

+ 引用透明性。函数对相同的输入总有相同的输出。好处是并发编程、可缓存代码（如阶乘）。
+ 声明式编程。与命令式编程不同，函数式编程主张以抽象的方式创建函数。

### 函数式编程的好处

+ 纯函数。纯函数使得代码易于阅读、理解、测试。
+ 并发代码。无需担心同步问题。
+ 编写可缓存、可记忆的函数。
+ 管道与（函数式）组合。组合是函数式编程范式的核心。
+ 纯函数是数学函数，可以将数学思想引入编程思想。

## JavaScript函数基础

书中的Node还不支持ES6，故用babel编译运行。
如今Node已经支持ES6，直接运行JS文件即可。

+ forEach(array,fn)

对数组的每一个元素调用fn

## 高阶函数

### 理解数据

JavaScript数据类型

+ Number
+ String
+ Boolean
+ Object
+ null
+ undefined

函数是一等公民，可以赋值给变量、作为函数参数、作为函数返回值。

### 抽象和高阶函数

抽象：隐藏实现细节，专注于实现，降低复杂度。
高阶函数实现抽象

+ forEach(array,fn)
+ forEachObject(object,fn)
+ unless(predicate,fn)
+ times(times,fn)
+ every(array,fn)
+ some(array,fn)
+ sort

## 闭包与高阶函数

### 闭包函数

闭包是一个函数内部的另一个函数。它有三个可访问作用域：

+ 本身内声明的变量
+ 全局变量
+ 外部函数定义的变量

使用闭包构建高阶函数：

+ tap
接受一个value，返回一个包含value的闭包函数。
+ unary
类似于适配器模式。
+ once
只执行一次给定的函数。
+ memoized

## 数组的函数式编程

### 投影函数

+ map
+ filter

### 连接函数

连接map和filter，最简单的方法是直接嵌套。
concatAll是一个辅助函数，辅助函数之间的连接。

+ concatAll(array,fn)
将嵌套（一层的）数组转化为非嵌套的单一数组。

### reduce

+ reduce(array,fn,initValue)
将数组归约成一个单一的值。

实现

```js
const reduce=(array,fn,indexValue)=>{
  if(indexValue){
    let result=indexValue
    for(let i=0;i<array.length;i++){
      result=fn(result,array[i])
    }
    return result
  }
  else{
    let result=array[0]
    for(let i=1;i<array.length;i++){
      result=fn(result,array[i])
    }
    return result
  }
};
```

### zip

+ zip(leftArr,rightArr,fn)

## 柯里化与偏应用

预备知识：一元函数、二元函数、变参函数。

概念：柯里化是将一个多参函数转化为嵌套的一元函数的过程。

```js
const curry=fn=>{
  if(typeof fn !=='function'){
    throw new Error('no function provided')
  }
  else{
    return function curriedFn(...args){
      if(args.length<fn.length){
        console.log(args)
        return function(){
          console.log(arguments)
          return curriedFn.apply(null,args.concat([].slice.call(arguments)))
        }
      }
      else{
        console.log(args)
        return fn.apply(null,args)
      }
    }
  }
}
```

实例

```js
let add=(a,b,c)=>a+b+c
let curriedAdd=curry(add)
curry(add)(3)(7)(5)
```

### 偏函数

偏函数用于创建创建可重用的函数，而不需要自己写包装器。

```js
const partial=(fn,...partialArgs)=>{
  let args=partialArgs
  return (...fullArguments)=>{
    let arg=0
    let fullArgs=[]
    for(let i=0;i<args.length && arg<fullArguments.length;i++){
      if(args[i]===undefined){
        fullArgs.push(fullArguments[arg++])
      }
      else{
        fullArgs.push(args[i])
      }
    }
    return fn.apply(null,fullArgs)
  }
}
```

实例

```js
//不需要每次写这样的包装器了
//let ugly=fn=>setTimeout(fn,10)

let delayTenMs=partial(setTimeout,undefined,10)

delayTenMs(()=>{
  console.log('hello world')
})

delayTenMs(()=>{
  console.log('goodbyle world')
})
```

## 组合与管道

### 组合

概念：以一个函数的输出作为另一个函数的输入的方式，将多个函数结合起来。

### compose

+ compose(...fns)
组合多个函数，数据流是从右至左的。
+ pipe(...fns)
数据流是从左至右的，管道pipeline也可以称作序列sequence。
实现上两者只有很小的不同。

实现

```js
const compose=(...fns)=>
  (value)=>
    reduce(fns.reverse(),(acc,fn)=>fn(acc),value)
const pipe=(...fns)=>
  (value)=>
    reduce(fns,(acc,fn)=>fn(acc),value)
```

如何组合多参函数？使用柯里化和偏应用即可。
实例

```js
let add=(x,y)=>x+y

let div=(x,y)=>x/y

let ac=compose(curry(div)(50),curry(add)(4))

// 50/(4+x)
console.log(ac(3))
```

### 组合的优势

组合符合结合律。

### 组合的应用

方便地log。创建identity函数，在数据流中随意的插入或移除，方便调试。

```js
const identity=(it)=>{
  console.log(it)
  return it
}
```

## 函子

函子是一个对象或者类，它实现了map函数，在遍历每个对象值的时候生成新的对象。
简单的说，函子是一个包含值的容器，并且实现了map函数。
实现

```js
class Container{
  constructor(value){
    this.value=value
  }

  static of(value){
    return new Container(value)
  }

  map(fn){
    return Container.of(fn(this.value))
  }
}
```

### Maybe

该函子会检查传入的值是否为null或者undefined，如果是则什么也不做。
该函子用于错误处理。
实现

```js
class Maybe {
  constructor(value){
    this.value=value
  }

  static of(value){
    return new Maybe(value)
  }

  isNothing(){
    return this.value===null || this.value===undefined
  }

  map(fn){
    return this.isNothing()?Maybe.of(null):Maybe.of(fn(this.value))
  }
}
```

### Either

Either用于解决分支拓展问题。

```js
class Nothing{
  constructor(value){
    this.value=value
  }

  static of(value){
    return new Nothing(value)
  }

  map(){
    return this
  }
}

class Some{
  constructor(value){
    this.value=value
  }

  static of(value){
    return new Some(value)
  }

  map(fn){
    return Some.of(fn(this.value))
  }
}

const Either={
  Nothing:Nothing,
  Some:Some
}
```

使用Either

```js
let summary=data=>data.reduce((sum,val)=>sum+val,0)

let cal=data=>{
  let sum
  try{
    sum=Either.Some.of(summary(data))
  }
  catch(e){
    sum=Either.Nothing.of({
      msg:'something has wrong.'
    })
  }
  return sum
}

console.log(cal(5))
//Nothing { value: { msg: 'something has wrong.' } }

console.log(cal([5]))
//Some { value: 5 }
```

## Monad

Monad是一个实现chain方法的函子。
join方法返回函子包含的值。
chain方法包装了map和join方法。
实现

```js
class Maybe {
  constructor(value){
    this.value=value
  }

  static of(value){
    return new Maybe(value)
  }

  isNothing(){
    return this.value===null || this.value===undefined
  }

  join(){
    return this.isNothing()?Maybe.of(null):this.value
  }

  chain(fn){
    return this.map(fn).join()
  }

  map(fn){
    return this.isNothing()?Maybe.of(null):Maybe.of(fn(this.value))
  }
}
```

使用（书上的示例太复杂，写个简单的）

```js
let bro=Maybe.of({
  name:'xhb',
  age:5
})

//我们来模仿一个场景！首先我们给小熊加一岁。
let newBro=bro.map(bear=>{
  return {
    ...bear,
    age:bear.age+1
  }
})

//我们继续处理小熊。
//为了方便，我们直接输出小熊。
newBro.map(bear=>{
  console.log(bear)
})
//天啊！这样太麻烦了。为了输出小熊我们不得不调用map函数。
//关键是，此刻我们关心的是bear，而不是包装了bear的Maybe

//可以这样
let bigBear=bro.map(bear=>{
  return {
    ...bear,
    age:bear.age+2
  }
}).join()

console.log(bigBear)
//顺利取出了结果！

//每次都要写map和join，不如封装到一起
let biggerBear=bro.chain(bear=>{
  return {
    ...bear,
    age:bear.age+3
  }
})
console.log(biggerBear)
//真好
```
