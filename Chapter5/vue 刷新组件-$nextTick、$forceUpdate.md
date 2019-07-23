# vue-刷新组件-`$nextTick`、`$forceUpdate`

很多时候，通过重置数据将页面重置时，子组件可以提供重置的方法，或者提供props重置自己的状态。但是相对麻烦，那可以使用强制刷新来实现刷新组件。

### .$nextTick( callback)

#### 组件

```vue
<component v-if="hackReset"></component>
```

#### data中 hardReset

```javascript
/*某一操作重置数据*/
this.hardReset= false
this.$nextTick(() => {
    this.hardReset= true
});
```

### .$forceUpdate()

迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

调用强制更新方法this.$forceUpdate()会更新视图和数据，触发updated生命周期。

> 使用场景：因为数据层次太多，render函数没有自动更新，需手动强制刷新。

数据通过异步操作后，对之前刚加载的数据进行变更后，发现数据不能生效

## vue 强制组件重新渲染（重置）

### 方案一： v-if

当数据变更后,通过`watch` 监听，先去销毁当前的组件，然后再重现渲染。使用 `v-if` 可以解决这个问题

```vue
<template>
	<third-comp v-if="reFresh" />
</template>
<script>
   export default{
       data() {
          return {
             reFresh: true,
             menuTree: []
          }
       },
       watch: {
           menuTree() {
               this.reFresh= false
               this.$nextTick(() => {
                   this.reFresh = true
               })
           }
       }
   }
</script>
```

这种方式虽然可以实现，太不优雅

### 方案二： key

通过vue `key` 实现，原理参考官方文档。所以当key 值变更时，会自动的重新渲染。

```vue
<template>
   <third-comp :key="menuKey" />
</template>
<script>
   export default{
       data() {
          return {
                menuKey: 1
            }
       },
       watch: {
             menuTree(){
                ++this.menuKey
            }
       }
}
</script>
```