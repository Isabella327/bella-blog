---
sidebar_position: 1
---

# Vue3基础快速

1.class绑定多个 属性 对象or数组or三元运算  都有
```
<div class="static" :class="{ active: isActive, 'text-danger': hasError }" ></div>
<div :class="[ activeClass, errorClass ]" ></div>
<div :class="[isActive ? activeClass : '', errorClass]"></div>
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
:style 用加号内联样式、用data下面的对象、用data下面的数组
```
2. v-on:click="setMsg('xxx')" 绑定方法
3. 循环遍历`<li v-for="(item, index) in list" :key="index">{{item}}</li>`
4. `data-aid="123" @click="eventFn($event)"` 事件对象 获取dom节点和自定义属性
`eventFn(e){  e.srcElement.style.backgroud = "red" }`  改变背景颜色
`e.srcElement.dataset.aid` 获取自定义属性'123'
`@click="warning('传入的参数', $event)"`  多个参数 `$event`放最后 
5. 一次执行多个方法 `@click="one(), two(), three()"`
5. 组织默认行为` <a href="http" target="_blank" @click="aFn($event)">  aFn(e){ e.preventDefault() }` 事件对象或者直接事件修饰符  ` @click.prevent="aFn()"`
`<input type="text" @keyup="doSearch($event)">` 监听键盘事件 
`e.keyCode == 13` 按了回车键 或者利用按键修饰符 `@keyup.enter="" `按回车时执行
6. 获取dom节点 定义`ref + this.$refs.age`
7. v-model MVVM model改变影响视图 视图改变影响model ；name相同 同一个组；
`<option v-for="(item, index) in userinfo.cityList" :key="index" :value="item">{{ item }} <option>`
8. `<pre>`原样输出
9. v-if是操作dom，v-show是通过css隐藏`（display:none）`适合频繁操作
10. 方法直接`{{ fuc() }}` 写在computed是计算属性，操作`this.msg`，会跟着msg的改变而重新计算属性，具有实时性，或者用`watch`监听属性，不能用箭头函数
11. 给子组件赋值的时候可以直接传递  `:home="this"` 子组件通过`this.home.run()` 执行父组件的方法；子组件可以校验传过来的数据`props: { title: Number }`也可以提供默认值；
12. 父组件可以传给子组件，父更新子会更新，但是子组件不能更新父组件传过来的数据；父组件想获取子组件的数据需要利用ref，父组件利用`this.$refs.header` 获取子组件的数据；子组件利用`this.$parent.run()`获取父组件的实例，ref和parent都可以改写
13. 自定义组件`@aaa`  父组件利用` @aaa="xxxx"` 子组件利用 `this.$emit("aaa")` 执行该方法时进行广播（emit)，触发父组件的aaa事件，执行xxxx方法；emit也可以传递数据，在子组件里export一个emits，里面写着数据处理方法比如校验，用来监听aaa事件，触发事件先执行`emits: { aaa : () => {} }`
14. 非父子组件利用mitt包通信 实例化利用a组件on监听，b组件emit触发事件时a组件收到传值
15. 双向数据绑定在自定义组件中传值，父组件 `v-model:keyword="aaa"`，子组件 `props: ["keyword"]`传入 `:value="keyword"` 绑定数据 `@input="$emit('update:keyword', $event.target.value)"` 监听事件通过emit广播出去；同一个组件组可以v-model多个属性
16. slot在子组件中插入自定义html元素或改写数据，给子组件加`<slot></slot>`，父组件传入的内容直接替换`<slot>`内容
17. 非prop的attribute是指传给一个组件内容但是没有定义变量，比如class/style/id属性，如果父组件没有定义，class会被子组件里的根标签继承，在子组件里有定义的变量就可以实现；如果子组件不希望被继承，设置`inheritAttrs: false` 禁用默认继承，在希望继承的组件中使用 `v-bind="$attrs"` ,多个根节点需要指定继承
18. 生命周期：`beforeCreate（创建实例）->created->beforeMount（模板编译）->mounted（请求数据操作dom）->beforeUpdate(数据更新）->updated->beforeUnmount(实例销毁)->unmounted`
19.` keep-alive`可以缓存动态组件，在想缓存的组件外部包裹后，卸载再加载后保持鸳鸯，适合路由切换，`activated（keep-alive缓存组件激活时）-> deactivated` 与上面那些生命周期组件都互斥
20. mounted和updated里面都有`this.$nextTick()`方法，表示在渲染整个视图之后运行的代码，可以用来在 组件挂载后数据改变完更新视图 之后获取这个dom元素的数据
21. 全局配置时 改写main.js(入口） 为 `const app = createApp(App); app.mount('#app');`
22. 请求远程api利用axios和fetch-jsonp，全局绑定axios在main.js里面`app.config.globalProperties.Axios = Axios;` 组件利用`this.axios`直接使用；fetch-jsonp使用需要`jsonpCallback: 'cb'`，箭头函数可以直接用this绑定（function不能直接用this）
23. mixins复用组件，`export default { mixins: [baseMixin] }`把baseMixin里面的所有功能混入都直接用，用来抽离公共功能，如果和当前组件相同名字的属性冲突，会优先输出当前组件的属性and方法；全局设定main.js里配置 `app.mixin(baseMixin);`
24. teleport：一个组件的模板中的部分想移动到当前组件外的DOM中，对内容加一层`<teleport to="boby">内容</teleport>`内容就会显示在body中，比如组件的绝对定位的父组件的位置不定，可以在子组件外层加teleport，挂在别级去了，不会受父组件的位置影响