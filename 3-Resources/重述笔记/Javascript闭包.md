---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---
```javascript
function fn1(){
	var a = 1
	function fn2(){
		return a
	}
	console.dir(fn2)
	return fn2
}

var f = fn1() 
console.log(f())
```

## 闭包产生的原因
- 函数内部可以直接读取全局变量。
- 函数外部无法读取函数内部声明的布局变量
- 需要有一种办法使得我们可以让我们能在函数外部读取函数内部的局部变量

## 闭包形成所需要具备的条件

1. 嵌套函数
2. 内部函数要使用外部函数的变量(函数)
3. 要在外部函数中使用(返回)内部函数
4. 要调用外部函数(要执行内部函数的声明)，内部函数并不是必须要调用


## 调试闭包

函数变量的声明在预处理阶段就完成了，无法对其打断点

打断点的代码是未执行的

## 面向对象
### this指向,call apply , bind
### 继承

## 数据的高级方法
## 防抖

## 节流
## Ajax

## Promise
异步和回调之间的关系
应用场景
Promise处理异步任务
### 回调函数的分类
1. 同步回调
2. 异步回调

### 错误类型

### 错误处理
try catch throw

### 基础概念
Promise链式调用，以及async,await可以终结回调地狱，
promise.then 执行异步函数，本身就是异步的
.then返回的是一个新的promise对象，可以进行链式调用

### 获取Promise结果的时机
promise在任何使用都可以通过then获取结果，无论promise内的异步任务是否完成

Promise.all

## 事件
### 介绍
event参数
事件绑定1的差异
事件监听
addlistener
body添加监听函数
### 事件处理程序
### 事件流(捕获和冒泡)
中止冒泡事件

### 常见事件
### 事件代理
### 事件循环