# 数据劫持

Vue3.0中，van You 放弃了Object.defineProperty，加入了Proxy来实现数据劫持，那么这两个函数有什么区别呢？本文深入的剖析一下两者的用法以及优缺点，相信看文本文你也会理解为什么Vue会选择Proxy。

## defineProperty 的缺陷

`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

```js
// obj：要定义属性的对象。
// prop：要定义或修改的属性的名称。
// descriptor：要定义或修改的属性描述符。
Object.defineProperty(obj, prop, descriptor)
```

简单示例:

```js
var user = {}
Object.defineProperty(user, 'name', {
  value: 'erikchan'
})
```

descriptor 属性描述是个对象，有以下属性：

| 属性名       | 描述                                       | 默认值   |
| ------------ | ------------------------------------------ | -------- |
| value        | 属性的值                                   | undefine |
| configurable | 属性描述是否可修改<br>属性描述是否可以删除 | false    |
| enumerable   | 对象是否可枚举                             | false    |
| writable     | 对象是否可重写                             | false    |
| get          | 访问改对象时， 调用此函数                  | undefine |
| set          | 属性值被修改时，调用此函数                 | undefine |

```js
// get 和 set 劫持数据
Object.defineProperty(user, "name", {
    get: function(){
        console.log('get name')
        return initName
    },
    set: function(val){
        console.log('set name')
        initName = val
    }
});
```

`Object.defineProperty`能够通过 get 和 set 劫持对象的属性，但是需要对对象的每一个属性进行遍历劫持；如果对象上有新增的属性，则需要对新增的属性再次进行劫持；如果属性是对象，还需要深度遍历。这也是为什么Vue给对象新增属性需要通过`$set`的原因，其原理也是通过`Object.defineProperty`对新增的属性再次进行劫持。

看一下 `Object.defineProperty`  对数组的劫持:

```js
var list = [1,2]

Object.defineProperty(list,  index, {
    get: function(){
        console.log("get index:" + index)
        return initName
    },
    set: function(val){
        console.log("set index:" + index);
        initName = val
    }
});

list[1] = 0; 			// set index:1
list.push(-1) 			// 无输出
list.length = 5 		
console.log(list[4]) 	// undefined
```

但是 `Object.defineProperty` 在劫持对象和数组存在以下缺陷：

- 无法检测到对象属性的添加或删除

- 无法检测数组元素的变化，需要进行数组方法的重写

- 无法检测数组的长度的修改

## 认识 Proxy

Proxy 相对 defineProperty，不在局限某个属性，而是直接对整个对象进行**代理**.

`Proxy`可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

基本语法:

```js
var proxy = new Proxy(target, handler);
```

实例:

```js
var target = {}

var proxyObj = new Proxy(
    target,
    {
        get: function (target, propKey, receiver) {
            console.log(`getting ${propKey}!`);
            return Reflect.get(target, propKey, receiver);
        },
        set: function (target, propKey, value, receiver) {
            console.log(`setting ${propKey}!`);
            return Reflect.set(target, propKey, value, receiver);
        },
        deleteProperty: function (target, propKey) {
            console.log(`delete ${propKey}!`);
            delete target[propKey];
            return true;
        }
    }
);
```

可以看到Proxy直接代理了`target`整个对象，并且返回了一个新的对象，通过监听代理对象上属性的变化来获取目标对象属性的变化, 并且 deleteProperty 还能监听属性的删除。

proxy 处理数组的劫持:

```js
var list = [1,2]
var proxyObj = new Proxy(list, {
    get: function (target, propKey, receiver) {
        console.log(`getting ${propKey}!`);
        return Reflect.get(target, propKey, receiver);
    },
    set: function (target, propKey, value, receiver) {
        console.log(`setting ${propKey}:${value}!`);
        return Reflect.set(target, propKey, value, receiver);
    },
})

proxyObj[1] = 0; 		// setting 1:0
proxyObj.push(-1) 		// getting push!  getting length! setting 2:-1! setting length:3!
proxyObj.length = 5 	//setting length:5!
```

从上可以看出, 不管是数组下标或者数组长度的变化，还是通过函数调用，Proxy都能很好的监听到变化。