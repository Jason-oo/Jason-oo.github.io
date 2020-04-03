---
title: js中一些常用函数的实现
date: 2020-04-03 09:51:47
tags:
---

`bind`、`call`、`apply`是js中常用的改变函数调用时this指向的方法，他们的主要区别在于传参
除了第一个对象参数外，`call`可以接收一个参数列表，`apply`接收一个数组，`bind`返回的是一个函数，不会立即执行，可用于实现柯里化。

### 实现一个bind函数

```js
Function.prototype.myBind = function(ctx){
  if(typeof this !== 'function'){
    throw new TypeError('Error')
  } // 非函数调用报错
  let self = this
  const args = [...arguments].slice(1)
  return function() {
    self.apply(ctx,args.concat([...arguments]))
  }
}
```

### 实现一个call函数
```js
Function.prototype.myCall = function(ctx){
  if(typeof this !== 'function'){
    throw new TypeError('Error')
  } // 非函数调用报错
  ctx = ctx || Window
  ctx.fn = this
  const args = [...arguments].slice(1)
  const res = ctx.fn(...args)
  delete ctx.fn
  return res
}
```

### 实现一个bind函数
```js
Function.prototype.myBind = function(ctx){
  if(typeof this !== 'function'){
    throw new TypeError('Error')
  } // 非函数调用报错
  ctx = ctx || Window
  ctx.fn = this
  let res
  if(arguments[1]){
    res = ctx.fn(...arguments[1])
  }else{
    res = ctx.fn()
  }
  delete ctx.fn
  return res 
}
```