``` shell
ls
cd
pwd
rm
vi
mv
touch
mkdir
cat
```

## 总结知识点

### React与Vue

1. React每次改变数据都会调用一次render函数，父组件值改变了，子组件会重新渲染一次。可以使用纯组件pureComponent
2. Vue父组件数据的改变不会导致子组件视图的重新渲染。Vue的依赖是在渲染过程中自动追踪的，所以系统能精确知晓哪个组件需要被重新渲染。

#### Vue双向数据绑定原理

Vue是通过数据劫持结合发布-订阅模式的方式，实现的双向绑定。通过Object.defineProperty()来劫持属性的，使用属性的时候触发getter函数，收集依赖；修改属性的时候触发setter函数。

1. Observer对数据属性进行递归遍历，使用Object.defineProperty进行数据劫持。把数据属性转化为访问器属性，访问器属性里面有有getter和setter函数，当我们用到数据的时候，会调用getter函数，数据会和渲染函数（模板编译的）结合生成虚拟DOM，当我们修改数据的时候，会调用setter函数，然后新的数据和渲染函数结合生成新的虚拟DOM，两次的虚拟DOM做比较，然后渲染真实DOM。
2. Compiler用于将模板编译为渲染函数，并渲染视图页面
3. Watcher是Observer和Compiler之间通信的桥梁
   + 自身实例化的时候，调用getter函数，向deps添加watch
   + 当修该数据时，调用setter函数，
   + 执行watch的update函数，重新生成新的虚拟DOM，通过diff算法对页面进行修改。

### 响应式



#### 哪些没有响应式？

由于js的限制，Vue**不能检测**数组和对象的变化。

1. Vue不能检测对象属性的添加或删除

```js
var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的
```

+ 对于已经创建的实例，Vue不允许动态添加根级别的响应式property。但是，可以使用`Vue.set(object, propertyName, value)`方法向嵌套对象添加响应式property，例如

```js
Vue.set(vm.someObject, 'b', 2)

this.$set(this.someObject, 'b', 2)

// 有时可能需要为已有对象赋值多个新property
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

2. Vue不能检测以下数组的变动

为了解决这种情况，Vue重写了数组的很多方法，如push、pop、shift、unshift、splice、sort、reverse。（这些都是Vue重写之后的方法，所以会触发视图更新）

+ 当你利用索引值直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`

**解决方法**：

```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```



+ 当你修该数组的长度时，例如`vm.items.length = newLength`

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

**解决方法**：

```js
vm.items.splice(newLength)
```

####  生命响应式property



#### 异步更新队列

1. Vue在更新DOM时是异步执行的。
2. 只要侦听到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。
3. 如果同一个watcher被多次触发，只会被推到队列中一次。
4. 然后再下一个的世纪那循环'tick'中，Vue刷新队列并执行实际(已去重)的工作。
5. 例如，当你设置`vm.someData =  'new value'`,该组件不会立即重新渲染。当刷新队列时，祖籍安徽在下一个事件循环'tick'中更新。
6. 为了在数据变化之后等待Vue完成更新DOM，可以在数据变化之后立即使用`Vue.nextTick(callback)`。这样回调函数将在DOM更新完成后被调用。

```js
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '未更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '已更新'
      console.log(this.$el.textContent) // => '未更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '已更新'
      })
    }
  }
})
```

#### data

Vue实例的数据对象。Vue会递归的将data的property转换为getter/setter，从而让data的property能够响应数据变化。**对象必须是纯粹的对象（含有零个或多个的key/value对）**

### Vuex

存储在Vuex中的数据和Vue示例中的data遵循相同的规则，例如状态对象必须是**纯粹的（含有零个或多个的key/value对）**

#### 1. State

##### 1. 如何在Vue组件中获得Vuex状态

+ 由于Vuex的状态存储是响应式的，从store实例中读取状态最简单的方法就是在计算属性中返回某个状态。
+ 通过在根实例中注册`store`选项，该store实例会注入到根组件下的所有子组件中，且子组件能通过`this.$store`访问到。

##### 2. mapState辅助函数

+ 当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用`mapState`辅助函数帮助我们生成计算属性。

```js
computed: mapState(['count'])

computed: {
 ...mapState(['userName'])   
}
```



#### 2. Getter

+ Vuex中允许我们在store中定义'getter'(可以认为是store的计算属性)。就像计算属性一样，gettr的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。
+ getter接受state作为其第一个参数。

##### 1. 访问Getter的方法

###### 1. 通过属性访问

+ Getter会暴露为`store.getters`对象，你俩可以以属性的形式访问这些值。
+ getter也可以接受其他getter作为第二个参数。

**注意**：getter在通过属性访问时是作为Vue的响应式系统的一部分缓存其中的。

###### 2. 通过方法访问

+ 你也可以通过让getter返回一个函数，来实现给getter传参。在你对store里的数组进行查询时非常有用。

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}

store.getters.getTodoById(2)
```

##### 2. mapGetters辅助函数

+ mapGetters辅助函数仅仅是将store中的getter映射到局部计算属性。

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

#### 3. Mutation

##### 1. Mutation 需遵循Vue的响应规则



##### 2. 使用常量替代Mutation事件类型



##### 3. Mutation必须是同步函数



##### 4. 提交Mutation

1. 你可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation
2. 或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

#### 4. Action

Action类似于mutation，不同在于：

+ Action提交的是mutation，而不是直接变更状态
+ Action可以包含任意异步操作。

Action函数接受一个与store实例具有相同方法和属性的`context`对象，因此你可以调用`context.commit`提交一个`mutation`，或者通过`context.state`和`context.getters`来获取`state`和`getters`。

实践中，我们会用es6的参数结构来简化

```js
actions: {
    increment({ commit }) {
        commit('increment')
    }
}
```

##### 分发Action

Action通过`store.dispatch`方法触发:

```js
store.dispatch('increment')
```

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }

```

##### 在组件中分发Action

你在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```



**let const** 

1. 无声明提升
2. 不可重复声明
3. 块级作用域
4. 暂时性死区

**变量的解构赋值**

**字符串**

第五节 模板字符串

includes split charAt indexOf slice

**函数**

参数的默认值

rest参数

箭头函数

**数组**

Array.from

includes join concat slice

entries() keys() values()

**对象**

简洁表示法

对象的扩展运算符

Object.assign()

**promise**

**class的基本语法**

``` js
 function People(name, age) {
     this.name = name
     this.age = age
 }
 People.prototype.getAge = function () {return this.age}



class People {
    constructor(name, age) {
        this.name = name
        this.age = age
    }

    getAge() {
        return this.age
    }

    getName() {
        return this.name
    }

    yell() {
        console.log(233)
    }
}
```























研究目标

1. 掌握现代化问答类网站的设计和实现原理
2. 掌握最新的前后端技术栈原理以及使用方式
3. 训练学生的项目工程能力和编程实现能力，培养学生解决较复杂的实际工程问题的能力。

主要研究内容和方法

研究内容

1. 掌握Vue框架以及相关类库在大型项目场景下的使用
2. 掌握如何使用Node和Koa开发后端服务器
3. 掌握前后端的协作原理，了解如何对浏览器的性能进行优化。
4. 设计一个UI精美，功能完备的系统，尽可能的实现更多的功能。

研究方法

1. 大量阅读相关类库的文档，例如Vue, Node, Koa的官方文献。
2. 大量阅读近五年内的有关系统开发的文献和资料。
3. 阅读Github上相关项目的源码。



参考文献

[1] Nicholas C.Zakas, JavaScript 高级程序设计， 2012

[2] Jeremy Keith JavaScript Dom编程艺术，2011

[3] 朴灵 深入浅出Node.js 2014

[4] Evan You, Vuejs官方文档，2019

[5] W3C, Cascading Style Sheets Level 2 Revision 1 (CSS 2.1) Specification, 2016.





Vue-cli工具

vue create bishe



npm run build 

dist/

​	css/

​	js/

​		a.js

​	index.html





vue-router 





我们目前用过的软件

Node：用来执行JavaScript代码。比如在命令行输入 node app.js

npm: 用来下载软件的。比如在命令行 npm install vue-cli

vue-cli：用来快速搭建项目，在命令行vue create project来创建项目



项目的目录下，有两个文件夹，src和public。



一个网站分为前端和后端。前端的目的是写出网页，或者说写出一个dist文件夹。

dist/

​	/css/a.css

​	/js/a.js

​	/image/tupian.jpg

​	index.html

以前，我们可以直接新建dist文件夹，然后写代码。

现在呢，我们在src文件夹下写代码，然后通过npm run build来自动生成dist。



vue

条件渲染，用一个变量来控制元素/组件是否渲染。

v-if和v-show的区别

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。



v-bind: 单向绑定，给元素属性绑定组件的数据。

v-model：双向绑定，数据控制表单的值，表单的值可以改变数据。

v-model只能用在表单中。



父子组件如何通信。父亲给子组件通信，子组件给父组件通信。

父组件通过props传值给子组件。

``` html
<Test v-bind:sum="sum"></Test>
```

子组件通过emit改变父组件。父组件通过v-on监听子组件的事件，当子组件emit的时候，父组件就知道了。

``` html
<Test v-on:clicked=“sum++”></Test>

this.$emit('clicked')
```





计算属性和方法的区别。

计算属性，根据依赖进行缓存值。

方法不会缓存值。



组件的生命周期

beforeCreate

created 

beforeMount

Mounted 

beforeUpdate

updated

beforeDestroy

destroyed



组件的指令

v-if v-else-if v-else v-show v-for v-bind v-model v-on v-html 





vuex

​	



this.$store.state.user_id

this.$store.commit('setId', 100)

提交commit的两种风格

``` js
this.$store.commit('setId', {
  id: 110,
})

this.$store.commit({
  type: 'setId',
  id: 110,
})
```



- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。



``` html
<router-view></router-view>

<router-link to="/questions?id=11">点我</router-link>

```





这个项目是个类似知乎的问答平台。

前端主要使用的Vue框架，后端使用的Node搭建服务器。

主要实现了用户的注册，登录，提问，评论，获取问题，获取评论等功能。

前端使用vue-router来实现前端路由的切换，使用vuex来储存组件共有的数据。

熟练 

有前途 技术氛围好 可以学习新东西 薪资3-5k 2-3k 

登录 使用set-cookie保存用户状态 jquery是个库 用来操作dom js时语言 

可以

钱怎么交

自己基础好 熟悉vue

介绍一下

注册/登录是怎么实现的？

注册。

登录。

``` http
Set-cookie: user_id=25
```





vue-router 怎么用的: 我们的根组件有一个router-view，根据路径的不同渲染不同组件。首页的时候渲染渲染所有问题，每一个问题的标题是一个router-link，点击后前端路径变成了/question?id=18并且查询符会带上问题的id。在question组件中，获取id，然后发送请求获取问题的信息。



后端是怎么做的。用到了Node，node的框架Koa。koa-body，koa-static, koa-router让我们写接口清晰一点。

``` js
// node的写法

const http = require('http')
const server = http.createServer(function (req, res) {
    if (req.url === '/signup') {
        
    }
    if (req.url === '/signin') {

    }

   
})

server.listen(3000)


const Koa = require('koa')
const app = new Koa()

const body = require('koa-body')
const static = require('koa-static')

app.use(body())
app.use(static('dist/'))
app.use(function (ctx) {
    
})
app.listen(3000)
```

实现了什么接口。Set-cookie: user_id=12

注册/登录

提问/评论



输入URL/网址会发生什么

1. 浏览器根据URL解析出对应的协议，域名，端口，路径

2. 强制缓存 Cache-control: max-age=1000; 

   协商缓存 Etag: 12345

   发请求，请求头部If-none-match: 12345

3. DNS解析。根据域名得到对应的IP地址。

   浏览器是否有缓存，操作系统，hosts，DNS服务器。

4. 发送请求 get /  

5. 获得响应

   



function People(name, age) {
    this.name = name
    this.age = age
}

People.prototype.getName = function () {
    return this.name

function Student(name, age, id) {
    People.call(this, name, age)
    this.id = id
}

有

毕业设计 是一个web的应用

