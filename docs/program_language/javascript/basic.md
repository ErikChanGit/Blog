# JavaScript 基础

## 宏任务与微任务
#### 背景
JS 是单线程的. 由于JS 的主要用途是与用户交互并且操作 DOM， 多线程会带来不必要的麻烦.  
在 JS 中,有同步任务和异步任务,异步任务会进入事件循环,并且先执行同步任务,再执行异步任务.

#### 事件循环
数据请求, 定时器, 事件 等任务会进入事件循环,等待被执行.  
在事件循环中, 包含宏任务和微任务.  
要执行宏任务之前需要先执行微任务

#### 宏任务
```JS
promise.then
process.nextTick()
async/await
```
#### 微任务
```JS
setTimeOut
setInterval
IO 操作
事件
UI 渲染
数据请求
```

#### 执行顺序
同步任务 -> 事件循环(异步任务) -> 微任务 -> 宏任务
![js 任务执行](../figures/js_tasks.jpg)

## 原型链
#### 可以解决什么问题
对象共享属性和和方法
#### 原型方式
- 函数拥有 protortpe
- 对象拥有 __proto__
#### 举例
```js
function Person() {
    this.run = "1";
}
var person = new Person();
// 函数拥有 protortpe
Person.protortpe.run = "2"
// 对象拥有 __proto__
person.__proto__.run = "3"
person.run = "4";
Object.protortpe.run = "5"

console.console.log(object.run); // 4,1,3,2,5
```
#### 原型链上的执行顺序
对象本身 -> 构造函数 -> 对象 __proto__  -> 构造函数 protortpe -> 当前原型的上级 protortpe

## js 继承的方式
#### es6
extends class 方式
#### 原型链继承
```js
Child.prototype = new Parent();
````
#### 借用构造函数
```js
function Child(){
  	this.name = ''
  	Parent.call(this);
  }
```
#### 组合
组合原型链和借用构造函数

