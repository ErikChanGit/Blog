# Python 高级用法

## Lamda

匿名函数，可以在不适用函数名的情况下完成简单的函功能

```JavaScript
fun = lammda a,b:a*a+b*b
res = fun(1,2) // output:5 
```

## 内置函数
> Map 函数

fun( arg );

map(fun, list) // 映射出list中每个参数带到 fun 中求出的结果

> Filter 函数

fun( arg );

filter(fun, list) //  根据 fun 求出的结果过滤list中返回 false 的值

> itealTools 模块

- chain() : 将多个可迭代的数据连接在一起

- count（n, step）: 从 n 开始，步长为step，获取无限序列

- filterFalse(fun, list ) : 根据 fun，过滤 list中为false的数据

- compress(list, booleans[] )

- starmap( fun, list) : 针对 list 中每一项， 调用 fun

- ...

## yield 生成器

yield 的作用就是把一个函数变成一个 generator，每次 yield会把数据append 到一个可迭代的数据中返回给函数

参考 [https://www.runoob.com/w3cnote/python-yield-used-analysis.html](https://www.runoob.com/w3cnote/python-yield-used-analysis.html)

## 多进程

> 多进程

进程是一个正在执行的程序，是资源分配最小的单位

开启进程

```JavaScript
multiprocessing.Process(target=run,args=(i,)) #创建多进程和
```

> 多线程

与进程类似，不过它们是在同一个进程下执行的，并且它们会共享相同的上下文。是CPU 调度最小的单位

开启线程

```JavaScript
threading.Thread(target=Producer,args=("Jack",))
```

经典的问题:  生产者与消费者模式

> 协程


