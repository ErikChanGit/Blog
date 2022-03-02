## 搞懂 Vuex





#### 生命周期

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeDestory
- destory

![img](https://upload-images.jianshu.io/upload_images/13119812-5890a846b6efa045.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)



1

#### Vuex

```
mutations 
操作state对象属性的方法
const mutations = {
    mutationsAddCount(state, n = 0) {
        return (state.count += n)
    },
    mutationsReduceCount(state, n = 0) {
        return (state.count -= n)
    }
}
```

```
actions 异步操作
const actions = {
    actionsAddCount(context, n = 0) {
        console.log(context)
        return context.commit('mutationsAddCount', n)
    },
    actionsReduceCount({ commit }, n = 0) {
        return commit('mutationsReduceCount', n)
    }
}
```

```
getter 获取 state 中的值
const getters = {
    getterCount(state, n = 0) {
        return (state.count += n)
    }
}
```



```
更简单的方式来使用vuex
import {mapState, mapMutations, mapActions, mapGetters} from 'vuex'
export default {
methods: {
    ...mapMutations({
      handleAddClick: 'mutationsAddCount',
      handleReduceClick: 'mutationsReduceCount'
    }),
    ...mapActions({
      handleActionsAdd: 'actionsAddCount',
      handleActionsReduce: 'actionsReduceCount'
    })
}
}
```



```
格式化 json

var content = JSON.stringify(this.bsp_info);  // 未格式化

var content = JSON.stringify(this.bsp_info, null, 4);  //格式化
JSON.stringify(json [, replacer] [, space]) 的用法

(1) json值：必须，可以是数组或Object;

(2) replacer: 可选值，可以是数组，也可以是方法；

数组时，它是和json有关系的：
　　一般来说，系列化后的结果是通过键值对来进行表示的。
　　如果此时replacer的值在value中存在，那么就以replacer的值做key，json的值为value进行表示，如果不存在，就忽略。
方法时，就是说把系列化后json的每一个对象传进方法里面进行处理。

(3) space: 用什么来进行分隔；
　　 1）如果省略，直接输出。
　　 2）如果为数字，就缩进几个字符，最大值为10。
　　 3）如果为转义字符，比如“\t”，表示每行一个回车。
　　 4）如果为字符串，在每行输出值的时候把这些字符串附加上去，字符串长度最大为10。

(4) 常用的格式化：

　　JSON.stringify(json, null ,2) // 每行缩进两个空格
　　JSON.stringify(json, null, '\t') // 每行以tab键缩进
```



```csharp
获取兄弟元素
<button @click = “clickfun($event)”>点击</button>
 
methods: {
clickfun(e) {
// e.target 是你当前点击的元素
// e.currentTarget 是你绑定事件的元素
    #获得点击元素的前一个元素
    e.currentTarget.previousElementSibling.innerHTML
    #获得点击元素的第一个子元素
    e.currentTarget.firstElementChild
    # 获得点击元素的下一个元素
    e.currentTarget.nextElementSibling
    # 获得点击元素中id为string的元素
    e.currentTarget.getElementById("string")
    # 获得点击元素的string属性
    e.currentTarget.getAttributeNode('string')
    # 获得点击元素的父级元素
    e.currentTarget.parentElement
    # 获得点击元素的前一个元素的第一个子元素的HTML值
    e.currentTarget.previousElementSibling.firstElementChild.innerHTML
    }
},
```