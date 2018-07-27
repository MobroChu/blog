---
title: vue学习笔记（一）
date: 2018-07-27 18:43:33
tags: [vue, vue-cli]
description: 对于 vue 官网给的教程由浅及深，非常容易上手。我之前有过 react 项目开发经验，对 webpack 打包，脚手架这一类的东西并不陌生。所以也是我上手比较快的原因吧。简单将我在学习 vue 中遇见的问题和我觉得比较重要的东西记录一下，增加记忆。先说好，我这是个人笔记，不是教程，不喜勿喷。
---

*对于 vue 官网给的教程由浅及深，非常容易上手。我之前有过 react 项目开发经验，对 webpack 打包，脚手架这一类的东西并不陌生。所以也是我上手比较快的原因吧。简单将我在学习 vue 中遇见的问题和我觉得比较重要的东西记录一下，增加记忆。先说好，我这是个人笔记，不是教程，不喜勿喷。*

哦，有个特别尴尬，特别严肃的问题。   
我想说一声  
读 ： /vjuː/，类似于 view  
别在读 v u e 了，各位大佬。  
官网学习地址：[https://cn.vuejs.org/v2/guide](https://cn.vuejs.org/v2/guide)


## 简单介绍

一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以**自底向上逐层应用**。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

#### 安装方式


```npm
npm i -g vue vue-cli
```
官网上说对于新手不推荐使用 `vue-cli`，注意学习一下打包工具 webpack， gulp 之类的，了解一下 node 构建流程，然后再回头用 vue-cli，绝壁会醍醐灌顶，如梦初醒，恍然大悟，豁然开朗，茅塞顿开... 没得词语了，书读少了。差不多就是这个意思，站在了一个更高的层面的来学 vue-cli 了。  

然后，新建一个文件夹 vue 用来存放项目的。在 vue 文件夹下打开命令行，用 vue-cli 初始化一个项目。ok， just so
``` ssh
vue init webpack 项目名称
```
然后一路回车就好了。回车前可以大概看一下，各个选项都是什么意思。
```js
Project name (项目名称)： 项目名称
Project description (A Vue.js project)： 项目描述，可以为空。
Author：作者姓名
Runtime + Compiler: recommended for most users： 运行加编译
Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specificHTML) are ONLY allowed in .vue files - render functions are required elsewhere：回车就好了
Install vue-router? (Y/n)： 是否安装vue-router，一般都要安装
Use ESLint to lint your code? (Y/n)：  是否使用ESLint管理代码，看个人习惯，条条框框太多，个人不愿使用。
接下来也是选择题Pick an ESLint preset (Use arrow keys)： 选择一个ESLint预设，编写vue项目时的代码风格
Setup unit tests with Karma + Mocha? (Y/n)： 是否安装单元测试
Setup e2e tests with Nightwatch？(Y/n)： 是否安装e2e测试 
```


#### 数据与方法

当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。  

_1. 只有当实例被创建时 data 中存在的属性才是响应式的。_
_2. 唯一的例外是使用 Object.freeze(),这会阻止修改现有属性，系统将无法追踪变化。_
_3. 除了数据属性，vue实例还会暴露一些有用的实例属性和方法。通常都前缀`$`，以便与用户定义的属性区分开来。如 `$watch`, `$el`, `$data` ..._

``` js
var data = {
    a: 1
}
var vm = new Vue({
    el: "#example",
    data: data
})

console.log(
    "vm.$data === data  ", vm.$data === data, "\n", // true
    "vm.$el === document.getElementById('example'): ",  vm.$el === document.getElementById('example') // true
)
vm.$watch('a', function(newValue, oldValue) {
    // 当修改 a 的值时，会执行该方法
    console.log("newValue === oldValue: ", newValue === oldValue)
})
    
```
再举个栗子：  
![image]()
具体可以查看 [API了解更多](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)

#### 生命周期

要说，生命周期是最基础，最必须要理解的。和 react 一样，每个实例在被创建时，都会经历几个阶段。beforeCreate，created， beforeMount， mounted， beforeUpdate， updated， beforeDestroy， destroyed

#### 模板语法
##### # 插值
数据绑定最常见的形式就是使用“Mustache”语法（双大括号）  ==> 一般用于插入文本
v-html： Mustache 会将数据解析成普通文本，而非 html 代码。  

```html
<p>Using mustaches: {{rawHtml}}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

span 的内容会被替换成 rawHtml 的值，这种会忽略解析 rawHtml 值中的数据绑定。不能使用 v-html 来复合局部模板，因为 vue 不是基于字符串的模板引擎。  

Mustache 语法不能作用在 html 特性上，遇到这种情况应该使用 v-bind 指令；在布尔特性的情况下，v-bind 工作起来略有不同。eg：

```js
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 isButtonDisabled 的值是 null， undefined 或是 false，则 disabled 特性甚至不会被包含在渲染出来的 <button> 元素中。

对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。流控制，语句，语句块都是不会生效的。

PS: 模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。  

##### # 指令
指令 (Directives) 是带有 v- 前缀的特殊特性。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。  

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。eg: v-bind, v-on  

```js
<a v-bind:href="url">aaa</a>
<a v-on:click="doSomething">bbb</a>
```

在这里 href 是参数，告知 v-bind 指令将该元素的 href 特性与表达式 url 的值绑定；v-on 用于监听 dom 事件。

对 v-bind 和 v-on 的简写为： `:`, 和 `@`; eg:  
```js
<a v-bind:href="url">aaa</a>
<a :href="url">aaa</a>

<a v-on:click="do">bbb</a>
<a @click="do">bbb</a>
```

##### # 计算属性和侦听器

PS: 对于任何复杂逻辑，都应当使用计算属性（computed）;

**计算属性 vs 方法**  
计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。使用方法，就不会有缓存。

**计算属性 vs 侦听属性**  
Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch。  
计算属性默认只有 getter，在需要的时也可以提供一个 setter；

```js
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

**侦听器**（有时间，再了解一下）  

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。  

在这儿需要学习一下 axios， lodash。  
axios： 异步请求封装  
lodash： 限制操作频率

有个问题值得注意一下：
```js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})

<my-component v-bind:class="{ active: isActive }"></my-component>

当 isActive 为 truthy 时， html 将会被渲染成：
<p class="foo bar active">Hi</p>

```
尤其注意，这里的 truthy 不是 true 哈。详情参见[MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)  

##### # 条件渲染
**v-show**  
1. 始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
2. v-show 不支持 <template> 元素，也不支持 v-else。

**`v-if` vs `v-show`**  
+ v-if:   
1. 会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
2. v-if 也是惰性的；如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
+ v-show:   
不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。  

**_一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。_**  

`v-if` 和 `v-for` 一起使用时，`v-for` 具有比 `v-if` 个更的优先级。  
##### # 列表渲染

1. 在 v-for 块中，我们拥有对父作用域属性的完全访问权限；  
question： 对祖先级呢？  
2. 可以用 of 代替 in 作为分隔符。`v-for="item of items"`
3. 可以用 v-for 遍历对象。可以有三个参数 (value, key, index);
4. 在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。  

Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。如下：  
+ push()
+ pop()
+ shift()
+ unshift()
+ splice()
+ sort()
+ reverse()

变异方法 (mutation method)，顾名思义，会改变被这些方法调用的原始数组。相比之下，也有非变异 (non-mutating method) 方法，例如：filter(), concat() 和 slice() 。这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组：
```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。  

---
PS:   
由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

1. 当利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
2. 当修改数组的长度时，例如：vm.items.length = newLength
举个栗子：   
```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```   
解决方法
```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

// 也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名
vm.$set(vm.items, indexOfItem, newValue)

// 解决第二类问题
vm.items.splice(newLength)
```
---






---
当年吹过得牛逼，如已不然，便全不作数。  -- Mobro

