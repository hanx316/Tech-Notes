# Vue-Router全局导航守卫踩的一个小坑

Vue-Router提供的导航守卫能够让我们在路由进入或者离开的时候进行一些定制的操作，让整个流程更加可控

全局导航守卫即`router.beforeEach`则可以统一对所有路由跳转的动作进行一层控制，假定有一个需求说需要对每个路由跳转都进行一层判断来决定是否跳转目标路由，这个api就正好派上了用场

更多详细的介绍可以参考Vue-Router的文档[导航守卫](https://router.vuejs.org/zh-cn/advanced/navigation-guards.html)

---

假定场景如下：

对所有跳转的路由进行判断，如果符合条件就跳转，否则跳转到登录页

```javascript
router.beforeEach((to, from, next) => {
  matchCase ? next() : next('login')
})
```

乍一看这样似乎就万事大吉了，其实不然，运行一下发现当需要跳转到登录页时，整个路由就开始无限循环最后爆栈~

显然是`next('login')`之后又开始进入了导航钩子函数，但是`next()`则不会

查阅文档得知

>next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。

既然进入新的导航，那么自然也就会触发beforeEach的钩子，于是我们需要再加多一个判断让再次进入钩子时顺利跳转从而终止循环

```javascript
router.beforeEach((to, from, next) => {
  (to.path === 'login' || matchCase) ? next() : next('login')
})
```

这个坑也不难排查，看看文档就能理解