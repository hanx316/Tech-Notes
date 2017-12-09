# Vue中的过滤器

过滤器在Vue2.0版本之后一度曾被废除，后来又添加了回去。大概是因为过滤器在实际效果上的表现其实通过methods，通过计算属性都可以实现吧。

Vue官方教程中对[过滤器](https://cn.vuejs.org/v2/guide/filters.html)的介绍也比较清楚，它的使用场景也如同官方所说，用于常见文本的格式化。

过滤器可以写在文本插值和v-bind表达式中，语法也比较特殊，是一根竖线将data和filter分隔开，一开始不太熟悉这样的语法，还以为是在做位运算。

```html
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

可以在组件的option里面定义`filters`，也可以全局注册`Vue.filter(name, func)`

过滤器函数的第一个参数都是前面的data属性，在串联过滤器时，前一个过滤器的处理结果将作为第一个参数传入后一个过滤器，过滤器也可以传入额外的参数，但第一个参数是确定的。

```
{{ message | capitalize | addDate(('2017-12-09') }}
```

在使用过滤器的时候，有一个问题官方并没有指出。在过滤器函数中访问`this`将会得到`undefined`，具体原因官方没有指出，恐怕还得阅读vue的源码。不过我做了一个小小的实验，先记录下表现，以后再深究。

```
<div>{{ testFilter | filtAbc }}</div>

data () {
  return {
    testFilter: 'abc'
  }
},
filters: {
  filtAbc(a) {
    console.log(this) // undefined
    return a.toUpperCase()
  }
},
watch: {
  testFilter() {
    console.log('watch')
  }
},
beforeCreate() {
  console.log('beforeCreate')
},
created() {
  console.log('created')
},
beforeMount() {
  console.log('beforeMount')
},
mounted() {
  console.log('mounted')
  this.testFilter = 'def'
},
```

在`mounted`之前的周期钩子中对abc的值进行修改的话，并不会触发filter的二次执行，换到`mounted`以后则会执行两次filter。

结合对filter使用场景来思考，过滤器主要用来对字符串模板进行二次加工，在挂载dom以后，由于已经输出了既定的模板，这时候修改data因而需要重新过滤吧。

比较疑惑的是在过滤器里访问this并不能拿到当前的vue实例，而是`undefined`，其实也很容易想清楚，因为实例也包含了既定的template，而template这时候还需要filter执行以后才能确定。但是运行上面的代码却发现，过滤器的执行（undefined打印的时间）是在beforeMount和mounted之间，再去看了下经典的[vue生命周期图](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)，猜测也许filter的执行时机是在render function执行的时候？