## 开发板支持包UI总结

#### 组件

对于一些公共的 html ，可以封装成一个组件被其他页面共用；

组件使用方法： 

```js
import CustomerComponent from 'CustomerComponent.vue';
Vue.component('customer-component',CustomerComponent)

<customer-component> </customer-component>
```

#### 国际化

需要引入 VueI18n 模块，并未不同国家的语言设置key和value，

```js
import VueI18n from 'vue-i18n'
Vue.use(VueI18n)
const i18n = new VueI18n({
 locale: 'en', // 定义默认语言为中文
 messages: {
  'zh': require('@/assets/languages/zh.js'),
  'en': require('@/assets/languages/en.js')
 }
})
new Vue({
  el: '#app',
  i18n,
  render: h => h(App),
}).$mount('#app')
```

#### 状态管理

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态。

## css 作用域

scope 会限制当前的样式只在当前页面生效，除去 scope 后会在全局生效

#### 路由

设置路由可让多个页面共用一个公共区域，实现页面切换的功能

```
import Router from 'vue-router'
Vue.use(Router)

const routerPush = Router.prototype.push
Router.prototype.push = function push (location) {
  return routerPush.call(this, location).catch(error => error)
}

export default new Router({
  routes: [
    {
      path: '/',
      redirect: '/BSP_PKG_INFO', // 首次启动的跳转地址
      name: 'Layout',
      component: Layout,
      children: [
        {
          name: 'BSP_PKG_INFO', // 地址名，相当于 ID
          path: '/BSP_PKG_INFO', // url 相对路径
          component: resolve => require(['@/views/BspPkgInfo.vue'], resolve) // 被装载的页面
        },
        {
          name: 'BSP_INFO',
          path: '/BSP_INFO',
          component: resolve => require(['@/views/BspInfo.vue'], resolve)
        },
      ]
    }
  ]
})
```



