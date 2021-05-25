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

