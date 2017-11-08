# Vue的插件编写

---

可以参看[官方教程的插件章节部分](https://cn.vuejs.org/v2/guide/plugins.html)，目的主要在于通过`Vue.use()`全局引入一些方法，模块，组件等

`Vue.use()`接受一个对象或者函数作为参数传入，意味着编写的插件必须作为一个对象或者函数导出

如果是对象的话，必须提供一个名为`install`的方法，调用`Vue.use()`会自动调用它。如果插件本身是一个函数，那么它也将作为`install`方法被调用。通常在对Vue的操作都写在这个方法里面

下面是一个注册全局组件的例子

```
// 全局组件
// ./Components/Global/Global.vue
<template>
  <div>Gloabl Component</div>
</template>
<script>
export default {...}
</script>

// ./Components/Global/index.js
import Global from './Global.vue'
export default {
  install() {
    Vue.component('Global', Global)
  }
}

// 使用
import Global from './Components/Global'
Vue.use(Global)
```

其他诸如在`Vue.prototype`上挂载方法也可以通过写在`install`方法里完成，具体可参考官方教程