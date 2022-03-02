# TypeScript 高级用法

## 命令空间

目的：解决重名问题；

作用：是将一系列相关的全局变量组织到一个对象的属性

用法：

```JavaScript
namespace Drawing { 
    export interface IShape { 
        draw(); 
    }
}

/// <reference path = "IShape.ts" />   // 引用
namespace Drawing { 
    export class Circle implements IShape { 
        public draw() { 
            console.log("Circle is drawn"); 
        }  
    }
} 
```

命令空间可以嵌套