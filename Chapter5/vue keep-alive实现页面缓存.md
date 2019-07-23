# Vue keep-alive实现页面缓存

首先keep-alive是vue内置的组件之一

在模板中有**两种场景**可以使用：

1、在 <router-view>外面包括

2、在组件外面包括 （如 <keep-alive> <edit></edit> </keep-live>）

添加完后，默认里面的一切页面/组件都会被缓存，这可能不是我们想要的，这时候需要做一些处理。

**include/exclude** 是keep-alive组件的属性，从单词上就可看出是包含或排除的意思，但是必须要知道组件的name。提供3种规则写法，

> 1. 组件name逗号分割
>
> 2. 数组
>
> 3. 正则匹配 如：/^list_/

- include: 字符串或正则表达式。只有匹配的组件会被缓存。
- exclude: 字符串或正则表达式。任何匹配的组件都不会被缓存。

### 常见用法：

```vue
<keep-alive include="test-keep-alive">
  <!-- 将缓存name为test-keep-alive的组件 -->
  <component></component>
</keep-alive>

<keep-alive include="a,b">
  <!-- 将缓存name为a或者b的组件，结合动态组件使用 -->
  <component :is="view"></component>
</keep-alive>

<!-- 使用正则表达式，需使用v-bind -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 动态判断 -->
<keep-alive :include="includedComponents">
  <router-view></router-view>
</keep-alive>

<keep-alive exclude="test-keep-alive">
  <!-- 将不缓存name为test-keep-alive的组件 -->
  <component></component>
</keep-alive>
```

### 结合router，缓存部分页面

使用$route.meta的keepAlive属性：

```vue
<keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

需要在`router`中设置router的元信息meta：

```javascript
//...router.js
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Hello',
      component: Hello,
      meta: {
        keepAlive: false // 不需要缓存
      }
    },
    {
      path: '/page1',
      name: 'Page1',
      component: Page1,
      meta: {
        keepAlive: true // 需要被缓存
      }
    }
  ]
})
```

### 例子

- 首页是A页面
- B页面跳转到A，A页面需要缓存
- C页面跳转到A，A页面不需要被缓存

思路是在每个路由的`beforeRouteLeave(to, from, next)`钩子中设置`to.meta.keepAlive`：

A的路由：

```javascript
{
    path: '/',
    name: 'A',
    component: A,
    meta: {
        keepAlive: true // 需要被缓存
    }
}

export default {
    data() {
        return {};
    },
    methods: {},
    beforeRouteLeave(to, from, next) {
         // 设置下一个路由的 meta
        to.meta.keepAlive = true;  // B 跳转到 A 时，让 A 缓存，即不刷新
        next();
    }
};

export default {
    data() {
        return {};
    },
    methods: {},
    beforeRouteLeave(to, from, next) {
        // 设置下一个路由的 meta
        to.meta.keepAlive = false; // C 跳转到 A 时让 A 不缓存，即刷新
        next();
    }
};
```

**keep-alive生命周期钩子函数：activated、deactivated**

使用`<keep-alive>`会将数据保留在内存中，如果要在每次进入页面的时候获取最新的数据，需要在`activated`阶段获取数据，承担原来created钩子中获取数据的任务。