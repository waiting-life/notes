# Node.js

## Node.js中遇到的问题

+ 在判断路径时，写成```url='/'```
  + 不报错，但是其他页面始终无法渲染，但是自己却渲染了!=-=!
  + ！！！！！千万别写成等号了再‘’=-=’‘

```javascript
//app application 应用程序

let http = require('http')
let fs = require('fs')

// let server = http.createServer()
// server.on('request',(req,res) => {
//   let url = req.url
// })
// server.listen(3000,()=> {
//   console.log('server is running...')
// })

let template = require('art-template')
let comments = [
  {
    name: 'cpp',
    message: 'i love laopo',
    date: '2020-12-2'
  },
  {
    name: 'cjz',
    message: 'i love game',
    date: '2020-12-2'
  },
  {
    name: 'clg',
    message: 'i love music',
    date: '2020-12-2'
  }
]
//简写
http
.createServer((req,res)=> {
  let url = req.url
  if(url === '/') {
    fs.readFile('./views/index.html',(err,data) => {
      if(err) {
        return res.end('404 not found')
      }
      let htmlStr = template.render(data.toString(),{
        comments: comments
      })
      res.end(htmlStr)
    })
  }else if(url = '/post') {
    fs.readFile('./views/post.html',(err,data)=> {
      if(err) {
        return res.end('404 not found')
      }
      res.end(data)
    })
  }else if(url.indexOf('/public/') === 0) {
    //  /public/css/main.css
    //  /public/js/main.js
    //  /public/lib/jquery.js
    //  统一处理
    //    如果请求路径是以 /public/ 开头的，则我认为你要获取public中的某个资源
    //    所以我们就直接可以把请求路径当作文件路径来直接进行读取

    fs.readFile('.' + url,function(err,data) {
      if(err) {
        return res.end('404 not found')
      }
      res.end(data)
    })
    // console.log(url)
  }else {
    fs.readFile('./views/404.html',(err,data) => {
      if(err) {
        return res.end('404 not found')
      }
      res.end(data)
    })
  }
})
.listen(3000,()=> {
  console.log('server is running....')
})


```

+ 一次请求对应一次响应，响应结束这次请求也结束了

一个res.end()为一次响应

+ 重定向302

  + 浏览器只要发现状态码是302
  + 就会去响应头部找Location，重新对location的路径发请求
  + 自动跳转到location对应的路径页面

+ Express

+ + 第三方web开发框架
  + 高度封装了http模块
  + 更加专注于业务，而非底层细节

+ each是art-template的模板语法，专属的

  + {{each 数组}}
  + <li>{{$value}}</li>
  + {{/each}}

  

+ IE8不支持forEach



+ juery的each方法

  + 伪数组是对象
  + 对象的原型链中没有forEach
  + 对象的原型链是Object.proptptype
  + 这个each是jquery提供的
  + 这个each在jquery的原型链中

  ```javascript
  //index为第一个参数，item为第二个参数
  $('div').each(function(index.item) {
  
  console.log(item)
  
  })
  ```

+ jQuery 不是专门用来遍历jQuery元素的

  + 1. 方便的遍历 jQuery 元素
  + 2. 可以在不兼容 forEach的低版本浏览器中使用jQuery 的 each 方法

  ```
  ;[].slice.call($('div')).forEach((item)=> {
  console.log(item)
  })
  ```

  

## Node.JS是什么

1. 

+ Node.js不是一门语言

+ Node.js不是库，不是框架
+ Node.js是一个Javascript运行时环境
+ 简单来讲就是Node.js可以解析和执行Javascript代码
+ 以前只有浏览器可以解析执行JavaScript代码
+ 也就是说现的JavaScript可以完全脱离浏览器来运行，一切都归功于Node.js

2. 浏览器中的JavaScript

+ EcmaScript
  + 基本的语法
  + if
  + var
  + function
  + Object
  + Array
+ BOM
+ DOM

3. Node.js中的JavaScript

+ 没有BOM、DOM
+ EcmaScript
+ 在Node这个JavaScript执行环境中为JavaScript提供了一些服务器级别的操作API
  + 例如文件的读写
  + 网络服务的构建
  + 网络通信
  + http服务器

4. 构建于Chrom的v8引擎之上

+ 代码只是具有特定格式的字符串而已
+ 引擎可以执行它，引擎可以帮你去解析和执行
+ Google Chrome的v8引擎是目前公认的解析执行Javascript最快的
+ Node.js的作者

4. Node.js的特性

+ event-driven事件驱动
+ non-blocking I/O model非阻塞IO模型（异步）
+ 轻量和高效



+ npm是世界上最大的开源库生态系统



## Node.js能做什么

+ Web服务器后台
+ 命令行工具
  + npm(node)
  + git(c语言)
  + hexo(开发)

## 这门课能学到什么

+ B/S编程模型

  + Browser-Swrver
  + back-end

  + 任何服务端技术这种BS编程模型都是一样的，和语言无关
  + Node只是作为我们学习BS编程模型的一个工具而已

+ 模块化编程

  + RequireJS
  + SeaJS
  + @import('文件路径')
  + 以前认知的JavaScript只能通过script标签来加载
  + 在Node中可以像@import()一样来引用加载JavaScript脚本文件

+ Node常用API

+ 异步编程

  + 回调函数
  + Promise
  + async
  + generator

+ Express Web开发框架

+ Ecmascript6

## Node.js学习

### Node.js入门

1. 浏览器中的js是没有文件读取的能力的
2. 但是Node中的js具有文件操作的能力
3. fs是filesystem的简写，就是文件系统的意思
4. 在Node中如果想要进行文件操作，就必须引入fs这个核心模块
5. 例如：fs.readFile就是用来读取文件的

#### 一、读取文件

```js
//1.使用require方法加载fs核心模块
let fs = require('fs')

//2.读取文件
//  第一个参数就是要读取的文件路径
//  第二个参数是一个回调函数
//    error
//        如果读取失败，error就是错误对象
//        如果读取成功，error就是null
//    data
//        如果读取成功，data就是读取到的数据
//        如果读取失败，error就是错误对象
//    成功
//        data 数据
//        error null
//    失败
//        data null
//        error 错误对象
fs.readFile('./data/test.txt',function(error,data) {
  //<Buffer 63 70 70 20 63 6a 7a 20 63 6c 67 0d 0a 69 20 6c 6f 76 65 20 79 6f 75>
  //文件中存储的其实都是二进制数据0 1
  //这里为什么看到的不是0和1？原因是二进制转为16进制console.log(data.toString())
  //所以我们可以通过toString方法把其转为我们能认识的字符
  //console.log(data)
  console.log(data.toString())
  //读取成功test.txt内容
  // cpp cjz clg
  // i love you
  // 你好吗老公
})

//在里面添加判断
 //在这里可以通过判断error判断是否有错误发生
 if(error) {
  console.log("读取文件失败了")
  // return 
 }else {
  console.log(data.toString())
 }
```

#### 二、文件写入

```js
let fs = require('fs')

//第一个参数：文件路径
//第二个参数：文件内容
//第三个参数：回调函数

//成功： 
//  文件写入成功
//  error是null
//失败：
//  文件写入失败
//  error就是错误对象
fs.writeFile('./data/test.md','lg hello^-^',function(error) {
  console.log("文件写入成功")
})


//在里面添加判断
fs.writeFile('./data/test>.txt','lg hello^-^',function(error) {
  if(error) {
    console.log('文件写入失败')
  }else {
    console.log('文件写入成功')
  }
})
```

#### 三、最简单的http服务

```js
//1、加载http模块
let http = require('http')


//2、使用http.createServer()方法创建一个web服务器
let server = http.createServer()

//3、服务器要干嘛？
//   提供服务：对数据的服务
//   发请求
//   接收请求
//   给个反馈（发送响应）
//   注册request请求事件
//   当客户端请求过来，就会自动触发服务器的request请求事件，然后执行第二个参数：回调函数
server.on('request',() => {
  console.log('收到客户端的请求了')
})

//4、绑定端口号：启动服务器
//命令行里面服务器不动，在等待浏览器的请求，每在浏览器输入路径，回车，都会发送一次请求
server.listen(3000,function() {
  console.log('服务器启动成功了，可以通过http://127.0.0.1:3000/ 来进行访问')
})
```
#### 四、根据不同的路径相应不同的内容
```js
let http = require('http')

//1.创建server
let server = http.createServer()
//2.监听request请求事件，设置请求处理函数
server.on('request',(req,res)=> {
  console.log("收到请求了，请求路径是："+req.url)
  // res.write('hello')
  // res.write('cpp')
  // res.end()


  //上面的方式比较麻烦，推荐使用更简单的方式，直接end的同时发送响应数据
  // res.end('hello cpp')

  //根据不同的请求路径发送不同的响应结果
  //1.获取请求路径
  //  req.url获取到的是端口号之后的那一部分路径
  //  也就是说，所有的url都是以 / 开头的
  //2.判断路径处理响应

  let url = req.url
  // res.end(url)
  // if(url === '/') {
  //   res.end('index page')
  // }else if(url === '/login') {
  //   res.end('login page')
  // }else {
  //   res.end('404 not found')
  // }

  if(url === '/lover') {
    let lover = [
      {
        name: 'cpp',
        age: 2
      },
      {
        name: 'cjz',
        age: 3
      },
      {
        name: 'lg',
        age: 22
      }
    ]
    //响应的内容只能是二进制数据和字符串，其他的都不行
    //JSON.parse()转为对象
    //JSON.stringify()转为字符串
    res.end(JSON.stringify(lover))
  }
 
})
//3.绑定端口号，启动服务
server.listen(3000,function() {
  console.log('服务器启动成功，可以访问了。。。')
})

```





## Node中的JavaScript
+ EcmaScript

  + 没有BOM\DOM
+ 核心模块
+ 第三方模块
+ 用户自定义模块

#### 核心模块

Node为JavaScript提供了很多服务器级别的API、这些API大多数都被包装到了一个具名的核心模块中了。

例如文件操作的fs核心模块、http服务构建的http模块、path路径操作模块、os操作系统信息模块



如果要使用核心模块

```javascript
let fs = require('fs')
let http = require('http')
```

```javascript
//a.js
//require方法有两个作用
//  1、加载文件模块并执行里面的代码
//  2、拿到被加载文件模块导出的接口对象

//  在每个文件模块中都提供了一个对象：exports
//  exports默认是一个空对象
//  要做的就是把所有需要被外部访问的成员挂载到exports中
let ret = require('./b')

let fs = require('fs')
console.log(ret)

console.log(ret.foo)
console.log(ret.add(10,20))
console.log(ret.lover)
console.log(ret.aaa)
ret.readFile('./a.js')
fs.readFile('./a.js',function(err,data) {
  if(err) {
    console.log('读取文件失败')
  }else {
    //会把a.js文件里面的源码输出去
    console.log(data.toString())
  }
})
```

```javascript
//b.js
let foo = 'bbb'
let aaa = 'aaa'
let lover = {
  name: 'cjz',
  age: 18
}
exports.foo = 'hello'
exports.aaa = aaa
// console.log(exports)
exports.readFile = function(path,callback) {
  console.log('文件路径：' ,path)
}
exports.add = function(x,y) {
  return x + y
}

exports.lover = lover
```

```
控制台

```

## 4、Web服务器开发



### 4.1IP地址和端口号的概念

#### 所有联网的程序都需要进行网络通信

+ **计算机中只有一个物理网卡，而且同一个局域网中，网卡的地址必须是唯一的**
+ **网卡是通过唯一的ip地址来进行定位的**



www.baidu.com

DNS ->ip地址



+ **IP地址用来定位具体的应用程序**
+ **端口号用来定位具体的应用程序**

(所有需要联网通信的的软件都必须具有端口号)

+ ip地址用来定位计算机
+ 端口号用来定位具体的应用程序
+ 一切需要联网通信的软件都会占用一个端口号
+ 端口号的范围从0-65536之间
+ 在计算机中有一些默认端口号，最好不要去使用
  + 例如http服务的80
+ 在开发中使用一些简单好记的就可以了，比如3000，5000没有什么含义
+ 可以同时开启多个服务，但一定要确保不同服务占用的端口号不一致才可以
+ 说白了，在一台计算机中，同一个端口号同一时间只能被一个程序占用

### 4.2结合fs模块和Content-Type

```javascript
//1、结合fs发送文件中的数据
//2、Content-type
//    http://tool/oschina.net/commons
//    不同的资源对应的Content-Type是不一样的
//    图片不需要指定编码
//    一般只为字符数据才指定编码

let http = require('http')
let fs = require('fs')

let server = http.createServer()

server.on('request',(req,res) => {
  let url = req.url;
  if(url === '/') {
    //node动态读取文件，每次刷新重读文件，所以不用重启
    fs.readFile('./resource/index.html',function(err,data) {
      if(err) {
        res.setHeader('Content-Type','text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      }else {
        //data默认是二进制，可以通过toString转为咱们能识别的字符串
        //res.end()支持两种类型，一种是二进制，一种是字符串
        res.setHeader('Content-Type','text/html; charset=utf-8')
        res.end(data)
      }
    })
  }else if(url === '/monv') {
    //url:统一资源定位符
    //一个url最终其实是要对应到一个资源的
    fs.readFile('./resource/monv.jpg',(err,data) => {
      if(err) {
        res.setHeader('Content-Type','text/plain; charset="utf-8')
        res.end('文件读取失败，请稍后重试！')
      }else {
        //图片就不需要指定编码了，因为我们常说的编码一般是：字符编码
        res.setHeader('Content-Type','image/jpeg')
        res.end(data)
      }
    })
  }
})

server.listen(3000,function() {
  console.log('Server is running')
})
```

+ [content-type]: http://tool.oschina.net

+ **fs模块常用方法**

fs.readdir读取目录，输出为数组

### 4.3请求对象Request





### 4.4响应对象



### 4.5在Node中使用模板引擎



#### 服务端渲染

+ 说白了就是在服务端使用模板引擎
+ 模板引擎最早诞生于服务端，后来才发展到了前端
  + 服务端渲染和客户端渲染的区别
    + 客户端渲染不利于 SEO 搜索引擎优化
    + 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
    + 所以你会发现真正的网站既不是纯异步也不是纯服务端渲染出来的
    + 而是两者结合来做的
    + 例如，京东的商品列表就采用的服务端渲染，目的是为了 SEO 搜索引擎优化
    + 而它的商品评论列表是为了用户体验，而且也不需要SEO 优化，所以采用客户端渲染

浏览器收到 HTML 相应内容之后，就要开始从上到下依次解析，如果发现：

​	link,script,img,iframe,video,audio等具有 src 或者 href（link不是a链接） 属性标签（具有外链的资源）的时候，浏览器会自动对这些资源(称为静态资源)发起新的请求。一个资源对应一个请求。

+ 客户端渲染和服务器端渲染的区别
  + 最少两次请求，发起 ajax 在客户端使用模板引擎渲染
  + 客户端拿到的就是服务端已经渲染好的

## 5. 留言本



## 6. Node中的模块系统

使用Node编写应用程序主要就是在使用

+ EcmaScript
  + 和浏览器不一样，在Node中没有Bom和Dom
+ 核心模块
  + 文件操作的fs
  + http服务的http
  + url路径操作模块
  + path路径处理模块
  + os操作系统信息
+ 第三方模块
  + art-template
  + 必须通过npm来下载才可以使用
+ 咱们自己写的模块
  + 自己创建的文件



### 6.1 什么是模块化

+ 文件作用域
+ 通信规则
  + 加载require
  + 导出exports

### 6.2 CommmonJS模块规范

在Node中的Javascript还有一个很重要的概念，模块系统

+ 模块作用域
+ 使用require方法用来加载模块
+ 使用exports接口对象导出模块中的成员



+ 注意，如果一个模块需要直接导出某个成员而非挂载的方式
+ 那这个时候使用

```javascript
module.exports = 'hello'
module.exports = add
```

#### 6.2.1 加载```require```

语法：

```javascript
//let 自定义变量名称 = require('模块')
let http = require('http')
let fs = require('fs')
```

两个作用：

+ 执行被加载模块的代码
+ 得到被加载模块中的```exports```导出的接口对象

#### path模块

+ path.basename
  + 获取一个路径的文件名（默认包含扩展名）
+ path.dirname
  + 获取一个路径中的目录部分
+ path.extname
  + 获取一个路径中的扩展名部分
+ path.parse
  + 把一个路径转为对象
    + root 根路径
    + dir 目录
    + base 包含后缀名的文件名
    + ext 后缀名
    + name 不包含后缀名的文件名
+ path.join
  + 当你需要进行路径拼接的时候，推荐使用这个方法
+ path.isAbsolute
  + 判断一个路径是否是绝对路径

```shell
C:\Users\wang>node
Welcome to Node.js v12.15.0.
Type ".help" for more information.
> path.basename('c:/a/b/c/index.js')
'index.js'
> path.basename('c:/a/b/c/index.js','.js')
'index'
> path.dirname('c:/a/b/c/index.js')
'c:/a/b/c'
> path.extname('c:/a/b/c/index.js')
'.js'
> path.extname('c:/a/b/c/index.html')
'.html'
> path.isAbsolute('c:/a/b/c/index.html')
true x_x

> path.parse('c:/a/b/c/index.html')
{
  root: 'c:/',
  dir: 'c:/a/b/c',
  base: 'index.html',
  ext: '.html',
  name: 'index'
}

> path.join('c:/a/','b')
'c:\\a\\b'  
```

#### Node中的其他成员

在每个模块中，除了`require`,`exports`等模块相关API之外，还有两个特殊的成员

+ `__dirname`**动态获取**可以用来获取当前文件模块所属目录的绝对路径
+ `__filename`**动态获取**可以用来获取当前文件的绝对路径
+ `dirname`和`filename`是不受执行 node 命令所属路径影响的

在文件操作中，使用相对路径是不可靠的，因为在 Node 中文件操作的路径设计为相对于执行node命令所处的路径

所以为了解决这个问题问题，只需要把相对路径变为绝对路径就可以了。（不过这样就写死了）

这里我们就可以使用`__dirname`或者`__filename`来帮我们解决这个问题

在拼接路径的过程中，为了避免手动拼接带来的一些低级错误，所以推荐多使用`path.join()`方法来辅助拼接。

```js
// 手动拼接，要考虑后面拼接的路径带不带 / ，可能写错带来错误
console.log(__dirname + '/a.txt')
fs.readFile(__dirname + '/a.txt', 'utf8', (err, data) => {
  if (err) {
    throw err
  } 
  console.log(data)
})

// 使用path.join()方法拼接
const path = require('path')
fs.readFile(path.join(__dirname, 'a.txt'), 'utf8', (err, data) => {
  if (err) {
    throw err
  } 
  console.log(data)
})
```



#### 6.2.2 导出```exports```

+ Node中是模块作用域，默认文件中所有的成员只在当前文件模块有效
+ 对于希望可以被其他模块访问的成员，我们就需要把这些公开的成员都挂载到```exports```接口对象中就可以了

导出多个成员（必须在对象中）：



```javascript
exports.a = 123
exports.b = 'hello'
exports.c = function() {
    console.log('ccc')
}
exports.d = {
    foo: 'bar'
}
```

导出单个成员（拿到的就是，函数、字符串）：

```javascript
module.exports = 'hello'
//以这个为准，后者会覆盖前者
module.exports = function(x,y) {
    return x + y
}
```

也可以这样来导出多个成员

```javascript
module.exports = {
	add: function() {
	return x + y
	}
	str: 'hello'
}
```



#### 6.2.3 原理解析

exports 是 ```module.exports```的一个引用

```javascript
console.log(exports === module.exports) //=>true

exports.foo = 'bar'

//等价于
module.exports.foo = 'bar'
```

#### 6.2.4 exports和modules.exports的区别

\- exports 和 module.exports 的区别

 \+ 每个模块中都有一个 module 对象

 \+ module 对象中有一个 exports 对象

 \+ 我们可以把需要导出的成员都挂载到 module.exports 接口对象中

 \+ 也就是：`moudle.exports.xxx = xxx` 的方式

 \+ 但是每次都 `moudle.exports.xxx = xxx` 很麻烦，点儿的太多了

 \+ 所以 Node 为了你方便，同时在每一个模块中都提供了一个成员叫：`exports`

 \+ `exports === module.exports` 结果为 `true`s

 \+ 所以对于：`moudle.exports.xxx = xxx` 的方式 完全可以：`expots.xxx = xxx`

 \+ 当一个模块需要导出单个成员的时候，这个时候必须使用：`module.exports = xxx` 的方式

 \+ 不要使用 `exports = xxx` 不管用

 \+ 因为每个模块最终向外 `return` 的是 `module.exports`

 \+ 而 `exports` 只是 `module.exports` 的一个引用

 \+ 所以即便你为 `exports = xx` 重新赋值，也不会影响 `module.exports`

 \+ 但是有一种赋值方式比较特殊：`exports = module.exports` 这个用来重新建立引用关系的

 \+ 之所以让大家明白这个道理，是希望可以更灵活的去用它

\- Node 是一个比肩 Java、PHP 的一个平台

 \+ JavaScript 既能写前端也能写服务端

### 6.2.5 require方法的加载规则

+ 核心模块
  + 模块名
+ 第三方模块
  + 模块名
+ 用户自己写的
  + 路径



+ 优先从缓存加载
  + 加载一个模块的时候，会先看这个模块有没有被加载过，
  + 如果缓存中有，直接拿，没有就重新加载

### 6.3 npm

```
--save会保存依赖项
//建议执行npm install的时候，都加上--save，目的是为了保存依赖项
//如果你的 node_modules 删除了也不用担心，我们只需要 npm install 就会自动把 package.json 中的依赖项下下来
//加了--save在package.json文件中，dependence里面会包含这个依赖
```

+ node package manager

### 6.3.1 npm网站

npmjs.com npm的包网站

### 6.3.2  命令行工具

npm的第二层含义就是一个命令行工具，只要你安装了node就已经安装

npm也有版本这个概念

```npm --version```

升级npm

```
npm install --global npm
```

#### 6.3.3 npm常用命令

+ npm init -y可以跳过向导，快速生成
+ npm install
  + 一次性把dependence中的依赖项全部安装
  + npm i
+ npm install 包名
  + 只下载
  + npm i 包名
+ npm install --save 包名
  + 下载并且保存依赖项(package.json文件中的dependence)
  + npm i -S 包名
+ npm unstall 包名
  + 只删除，如果有依赖项依然会保存
+ npm unstall --save 包名
  + 删除的同时也会把依赖信息去掉
  + npm un -S 包名
+ nom help
  + 查看使用帮助
+ npm 命令 --help
  + 查看指定命令使用帮助
  + 例如我忘记 unstall 命令的简写了，可以通过npm unstall --help来查看帮助

#### 6.3.4 解决 npm 被墙问题

cnpm

淘宝的团队把npm在国内做了一个备份

安装cnpm

```
npm install --global cnpm
```

如果不想安装```cnpm```又想使用淘宝的服务器来下载

```
npm install jquery --registry=https://registry.npm.taobao.org
```

但是每一次手动这样添加参数很麻烦，所以我们可以把这个选项加入配置文件中

```
npm config set registry https://registry.npm.taobao.org 

查看npm配置信息
npm config list
```



### 6.4 package.json



## 7. Express

原生的http在某些方面不足以应对我们的开发需求，所以我们就需要使用框架来加快我们的开发效率，让我们的代码更加高度统一

在Node中，有很多web开发框架，我们这里以学习```express```为主

+ https://expressjs.com/
+ express封装的http

作者：tj

koa  express

node作者：Ryan Dahl

### 7.1 起步

#### 7.1.1 安装

```
npm install --save express
```

#### 7.1.2 hello world

```javascript
const express = require('express')
const app = express()

app.get('/',(req,res) => res.send('hello world'))
app.listen(3000,()=> console.log('app is running at port 3000'))
```

#### 7.1.3 基本路由

路由器

+ 请求方法
+ 请求路径
+ 请求处理函数

get：

```javascript
//当你以 GET方法请求 / 的时候，执行对应的处理函数
app.get('/',(req,res)=> res.send('hello world'))
```

post:

```javascript
//当你以 GET方法请求 / 的时候，执行对应的处理函数
app.post('/',(req,res) => res.send('got a post request'))
```

#### 7.1.4 静态服务

```
// 1.
app.use('/public/',express.static('./public/'))
// 2.别名
app.use('/a/',express.static('./public/'))
// 3.省略public
app.use(express.static('./public'))

```



```
//app.js

// 0.安装
// 1.引包
let express = require('express')

//2.创建你的服务器应用程序
//  也就是原来的http.createServer
let app = express()

//在express中开放资源就是一个api的事
//公开指定目录
//只要这样做了，public目录就被公开了
//你就可以直接通过 /public/xx 的方式访问 public 目录中的所有资源了
app.use('/public/',express.static('./public/'))
app.use('/static/',express.static('./static/'))
app.use('/node_modules/',express.static('./node_modules/'))

//模板引擎，在 express 中也是一个api的事


//当服务器收到get请求 / 的时候，执行回调处理函数
// app.get('/',(req,res)=>{
//     res.send(`
//         <!DOCTYPE html>
//         <html lang="en">
//         <head>
//             <meta charset="UTF-8">
//             <title>Title</title>
//         </head>
//         <body>
//             <h1>hello 你好 express</h1>
//         </body>
//         </html>
//             `)
// })

app.get('/',(req,res)=>{
    console.log(req)
    console.log(req.query)
    res.send('hello express')
})
app.get('/pinglun',(req,res) => {
    //req.query
    //在express中使用模板引擎有更好的方式： res.render('文件名),{模板对象}
})
app.get('/about',(req,res)=>{
    res.send('你好，我是 express')
})

//相当于server.listen
app.listen(3000,function() {
    console.log('app is running at port 3000')
})



```



### 7.2 在Express中配置使用art-template模板引擎

+ [art-template-GitHub仓库](https://github.com/aui/art-template)
+ [art-template-官方文档](https://aui.github.io/art-template/zh-cn/index.html)

```javascript
var express = require('express');
var app = express();

// view engine setup
app.engine('art', require('express-art-template'));
app.set('view', {
    debug: process.env.NODE_ENV !== 'production'
});
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'art');

// routes
app.get('/', function (req, res) {
    res.render('index.art', {
        user: {
            name: 'aui',
            tags: ['art', 'template', 'nodejs']
        }
    });
});
```



安装：

```shell
npm install --save art-template
npm install --save express-art-template
```

配置：

```javascript
//不想以.art结尾可以用例如.html替换
app.engine('art', require('express-art-template'));
```

使用：

```javascript
app.get('/'(req,res) => {
	//express 默认会去项目中的 views 目录找 index.html
	res.render('index.html',{
		title: 'hello world'
	})
})
```

如果希望修改默认的```views```视图渲染目录，可以

```javascript
//注意第一个参数views千万别写错
app.set('views',目录路径)
```

### 7.3 在Express中获取表单GET请求参数

Express内置了一个API，可以直接通过```req.query```来获取

```javascript
req.query
```

### 7.4 在Express中获取表单POST请求体数据

在Express中没有内置获取表单POST请求体的API，这里我们需要使用一个第三方包： ```body-parser```

[express安装插件](http://expressjs.com/)

[body-parser](http://expressjs.com/en/resources/middleware/body-parser.html)

安装：

```shell
npm install --save body-parser
```

配置：

```javascript
var express = require('express')
//0.引包
var bodyParser = require('body-parser')

var app = express()

//配置body-parser
//只要加入这个配置，则在 req 请求对象上会多出来一个属性： body
//也就是说可以直接通过 req.body 来获取表单 POST 请求体数据了

// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())

```

使用：

```javascript
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  //可以通过req.body来获取表单 POST 数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

### 7.5在express中配置使用express-session插件

```shell
npm install express-session
```

[express-session](https://www.npmjs.com/package/express-session)

引入：

```js
const session = require('express-session')
```

```js
// 1. npm install express-session
// 2. 配置
// 3. 使用
//    当把这个插件配置好后，我们就可以通过 req.session 来访问
//    添加 Session 数据： req.session.foo = 'bar'
//    访问 Session 数据： req.session.foo
```

配置：

```js
app.use(session({
  // 配置加密字符串。它会在原有加密基础上和这个字符串拼接起来
  // 目的是为了增加安全性，防止客户端恶意伪造
  secret: 'itcast',  
  resave: false,
  saveUninitialized: true  // 无论你是否使用 Session ，我都默认给你
}))
```

使用：

```js
// 添加 Session 数据
req.session.foo = 'bar'
// 获取Session 数据
req.session.foo
```

提示，默认Session数据是内存存储的，服务器一旦重启就会丢失，真正的环境会把Session进行持久化存储

#### express中间件



## 8. MongDB

### 8.1 关系型数据库和非关系型数据库

表就是关系

或者说表与表之间存在关系

+ 所有关系型数据库都要通过```sql```语言来操作
+ 所有的关系型数据库在操作之前都需要设计表结构
+ 而且数据表还支持约束
  + 唯一的
  + 主键
  + 默认值
  + 非空
+ 非关系型数据库非常灵活
+ 有的非关系型数据库就是key-value对
+ 但是MongoDB是长得最像关非系型数据库
  + 数据库 -》数据库
  + 数据表 -》集合（数组）
  + 表记录 -》 文档对象
+ MongDB不需要设计表结构
+ 也就是说你可以任意地往里面存数据，没有结构性这么一说

### 8.4连接和退出数据库

连接：

```shell
# 该命令默认连接本机的MongoDb服务
mongo
```

退出：

```shell
# 在连接状态输入exit退出连接
exit
```

### 8.5 基本命令

+ `show dbs`
  + 查看显示所有的数据库
+ `db`
  + 查看当前操作的数据库
+ `use数据库名称`
  + 切换到指定的数据库(如果没有会新建)
+ 插入数据

```shell
> use itcast

> db
itcast

# 没有内容，show dbs里面还是没有
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB

# 添加内容
> db.students.insertOne({"name":"cpp"})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("5fea8e8bbc4e7c0bc1719ce9")
}
# 此时show dbs
> show dbs
admin   0.000GB
config  0.000GB
itcast  0.000GB   # 多了新建的
local   0.000GB

> show collections
students

> db.students.find()
{ "_id" : ObjectId("5fea8e8bbc4e7c0bc1719ce9"), "name" : "cpp" }
>


```

### 8.6 在Node中如何操作MongoDB数据库

#### 使用官方的mongodb包操作

##### 8.6.1 使用官方的 `mongodb` 包来操作



##### 8.6.2 使用第三方 mongoose 来操作 MongoDB数据库

第三方包： `mongoose`基于`MongoDB`包再做了一次封装

```shell
npm install mongoose
```

hello world

```js
const mongoose = require('mongoose');
// 连接mongodb数据库
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});

// 创建一个模型
// 就是在设计数据库
// mongodb是动态的，非常灵活，只需要在代码中设计你的数据库就可以了
// mongoose这个包可以让你的设计编写过程变的非常简单
const Cat = mongoose.model('Cat', { name: String });

for (let i = 0; i < 100; i++) {
  // 实例化一个Cat
  const kitty = new Cat({ name: '喵喵' + i });
  // 持久化保存kitty实例
  kitty.save().then(() => console.log('meow'));
}
```

#### MongoDB数据库的基本概念

+ 可以有多个数据库
+ 一个数据库中可以有多个集合（表）
+ 一个集合可以有多个文档（表记录），文档结构很灵活，没有任何限制
+ MongoDB非常灵活，不需要像MySQL一样先创建数据库、表、设计表结构
  + 在这里只需要，当你需要插入数据的时候，只需要指定住哪个数据库的集合操作就可以了
  + 一切都由MongoDB来帮你自动完成建库建表这件事

```
{
	qq: {
		users: [
			{name: 'cpp',age: 2},
			{name: 'cpp',age: 2},
			{name: 'cpp',age: 2},
			{name: 'cpp',age: 2},
			...
		]
	},
	taobao: {
		products: [
		
		]
	},
	baidu: {
		
	}
}
```

#### 起步

[https://mongoosejs.com/docs/guide.html]()

安装

```shell
npm install mongoose
```

hello world

```js
const mongoose = require('mongoose');
// 连接mongodb数据库
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});

// 创建一个模型
// 就是在设计数据库
// mongodb是动态的，非常灵活，只需要在代码中设计你的数据库就可以了
// mongoose这个包可以让你的设计编写过程变的非常简单
const Cat = mongoose.model('Cat', { name: String });

for (let i = 0; i < 100; i++) {
  // 实例化一个Cat
  const kitty = new Cat({ name: '喵喵' + i });
  // 持久化保存kitty实例
  kitty.save().then(() => console.log('meow'));
}
```

####  官方指南

##### 设计 Schema 发布 Model

```js
import mongoose from 'mongoose'
const { Schema } = mongoose;
// 连接数据库
// 指定连接的数据库不需要存在， 当你插入第一条数据之后就会被自动创建出来
mongoose.connect('mongodb://locallhost/test')
// 设计集合结构（表结构）
// 字段名称就是表结构中的属性名称
// 值
// 约束的目的是为了保证数据的完整性，不要有脏数据
const userSchema = new Schema({
    username: {
      type: String,
      required: true,  // 必须有
    },
    password: {
      type: String,
      required: true
    },
    email: {
      type: String
    }
  });

  // 3. 将文档结构发布为模型 使用mongoose.model
  // mongoose.model方法就是用来将一个架构发布为model的
  // 第一个参数： 传入一个大写名词单数字符串用来表示你的数据库名称，
  //    mongoose会自动将大写名词的字符串生成小写复数的集合名称
  //    例如这里的 User 最终会变为 users 集合名称
  // 第二个参数：架构Schema
  //  
  // 返回值：模型构造函数
  const User = mongoose.model('User', userSchema)

  // 4. 当我们有了模型构造函数之后，就可以使用这个构造函数对users中的数据为所欲为了

```

##### 增加数据

```js
const admin = new User({
  username: 'admin',
  password: '123456',
  email: 'admin@admin.com'
})
// 持久化
admin.save((err, ret) => {  // ret就是刚刚插入的数据
  if (err) {
    console.log('保存失败')
  } else {
    console.log('保存成功')
    console.log(ret)
  }
})
```

##### 查询数据

**查询所有**

```js
User.find((err, ret) => {
  if (err) {
    console.log('查询失败')
  } else {
    console.log('查询成功')
    console.log(ret)
  }
})
```

**按条件查询所有**

```js
//按条件查,即使查询到的只有一个也会放到数组里面
User.find({
  username: 'cpp'
}, (err, ret) => {
  if (err) {
    console.log('查询失败')
  } else {
    console.log(ret)
  }
})

```

**按条件查询单个**

```js
User.findOne({
  username: 'cpp'
}, (err, ret) => {
  if (err) {
    console.log('查询失败')
  } else {
    console.log(ret)
  }
})
```

##### 删除数据

```js
User.remove({
  username: 'admin'
}, (err, ret) => {
  if (err) {
    console.log('删除失败')
  } else{
    console.log('删除成功')
    console.log(ret)
  }
})

// 终端
删除成功
{ n: 3, ok: 1, deletedCount: 3 }
```

```shell
// 查询
> db.users.find()
{ "_id" : ObjectId("5feaa8e382fa840cc4a7032d"), "username" : "admin", "password" : "123456", "email" : "admin@admin.com", "__v" : 0 }
{ "_id" : ObjectId("5feaabf892e1b219e865c660"), "username" : "admin", "password" : "123456", "email" : "admin@admin.com", "__v" : 0 }
{ "_id" : ObjectId("5feaac23e0eeb70a209736fb"), "username" : "admin", "password" : "123456", "email" : "admin@admin.com", "__v" : 0 }
{ "_id" : ObjectId("5feab0f00e400c35b0740fb8"), "username" : "wqj", "password" : "lovelg", "email" : "wqj@lovelg.com", "__v" : 0 }
{ "_id" : ObjectId("5feab1160b9747181860f5eb"), "username" : "cjz", "password" : "lovelp", "email" : "cjz@lovewqj.com", "__v" : 0 }

// 删除admin之后，查询
> db.users.find()
{ "_id" : ObjectId("5feab0f00e400c35b0740fb8"), "username" : "wqj", "password" : "lovelg", "email" : "wqj@lovelg.com", "__v" : 0 }
{ "_id" : ObjectId("5feab1160b9747181860f5eb"), "username" : "cjz", "password" : "lovelp", "email" : "cjz@lovewqj.com", "__v" : 0 }
```

##### 更新数据

+ **根据条件更新所有**

```js
Model.update(conditions, doc, [options], [callback])
```

+ **根据指定条件更新一个**

```js
Model.findByIdAndUpdate([conditions], [update], [options], [callback])
```

+ **根据id更新一个**

```js
User.findByIdAndUpdate('5feacc1ac9a20934c81b350e', {
  password: 'lovewqj'
}, (err, ret) => {
  if (err) {
    console.log('更新失败')
  } else {
    console.log('更新成功')
    console.log(ret)
  }
})
```

+ **更新后查看结果**

```js
查询成功
[
  {
    _id: 5feab0f00e400c35b0740fb8,
    username: 'wqj',
    password: 'lovelg',
    email: 'wqj@lovelg.com',
    __v: 0
  },
  {
    _id: 5feab1160b9747181860f5eb,
    username: 'cjz',
    password: 'lovelp',
    email: 'cjz@lovewqj.com',
    __v: 0
  },
  {
    _id: 5feacc1ac9a20934c81b350e,
    username: 'cpp',
    password: 'lovewqj',   //之前的密码为lovelpp，修改更新后
    email: 'cjz@lovewqj.com',
    __v: 0
  }
]
```

#### 使用Node.js操作MySQL数据库

安装

```shell
npm i --save mysql
```



## 9. 其他

### 9.1 代码风格

### 9.2 修改完代码自动重启

我们这里可以使用一个第三方命令行工具：```nodemon```来帮我们解决频繁修改代码重启服务器

```nodemon```是一个基于Node.js开发的一个第三方命令行工具，我们使用的时候需要独立安装：

```shell
#在任意目录执行该命令都可以
#也就是说，所有需要 --global 来安装的包都可以在任意目录执行
npm install --global nodemon
```

安装完毕之后，使用

```shell
node app.js

#使用 nodemon
nodemon app.js
```

只要是通过```nodemon app .js```启动的服务则它会监视你的文件变化，当文件变化的时候，自动帮你重启服务器

### 9.3 文件操作路径和模块路径

+ 文件操作路径

```javascript

//在文件操作的相对路径中
//  ./data/a.txt相对于当前目录
//  data/a.txt  相对于当前目录
//  /data/a.txt 绝对路径，当前文件所属磁盘根目录
//  C:/xx/xx... 绝对路径
// fs.readFile('/data/a.txt',(err,data) => {
//   if(err) {
//     console.log(err)
//     return console.log('读取失败')
//   }
//   // console.log('读取成功')
//   // console.log(data)
//   console.log(data.toString())
// })
```

+ 模块操作路径

```javascript
//模块加载中，加了 / ，  .也不能省略
//这里忽略了点 . 
//也是磁盘根目录

//不加 . 报错
//Error: Cannot find module '/data/foo.js'
require('/data/foo.js')

//相对路径
require('./data/foo.js')
```

## 10. 综合案例

### 2. 模板页



### 3. 路由设计

| 路径      | 方法 | get参数 | post参数                  | 是否需要登录 | 备注         |
| --------- | ---- | ------- | ------------------------- | ------------ | ------------ |
| /         | get  |         |                           |              | 渲染首页     |
| /register | get  |         |                           |              | 渲染注册页面 |
| /register | post |         | email、nickname、password |              | 处理注册请求 |
| /login    | get  |         |                           |              | 渲染登陆页面 |
| /login    | post |         | email、password           |              | 处理登录请求 |
| /loginout | get  |         |                           |              | 处理退出请求 |



