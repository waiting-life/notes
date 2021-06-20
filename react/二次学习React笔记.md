## 基础部分
### 创建虚拟DOM的两种方式
#### 1. jsx创建虚拟DOM
  <!-- ```jsx -->
      // 1. 创建虚拟DOM
      const VDOM = <h1>Hello,React</h1>
      // 2. 渲染虚拟DOM到页面
      ReactDOM.render(VDOM, document.getElementById('test'))
#### 2. js创建虚拟DOM(一般不用)
  <!-- ```js -->
      // 1. 创建虚拟DOM,document.createElement创建真实DOM
      //    React.createElement创建虚拟DOM
      const VDOM = React.createElement('h1', { id: 'title'}, 'Hello,React')
      // 2. 渲染虚拟DOM到页面
      ReactDOM.render(VDOM, document.getElementById('test'))

#### 3. 虚拟DOM与真实DOM
+ 关于虚拟DOM
  + 1. 虚拟DOM本质是Object类型的对象(一般对象)
  + 2. 虚拟DOM比较"轻",真实DOM比较"重"，因为虚拟DOM是React内部在用，无需真实DOM上那么多属性
  + 3. 虚拟DOM最终会被React转化为真实DOM，呈现在页面上。
### JSX语法规则
+ 1. 定义虚拟DOM时，不要写引号
+ 2. 标签中混入JS表达式时要用{}
+ 3. 样式的类名不能用class，要用className
+ 4. 内联样式要用 `style={{ key: value }}`的形式去写。
+ 5. 只有一个根标签。
+ 6. 标签必须闭合。
+ 7. 组件标签首字母必须大写

### 声明组件两种方法

#### 1. 函数式组件
+ 函数式组件里面的 `this` 为 `undefined`，因为babel编译的时候会开启严格模式，严格模式禁止自定义的函数指向window
+ 执行了 `ReactDOM.render(<MyComponent/>, document.getElementById('test'))` 之后，发生了什么？
  1. React解析组件标签，找到了MyComponent组件
  2. 发现组件时使用函数定义的，随后调用该函数
  3. 将返回的虚拟DOM转为真实DOM，然后呈现在页面上。


#### 2. 类式组件

+ 执行了 `ReactDOM.render(<MyComponent/>, document.getElementById('test'))` 之后，发生了什么？
  1. React解析组件标签，找到了MyComponent组件
  2. 发现组件时使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的人的人、方法。
  3. 将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。

+ 构造器和render
  + 构造器的this指向组件实例对象，构造器调用了1次
  + render中的this也指向组件实例对象，因为`ReactDOM.render(<MyComponent/>, document.getElementById('test'))`写了组件标签，react `new MyComponent`了一个实例，通过`实例.render()`调用。renderi盗用了`1+n`次，n为状态更新的次数。
  + 自定义的函数方法react不会调用。所以它们的this为undefined
  
+ 初始化装胎和自定义方法
  + 1. 初始化状态
    +  类中可以直接写实例属性`a=1`，意思为往实例对象上添加一个属性名为a，值为1。
  + 2. 自定义方法
    + 要用赋值语句加箭头函数`a = () => {}`
  

  **注意**：
  + 类中所有定义的方法在局部都开启了严格模式，类中直接定义的函数的this为undefined
  + 局部是可以开启严格模式的

#### 3. 简单组件与复杂组件
+ 复杂组件：有状态的
  + 状态为state，组件的状态里面存着数据，数据的改变驱动着页面的显示
  + state是组件实例对象身上的
+ 简单组件：无状态
  + 函数式组件this为undefined

### 组件的三大属性
#### 1. 组件三大属性：state
+ `setState`
  + 合并

#### 2. 组件的三大属性：props

**注意**： 
+ 原生展开运算符不能展开一个对象,
  + 构造字面量对象时使用展开语法，可以通过包含一个`{...obj}`的形式复制一个对象
+ react+babel就可以用展开运算符展开一个对象


##### 对props进行类型限制
**注意**：`propTypes` 和 `defaultProps` 要加在类自身上，所以用 `static` 声明
+ props是只读的，不能直接修改
1. 类型限制
2. 必要性限制
3. 默认值
 ```js
  static propTypes = {
        name: PropTypes.string.isRequired,
        age: PropTypes.number
      }
   static defaultProps = {
     name: 'clg',
     age: 3
   }
  ```

**注意**：如果写了constructor,super里面不写props，在构造器里面通过`this.props`获取props会输出undefined
```js
  // 构造器是否接受props，props是否传递给super，取决于是否希望在构造器中通过this访问props
      // constructor() {
      //   super()
      //   console.log('constructor', this.props) 
      //   // constructor undefined   
      //   // constructor undefined
      //   // constructor undefined
      // }

      constructor(props) {
        super(props)
        console.log('constructor', this.props)    
        // constructor {name: "cpp", age: 2}
        // constructor {name: "cjz", age: 22}
        // constructor {name: "clg", age: 3}
      }
```
#### 3. 组件的三大属性：refs
+ 组件内的标签可以使用ref来标识自己
+ 用refs获取，获取到的节点为真实dom
+ 不要过度使用ref，发生事件的DOM元素和你要操作的DOM元素是一个，直接用（event.target）

##### 3.1. 字符串形式的ref
+ 过时

##### 3.2 回调函数形式的ref
```jsx
   <div>
      <input type="text" ref={ currentNode => this.inputNode = currentNode}/>
      <button onClick={ this.getInputData }>点击获取输入框信息</button>
    </div>
```
**关于回调refs的说明**：如果ref回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数null，然后第二次会传入参数DOM元素，这是因为在每次渲染时会创建一个新的函数实例，所以React清空旧的ref，并设置新的。通过将ref回调函数定义成class的绑定函数的方式可以避免上述问题。
1. 内联函数定义ref回调函数(无伤大雅，内联方式用的多)
```jsx
  <div>
    <h2>今天天气很{ isHot ? '炎热' : '凉爽'}</h2>
    <input type="text" ref={ c => { console.log('@', c);this.inputNode = c }}/><br/>
    {/* 
     1. 页面刚渲染时调用输出了一次：@ <input type="text">
     2. 后面点击按钮触发方法的时候，该ref回调函数会执行两次
        @ null                  // 清空
        @ <input type=​"text">​
    */}
    <button onClick={ this.getInputData }>点击获取输入框信息</button><br/>
    <button onClick={ this.changeWeather }>点我切换天气</button>
  </div>
```
2. class的绑定函数定义ref回调函数

```jsx
  class Demo extends React.Component {
   getInputData = () => {
     console.log(this.inputNode)
     const { value } = this.inputNode
     console.log(value)
   }
   render() {
     return(
       <div>
         <input type="text" ref={ currentNode => this.inputNode = currentNode}/>
         <button onClick={ this.getInputData }>点击获取输入框信息</button>
       </div>
     )
   }
 }
```

##### 3.3 createRef
+ `React.createRef` 调用后可以返回一个容器，该容器可以存储被ref所标识的节点
+ 该容器是专人专用的，ref容器名字一样会覆盖，所以可能会创建很多ref的容器
  
```jsx
  class Demo extends React.Component {
    // `React.createRef` 调用后可以返回一个容器，该容器可以存储被ref所标识的节点
    myRef = React.createRef()
    myRef2 = React.createRef()
    getData = () => {
      console.log(this.myRef)  // {current: input}
      console.log(this.myRef.current.value)
    }
    getData2 = () => {
      console.log(this.myRef2.current.value)
    }
    render() {
      return(
        <div>
          {/* 这里写了ref={ this.myRef }之后就会把当前ref所在的节点(比如这里为<input/>)直接存储到这个容器myRef里面 */}
          <input type="text" ref={ this.myRef }/>
          <button onClick={ this.getData }>点击获取输入框信息</button>
          <input type="text" ref={ this.myRef2 } onBlur={ this.getData2}/>
        </div>
      )
    }
  }
```






### React中的事件处理
1. 通过onXxxx属性指定事件处理函数(注意大小写)
   a. React使用的是自定义(合成)事件，而不是使用原生DOM事件————为了更好的兼容性
   b. React中的事件是通过事件委托方式处理的(委托给最外层的元素)。————为了高效
2. 通过`event.target` 得到发生事件的DOM元素对象。


### 收集表单数据
#### 1. 受控组件(根据数据更新状态类似于vue双向数据绑定)---推荐
```jsx
  class Login extends React.Component{
   state = {
     username: 'cpp',
     password: 'lovewqj'
   }
   saveUsername = (event) => {
     this.setState({ username: event.target.value })
   }
   savePassword = (event) => {
     this.setState({ password: event.target.value })
   }
   handleSubmit = (event) => {
     event.preventDefault() // 组织表单提交
     const { username, password } = this.state
     alert(`你输入的用户名是${ usename }，你输入的密码为${ password }`)
   }
   render() {
     return (
       <form action="" onSubmit={ this.handleSubmit }>
         用户名：<input type="text" name="username" onChange={ this.saveUsername }/>
         密码：<input type="password" name="password" onChange={ this.savePassword }/>
         <button>登录</button>
       </form>
     )
   }
 }
```

#### 2. 非受控组件(现用现取)
```jsx
class Login extends React.Component{
   handleSubmit = (event) => {
     console.log(this)
     event.preventDefault() // 组织表单提交
     const { userNode, pswNode } = this
     alert(`你输入的用户名是${ userNode.value }，你输入的密码为${ pswNode.value }`)
   }
   render() {
     return (
       <form action="" onSubmit={ this.handleSubmit }>
         用户名：<input type="text" name="username" ref={ c => this.userNode = c }/>
         密码：<input type="password" name="password" ref= { c => this.pswNode = c}/>
         <button>登录</button>
       </form>
     )
   }
 }
```

### 高阶函数 柯里化
1. 高阶函数：如果一个函数符合下面2个规范中的任何一个，那么该函数就是高阶函数
  + 若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。
  + 若A函数，调用的函数返回值依然是一个函数，那么A就可以称之为高阶函数。
  + 常见的高阶函数有：Promise、setTimeout、arr.map()等等

2. 函数的柯里化化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。
   ```js
  function sum(a) {
      return (b) => {
        return (c) => {
          return a + b + c
        }
      }
    }
    console.log(sum(1)(2)(3))
   ```

3. 用到高阶函数和柯里化的例子
```jsx
class Login extends React.Component{
   state = {
     username: 'cpp',
     password: 'lovewqj'
   }
   saveFormData = (dataType) => {
     console.log(dataType)
     return (event) => {
       this.setState({ [dataType]: event.target.value })
     }
   }
 
   handleSubmit = (event) => {
     event.preventDefault() // 组织表单提交
     const { username, password } = this.state
     alert(`你输入的用户名是${ username }，你输入的密码为${ password }`)
   }
   render() {
     return (
       <form action="" onSubmit={ this.handleSubmit }>
         {/*把saveFormData的返回值作为onChange回调*/}
         用户名：<input type="text" name="username" onChange={ this.saveFormData('username') }/>
         密码：<input type="password" name="password" onChange={ this.saveFormData('password') }/>
         <button>登录</button>
       </form>
     )
   }
 }
```

+ 上述案例不用柯里化实现

```js
  class Login extends React.Component{
    state = {
      username: 'cpp',
      password: 'lovewqj'
    }
    saveFormData = (dataType, event) => {
      const { value } = event.target
      this.setState({ [dataType]: value })
    }
  
    handleSubmit = (event) => {
      event.preventDefault() // 组织表单提交
      const { username, password } = this.state
      alert(`你输入的用户名是${ username }，你输入的密码为${ password }`)
    }
    render() {
      return (
        <form action="" onSubmit={ this.handleSubmit }>
          {/*把saveFormData的返回值作为onChange回调*/}
          用户名：<input onChange={ event => this.saveFormData('username', event) } type="text" name="username" />
          密码：<input onChange={ event => this.saveFormData('password', event) } type="password" name="password"/>
          <button>登录</button>
        </form>
      )
    }
  }
```

### 组件的生命周期
**生命周期是通过React `new` 出来的组件的实例对象调用。**
+ 下载(mount)：
+ 卸载(unmount)：`ReactDOM.unmountComponentAtNode(document.getElementById('test))`

#### 1. 旧版生命周期函数
+ 初始化阶段：由ReactDOM.render()触发---初次渲染
  + 1. constructor()
  + 2. componentWillMount()
  + 3. render()
  + 4. componentDidMount()    ==常用==
    + 一般在这个钩子里面做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息

+ 更新阶段：由组件内部 `this.setState()` 或父组件render触发
  + 1. shouldComponentUpdate()
  + 2. componentWillUpdate()
  + 3. render()     ==必须使用==
  + 4. componentDidUpdate()

+ 卸载组件：由ReactDOM.unmountComponentAtNode()触发
  + 1. componentWillUnmount()     ==常用==
    + 一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息

#### 2. 新版生命周期函数
+ 初始化阶段：由ReactDOM.render()触发---初次渲染
  + 1. constructor()
  + 2. getDerivedStateFromProps
  + 3. render()
  + 4. componentDidMount()    ==常用==
    + 一般在这个钩子里面做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息

+ 更新阶段：由组件内部 `this.setState()` 或父组件render触发
  + 1. getDerivedStateFromProps
  + 2. shouldComponentUpdate()
  + 3. render()     ==必须使用==
  + 4. getSnapshotBeforeUpdate
  + 5. componentDidUpdate()

+ 卸载组件：由ReactDOM.unmountComponentAtNode()触发
  + 1. componentWillUnmount()     ==常用==
    + 一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息


### DOM的Diffing算法

**经典面试题:**
1). react/vue中的key有什么作用？（key的内部原理是什么？）
2). 为什么遍历列表时，key最好不要用index?
  
1. 虚拟DOM中key的作用：
	1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

	2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 
								随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

					a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
								(1).若虚拟DOM中内容没变, 直接使用之前的真实DOM
								(2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

					b. 旧虚拟DOM中未找到与新虚拟DOM相同的key
								根据数据创建新的真实DOM，随后渲染到到页面
					
2. 用index作为key可能会引发的问题：
				1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
								会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

				2. 如果结构中还包含输入类的DOM：
								会产生错误DOM更新 ==> 界面有问题。
								
				3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，
					仅用于渲染列表用于展示，使用index作为key是没有问题的。
	
3. 开发中如何选择key?:
				1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
				2.如果确定只是简单的展示数据，用index也是可以的。

### React路由
#### 一、概念

1. 什么是路由？
   + 一个路由就是一个映射关系(key.value)
   + key为路径，value可能是function或component

2. 路由分离
  + 后端路由
    + 理解：value是function，用来处理客户端提交的请求
    + 注册路由：router.get(path, function(req, res))
    + 工作过程：当node接收到一个请求时，根据请求路径找到匹配的路由，调用路由中的函数来处理请求，返回响应数据
  + 前端路由
    + 浏览器端路由：value是component，用于页面内容。
    + 注册路由：<Route path="test" component={ Test }>
    + 工作过程：当浏览器path变为/test时，当前路由组件就会变为Test组件

#### 二、History
**浏览器的历史记录是栈的结构**


#### 三、React-router
##### 3.1 react-router-dom 的理解
   

##### 3.2 react-router-dom 相关API
##### 3.2.1 内置组件
1. <BrowserRouter>
2. <HashRouter>
3. <Router>
4. <Redirect>
5. <link>
6. <NavLink>
7. <Switch>

##### 3.2.2 其他
1. history对象
2. match对象
3. withRouter函数

#### 3.3 基本路由的使用
##### 3.3.1 注意点
**整个应用得用一个路由器管理，所以：**
```jsx
  ReactDOM.render(
    <BrowserRouter>
      <App/>
    </BrowserRouter>,
    document.getElementById('root')
  )
```
+ 编写路由链接：Link
+ 注册路由： Route

##### 3.3.2 路由组件一般组件的区别
1. 写法不同：
   + 一般组件： `<Demo/>`
   + 路由组件：`<Route path="/demo" component={ Demo }/>`
2. 存放位置不同：
   + 一般组件：components
   + 路由组件：pages
3. **接收到的props不同：**
   + 一般组件：写组件标签时传递了什么，就能接收到什么
   + 路由组件：接受到三个固定的属性，路由组件会收到路由器给传递的三个最重要的props信息
     + history:
				go: ƒ go(n)
				goBack: ƒ goBack()
				goForward: ƒ goForward()
				push: ƒ push(path, state)
				replace: ƒ replace(path, state)
		 + location:
				pathname: "/about"
				search: ""
				state: undefined
		 + match:
				params: {}
				path: "/about"
				url: "/about"

##### 3.3.3 补充
**NavLink**:
1. `<NavLink>`如果点击了，会给当前点击的加一个类名，默认为`active`
2. 如果想要修改类名：`<NavLink activeClassName="demo"></NavLink>`,通过`activeClassName`属性来修改

**封装MyNavLink**：
```jsx
  import React, { Component } from 'react'
  import { NavLink } from 'react-router-dom'

  export default class MyNavLink extends Component {
    render() {
      // console.log(this.props)
      return (
        <NavLink activeClassName="atguigu" className="list-group-item" {...this.props}/>
      )
    }
  }
```
**路径匹配：**<Switch></Switch>组件，注册路由的时候，如果有多个路由，用`Switch`包裹
1. 通常情况下，path和component是一一对应的关系。
2. `Switch`可以提高路由匹配效率（单一匹配）

**样式丢失：**路由路径是多级的结构，一刷新就会丢失
1. 前端路由不发送网络请求
2. 如果请求的资源不存在，就会把index.html返回给你，index.html是兜底的
+ 解决办法：
  + 1. 引入样式时不要写`.`：<link rel="stylesheet" href="/css/bootstrap.css">
  + 2. 引入样式时不要写`.`：<link rel="stylesheet" href="%PUBLIC_URL%/css/bootstrap.css">
  + 3. 不用BrowserRouter, 用HashRouter 
**解决多级路径刷新页面样式丢失的问题：**
1. public/index.html 中 引入样式时不写 ./   写/ (常用)
2. public/index.html 中 引入样式时不写 ./   写%PUBLIC_URL% (常用)
3. 使用HashRouter

**路由的精准匹配和模糊匹配**
1. 模糊匹配：默认为模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且要顺序一致）

2. 精准匹配：`<Route exact path="/about" component={ About }/>`
   + 严格模式不要随便开启，需要再开，有些时候会导致无法继续匹配二级路由

**Redirect重定向**：如下例子，前面的`/about`，`/home`匹配不上的时候就会走`<Redirect/>`
+ **一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由**
```jsx
  <Switch>
    <Route path="/about" component={ About }/>
    <Route path="/home" component={ Home }/>
    <Redirect to="/about">
  </Switch>
```
**嵌套路由的使用**：
1. React中路由的注册是由顺序的，路由的匹配都是从最先注册到最后注册这个顺序走下去的



**路由组件传递参数：**
1. params参数
   	路由链接(携带参数)：`<Link to='/demo/test/tom/18'}>详情</Link>`
   	注册路由(声明接收)：`<Route path="/demo/test/:name/:age" component={Test}/>`
   	接收参数：this.props.match.params
2. search参数
   	路由链接(携带参数)：`<Link to='/demo/test?name=tom&age=18'}>详情</Link>`
   	注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
   	接收参数：this.props.location.search
   	备注：获取到的search是urlencoded编码字符串，需要借助querystring解析
3. state参数
    路由链接(携带参数)：`<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>`
    注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
    接收参数：this.props.location.state
    备注：刷新也可以保留住参数

**编程式导航：**
+ this.props.history.push()和this.props.history.replace()

**withRouter**
+ 如何让一般组件里面用上路由组件的属性？
+ withRouter函数，可以将一般组件加工为路由组件
  `withRouter(Header)`
  withRouter的返回值是一个有路由组件属性的新组件

**BrowserRouter与HashRouter的区别**
1. 底层原理不一样：
		BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
		HashRouter使用的是URL的哈希值。
2. path表现形式不一样
		BrowserRouter的路径中没有#,例如：localhost:3000/demo/test
		HashRouter的路径包含#,例如：localhost:3000/#/demo/test
3. 刷新后对路由state参数的影响
		(1).BrowserRouter没有任何影响，因为state保存在history对象中。
		(2).HashRouter刷新后会导致路由state参数的丢失！！！
4. 备注：HashRouter可以用于解决一些路径错误相关的问题。
**因为HashRouter没有用到history这个api，location在history里面所以刷新会导致路由state参数的丢失**


### redux
+ store.getState()              ————获取状态
+ store.subscribe(()=> {})      ————检测redux中状态的变化，只要变化，就调用render

```js
store.subscribe(()=>{
	ReactDOM.render(<App/>,document.getElementById('root'))
})
```


+ action
  + 1. 返回值为一般对象，同步action`{ type: '', data: ''}`
  + 2. 返回值为函数(因为函数能开启异步任务)，为异步action,但是store只接受一般对象，异步action中一般都会调用同步action
    + 所以，这个地方需要一个中间件`react-thunk`，需要让store执行一下函数，store调用后发现里面执行的是同步任务，此时就可以交给reducer来处理

```js
/* 
该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/

//引入createStore，专门用于创建redux中最为核心的store对象
import {createStore,applyMiddleware} from 'redux'
//引入为Count组件服务的reducer
import countReducer from './count_reducer'
//引入redux-thunk，用于支持异步action
import thunk from 'redux-thunk'
//暴露store
export default createStore(countReducer,applyMiddleware(thunk))
```


**index.js**里面：只要redux里面的状态发生任何变化，给App重新render，其他组件也会
```js
  store.subscribe(()=> {
    ReactDOM.render(<App/>, document.getElementById('root'))
  })
```


### react-redux

1. 所有的UI组件都应该包裹一个容器组件，他们是父子关系
2. 容器组件是真正和redux打交道的，里面可以随意的使用redux的api
3. UI组件中不能使用任何redux的api
4. 容器组件回传给UI组件：
   + 1. redux中所保存的状态
   + 2. 用于操作状态的方法
5. 备注：容器给UI传递：状态、操作状态的方法，均通过props传递。

```jsx
// 引入Count的UI组件
import CountUI from '../../components/Count'
import { createIncrementAction, createDecrementAction, createIncrementAsyncAction } from '../../redux/count_action'
// 引入connect用于连接UI组件与redux
import { connect } from 'react-redux'
/*
  1. mapStateToProps函数返回的是一个对象中
  2. 返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value
  3. mapStateToProps用于传递状态
*/
// const mapStateToProps = state => ({count: state })

/*
  1. mapDispatchToProps函数返回的是一个对象中
  2. 返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value
  3. mapDispatchToProps用于传递操作状态的方法
*/
// const mapDispatchToProps = dispatch => ({
//  add: number => dispatch(createIncrementAction(number)),  // 通知redux执行加法,思路为 // store.dispatch({ type: 'increment', data: number })
//  dec: number => dispatch(createDecrementAction(number)),
//  addAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time))
// })

// 使用connect()()创建并暴露一个Count的容器组件
// 也可以直接把函数放在这里，不用变量保存
export default connect(
  state => ({count: state }), 
  // mapDispatchToProps的一般写法
  // dispatch => ({
  //   add: number => dispatch(createIncrementAction(number)),  
  //   dec: number => dispatch(createDecrementAction(number)),
  //   addAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time))
  // })

  // mapDispatchToProps的简写，可以写成一个对象， react-redux会自动分发
  {
    add：createIncrementAction，
    dec：createDecrementAction，
    addAsync：createIncrementAsyncAction，
  }
)(CountUI)
```
**注意**：连接redux需要用到的store，需要在用到容器组件的地方传过来`<CountContainer store={ store }/>`
+ 用了react-redux，不用我们通过 `store.subscribe()`手动监测，react-redux内部做了监测，容器组件自己有监测的能力
```js
store.subscribe(()=> {
  ReactDOM.render(<App/>, document.getElementById('root'))
})
```
+ Provider
**index.js:**
```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import store from './redux/store'
import {Provider} from 'react-redux'

ReactDOM.render(
	/* 此处需要用Provider包裹App，目的是让App所有的后代容器组件都能接收到store */
	<Provider store={store}>
		<App/>
	</Provider>,
	document.getElementById('root')
)
```

### 纯函数
+ 因为redux会进行浅比较，如果返回的数据和原来的数据地址一样，它就不改变

**redux中的reducer必须为纯函数**
+ 1. 一类特别的函数：只要是同样的输入(实参)，必定得到同样的输出(返回)
+ 2. 必须遵守以下一些约束
  + a. 不得改写参数数据
  + b. 不会产生任何副作用，例如网络请求、输入和输出设备
  + c. 不能调用Date.now()或者Math.random()等不纯的方法


### setState
**react在状态的更新时候是异步的**：setState调完后引起的react后续更新状态的动作是异步的。

#### setState更新状态的两种方法
1. 对象式的setState:`setState(stateChange, [callback])`
   + `setState({},[callback])`
   + stateChange为状态改变对象
   + callback是可选的回调函数，界面更新后(render调用后)才被调用
```jsx
  this.setState({ count: count + 1 }, () => {
    console.log(this.state.count)
  })
```

2. 函数式的setState：`setState(updater, [callback])`updater为函数
   + updater为返回stateChange对象的函数
   + updater可以收到state和props
   + callback是可选的回调函数，界面更新后(render调用后)才被调用
```jsx
  this.setState((state, props)=> {
    return { count: state.count + 1 }
  }, () => {
    console.log(this.state.count)
  })
```
#### 总结
1. 对象式的setState是函数式的setState的间歇方式
2. 使用原则：
  （1）. 如果新状态不依赖于原状态   ——————使用对象方式
  （2）. 如果新状态依赖于原状态  ——————使用函数方式
  （3）. 如果需要在setState()执行后获取最新的状态数据，要在setState的第二个参数[callback]回调函数中获取



### lazyLoad懒加载
```jsx
import Loading from './Loading'
const About = lazy(() => import('./About'))
const Home = lazy(() => import('./Home'))

{/*如果组件很久没请求出来，就给用户看定义好的Loading组件*/}
<Suspense fallback={ <Loading/> }>
  <Route path='/about' component={ About }/>
  <Route path='/home' component={ Home }/>
</Suspense>

```

### Hooks
#### Hooks是什么
3. Hook可以让你在函数组件中使用state以及其他的React特性

#### 三个常用的Hook
1. State Hook:  React.useState()
2. Effect Hook: React.useEffect()
3. Ref Hook: React.useRef()

##### State Hook
1. State Hook 让函数组件也可以有state状态，并进行状态数据的读写操作
2. 语法： `const [xxx, setXxx] = React.useState(initValue)`
3. useState()说明：
   参数：第一次初始化指定的值在内部作缓存
   返回值：包含2个元素的数组，第一个为内部当前状态值，第二个为更新状态值的函数
4. setXxx()两种写法：
   setXxx(newValue): 参数为非函数值，直接指定新的状态值，内部用其覆盖原来的值
   setXxx(value => newValue):参数为函数，接受原来的状态值，返回新的状态值，内部用其覆盖原来的状态值
##### Effect Hook
1. Effect Hook 可以让你在函数式组件里面执行副作用操作(**用于模拟类组件中的生命周期钩子**)
2. React中的副作用操作：
   发ajax请求数据获取
   设置订阅/启动定时器
   手动更改真实DOM
3. 语法和说明
```jsx
React.useEffect(() => {
  // 在此处执行任何副作用操作
  let timer = setInterval(() => {
    setCount(count => count + 1)
  }, 1000);
  return () => {    // 在组件卸载前执行
    // 在此处做一些收尾工作，比如清除定时器/取消订阅
    clearInterval(timer)
  }
}, [count])
```
4. 可以把useEffect Hook看成如下三个函数的组合
   componentDidMount()
   componentDidUpdate()
   componentWillUnmount()
##### Ref Hook
1. Ref Hook 可以在函数组件中存储/查找组件内的标签或任意其他数据
2. 语法：`const myRef = React.useRef()`
3. 作用：保存标签对象，功能与React.createRef()一样

### Fragment
+ Fragment只接收一个key属性

### Context
**理解：**一种组件间通信方式，常用于【祖组件】与【后代组件间通信】
**整合容器组件，UI组件**：写在一个文件里面，暴露容器组件
**注意：**
1. 路由组件和一般组件分开放置
2. 路由组件放在pages文件夹放中
3. 一般组件放在components文件夹中
4. 注册子路由时要写上父路由的路径比如， /home/news

