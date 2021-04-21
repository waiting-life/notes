### 第四天` Vue`框架

#### 1. 单页/多页面应用

##### 1. SPA单页面应用

**单页面应用**：服务器一次性把完整的页面返回回来，通过前端路由切换页面。

**单页面应用优点**：

1. 前后端分离
2. 页面的切换流畅

**单页面应用缺点**：

1. 首屏加载慢，容易出现白屏的情况
2. 对`SEO`不友好

##### 2. 多页面应用

**多页面应用**：每个页面都会重新发送请求，服务器返回的页面

**多页面应用优点**：

1. 首屏加载快
2. 对`SEO`友好

**多页面应用缺点**：

1. 页面的切换不流畅。

#### 2. v-if和v-show

**v-if**

1. v-if是真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当的销毁和重建
2. v-if也是惰性的：如果在初始渲染时条件为假，则什么也不做，直到条件第一次变为真时，才开始渲染条件块。

**v-show**

v-show不管条件是什么，元素总是会被渲染，并且只是简单地基于`CSS`进行切换。

**v-if有更高的切换开销（因为每一次切换元素条件块都会重新卸载或重建），v-show有更高的初始化开销（因为它不管条件为真为假，条件块一直会渲染），所以如果需要非常频繁的切换，使用v-show比较好，如果运行时条件很少改变，则使用v-if较好**

#### 3. computed和watch的区别和运用场景

**computed**：是计算属性，依赖其他属性值，并且computed的值有缓存，只有它依赖的属性值发生改变，下一次获取computed的值时才会重新计算computed的值

**watch**:更多的是观察的作用，类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作。

**运用场景**：

+ 当我们需要进行数值计算，并且依赖于其他数据时，应该使用computed，因为可以利用computed的缓存特性，避免每次获取值时，都要重新计算。

+ 当我们需要在数据变化时执行异步或开销较大的数据时，应该使用watch，使用watch选项允许我们执行异步操作，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。

#### 4. `Vue`的生命周期

1. 生命周期是什么

`Vue`实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载DOM->渲染、更新->渲染、卸载等有一些列过程。

2. 各个生命周期的作用

`beforeCreate`:组件实例被创建之初，组件的属性生效之前。

`created`:组件实例已经完全创建，属性也绑定，但真实`dom`还没有生成，`$el`还不可用。

`beforeMount`：在挂载之前开始被调用：相关的render函数首次被调用

`mounted`:el被新创建的`vm.$el`替换，并挂载到实例上去之后调用该钩子。

`beforeUpdate`:组件数据更新之前调用，发生在虚拟DOM打补丁之前

`updated`：组件数据更新之后

`actived`：keep-alive专属，组件被激活时调用

`deactived`：keep-alive专属，组件被销毁时调用

`beforeDestory`：组件销毁前调用

`destoryed`：组件销毁之后调用。

#### 5. `Vue`的父组件和子组件生命周期钩子函数执行顺序

##### 1. 生命周期钩子

`Vue`的父组件和子组件生命周期钩子函数执行顺序可以归类为以下4部分

+ 加载渲染过程

  父`beforeCreate`	->	父`created`	->	父`beforeMount`	->	子`beforeCreate`	->	子`created`	->	子`beforeMount` 	->	子`mounted`	->	父`mounted`

##### 2.  在哪个生命周期内调用异步请求

在created里面获取服务器请求，因为created在数据和模板(编译成渲染函数)生成虚拟`dom`之前，我们可以直接用收到的新数据渲染页面。

+ 能更快获取到服务端数据，减少页面loading时间

##### 3. 在什么阶段才能访问操作DOM

在钩子函数`mounted`被调用之前，`Vue`已经将编译好的模板挂载到页面上了，所以在mounted中可以访问操作DOM

#### 6. keep-alive

keep-alive是`Vue`内置的一个组件，可以使被包含的组件保留状态，避免重新渲染。

+ 一般结合路由和动态组件一起使用，用户缓存组件
+ 提供`include`和`excluede`属性
+ 对应两个钩子函数`actived`和`deactived`，当组件被激活时触发钩子函数`actived`，当组件被移除时，出发钩子函数`deactived`



#### 7. 组件中data为什么是一个函数

**组件是可复用的`Vue`实例**

为什么组件中的data必须是一个函数，然后return一个对象，而`new Vue`实例里，data可以直接是一个对象

+ 因为组件是用来复用的，且对象是引用类型，如果组件中data是一个对象，那么这样作用域没有隔离，子组件中的data属性值会相互影响，如果组件中data是一个函数，返回一个对象，那么每个实例都可以维护一份被返回对象的独立的拷贝，组件实例之间的data属性值不会相互影响。
+ 而`new Vue`的实例，是不会被复用的，因此不存在引用对象的问题。

#### 8. v-model的原理

```html
<input v-model="something"/>
<!--等价于-->
<input value="something" @input = "something = $event.target.value"/>
```

如果在自定义组件中，v-mode会默认利用名为value的prop和名为input的事件。

#### 9. `Vue`组件间通信的方式有哪几种？

1. props，$emit父子组件通信

+ 父组件通过props传值给子组件
+ 子组件通过$emit触发事件，父组件监听到该自定义事件后，执行相应的回调

2. `EventBus`

这种方法通过一个空的`Vue`实例作为中央事件总线，用它来触发事件和监听事件。

3. `vuex`

`Vuex`是一个专为`Vue.js`应用开发的状态管理模式。每一个`Vuex`应用的核心就是store是一个容器，它包含着应用中大部分的状态。

+ `vuex`中的状态存储是响应式的。
+ 改变store中的状态的唯一途径就是显式地提交(commit)mutation

#### 10.`vue-router`路由模式有哪几种？实现原理？

**有hash和history两种模式**

两个都是通过`history.pushState`来改变`url`的，

`pushState`只是向浏览器历史记录栈添加新的记录

`pushState`不会发请求

无论哪种模式，本质都是使用`history.pushState`，每次`pushState`后，会在浏览器地历史记录栈中添加一个新的记录，但是并不会触发页面刷新，也**不会请求新的数据**

1. **hash**

+ hash值的改变，都会在浏览器的访问历史中增加一个记录，因此我们能通过浏览器的回退、前进按钮控制hash的切换。
+ 可以使用`hashchange`事件来监听hash值的变化，从而对页面进行跳转。

2. **history**

+ 使用history模式需要后端进行配置
+ 因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问http://oursite.com/user/id就会返回404

+ `pushState`和`replaceState`两个`API`来操作实现URL的变化





+ 所以要在服务端增加一个覆盖所有情况的时候的候选资源：入宫URL匹配不到任何静态资源，则应该返回同一个`index.html`页面，这个页面就是你`app`依赖的页面(兜底页面)





`hashchange`只有在hash值改变时才能触发，而`popstate`在支持它的浏览器中只要按下“前进”，“后退”按钮就会在window对象上触发

#### 11. `Vue`是如何实现双向绑定的？

视图更新data； data改变更新视图

视图变化更新data：可以通过事件监听的方式来实现

data变化更新视图：响应式原理



`Vue`是通过数据劫持结合发布-订阅的方式来实现双向绑定的， 通过`Object.defineProperty()`对属性进行劫持，使用属性的时候触发getter函数，收集依赖；修改属性的时候触发setter函数，即监听到了属性值的变化，触发相应的回调。

`Vue`主要通过以下4个步骤来实现数据双向绑定的：

1. 实现一个监听器Observer: Observer对数据的属性进行递归遍历，通过`Object.defineProperty()`进行数据劫持。
2. 实现一个解析器Compiler：用于将模板编译为渲染函数，并渲染视图。
3. 实现一个订阅者Watcher: Watcher是Observer和Compiler之间通信的桥梁。主要的任务是订阅Observer中的属性值变化的消息，当收到属性值变化的消息时，触发解析器Compiler中对应的更新函数。

```js
set: () => {
	watch.update()
}
```



4. 实现一个订阅器`Dep`:订阅器采用发布-订阅设计模式，用来收集订阅者Watcher，对监听器Observer和订阅者Watcher进行统一管理。

#### 12. `Vue`中key的作用

key是虚拟`dom`的唯一标识，依靠 key,我们的 `diff `操作可以更准确、更快速

#### 13. `nextTick`的使用

`Vue`是异步执行DOM更新的。

1. `Vue`生命周期的**created()钩子函数进行DOM操作时，一定要放在`Vue.nextTick()`的回调函数中。**`$nextTick`是在下次DOM更新循环结束之后执行延时回调，再修改数据之后使用`$nextTick`，则可以在回调中获取更新后的DOM

```js
created () {
    // console.log(this.$refs.name)
    // created里面不能直接操作dom, 会报错
    // this.$refs.name.innerHTML = 'cpp'
    // this.$refs.age.innerHTML = 22

    this.$nextTick(() => {
      this.$refs.name.innerHTML = 'cpp'
      this.$refs.age.innerHTML = 22
    })
  },
```

#### 14. `Vue`怎么用`vm.$set()`解决对象新增属性不能响应的问题？

+ `Vue`**无法检测到对象的属性的添加或删除**。属性必须在data对象上才能让`Vue`将它转换为响应式。

+ `Vue`提供了`Vue.set(object, propertyName, value) / vm.$set(object, propertyName, value)`来实现为对象添加响应式属性。

#### 15. 虚拟DOM的优缺点

**优点：**

+ **保证性能下限**：框架的虚拟DOM至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限。
+ **无需手动操作DOM**：框架会根据虚拟DOM和数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率。
+ **跨平台**：

**缺点：**

+ **无法进行极致的优化**



#### 16. 虚拟DOM的实现原理

虚拟DOM的实现原理主要包括以下3部分

+ 用`javascript`对象模拟真实DOM树，对真实DOM进行抽象。
+ `diff`算法—比较两棵虚拟DOM树的差异。
+ `pach`算法—将两个虚拟DOM对象的差异应用到真正的DOM树。



