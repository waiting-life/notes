## React入门

### 一、React概念学习

#### 1、react简介

1. React构建用户界面的Javascript库，主要用于构建UI界面

 	2. 声明式的设计
 	3. 高效，采用虚拟DOM来实现DOM的渲染，最大限度的减少DOM的操作
 	4. 灵活，跟其他库灵活搭配使用
 	5. JSX,俗称JS里面写HTML，JavaScript语法的扩展
 	6. 组件化、模块化、代码容易复用
 	7. 单向数据流。没有实现数据的双向绑定。数据》视图》事件》数据

```
<Container>
	<Header/>
	<Sider/>
	<Content/>
</Container>

//eg:Header组件，实际上是一个类
class Header extends Component() {
	constructor(props) {
		super(props)
	}
	render() {
		return <h1>React</h1>
	}
}

//组件复用，在其他地方也可以使用
<App>
	<Header/>
	<Main/>
</App>
```

+ React 是一个 UI 库，通过使用 React，我们可以更方便直观地构建复杂的页面，这也是大部分 UI 库的目的
+ React 最大的特点就是将页面结构进行组件化，根据页面结构划分出一个个组件，这样代码结构就会更清晰，有利于我们开发和后期维护
+ React 组件化之后，因为相关逻辑的代码都放在一起了，而且组件间也很大程度上解耦了，我们可以很方便地复用组件
+ React 本身只是个 UI 库，现在我们看到复杂庞大的 React 全家桶是在 React 的基础上衍生出的其他的库，通常跟 React 组合在一起使用，React 本身是没有集成这些功能的
+ Create React App 不会处理后端逻辑或操纵数据库；它只是创建一个前端构建流水线（build pipeline），所以你可以使用它来配合任何你想使用的后端。它在内部使用 [Babel](https://babeljs.io/) 和 [webpack](https://webpack.js.org/)，

**必须要用webpack和babel把react来把它编译成合法的合适的js代码**
#### 2、react开发环境

```
npx create-react-app my-app
cd my-app
npm start
```

### Next.js

[Next.js](https://nextjs.org/) 是一个流行的、轻量级的框架，用于配合 React 打造**静态化和服务端渲染应用**。它包括开箱即用的**样式和路由方案**，并且假定你使用 [Node.js](https://nodejs.org/) 作为服务器环境。

### Gatsby

[Gatsby](https://www.gatsbyjs.org/) 是用 React 创建**静态网站**的最佳方式。它让你能使用 React 组件，但输出预渲染的 HTML 和 CSS 以保证最快的加载速度。

从 Gatsby 的[官方指南](https://www.gatsbyjs.org/docs/)和[入门示例集](https://www.gatsbyjs.org/docs/gatsby-starters/)了解更多。

###  二、React基本练习

#### 1. React练习

```jsx
//index.js入口文件
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />,document.querySelector('#root'))
//ReactDOM.render(<App />,document.getElementById('root'))

//App.js
import React,{Component} from 'react'

// import React from 'react'
//const Component = React.Component

class App extends Component {
  render() {
    return (
      <div>
        Hello cpp
      </div>
    )
  }
}

export default App;
```

#### 2. JSX

+ JSX ——>javascript 和 xml
+ 方便地利用html来创建虚拟dom
+ 当遇到<会把我们语法当成html解析
+ 当遇到{会把我们语法当成js解析

#### 3. JSX实例

+ 外部都要包裹一个div，如果不想要这个div，可以引入Fragment

```jsx
//div
import React,{Component} from 'react'
class Lg extends Component {
  render() {
    return (
      <div>
        <div><input/><button>吃饭</button></div>
        <ul>
          <li>螺蛳粉</li>
          <li>无界</li>
        </ul>
      </div>
    )
  }
}
export default Lg;


//Fragment,控制台检查代码，没有div了

import React,{Component,Fragment} from 'react'
class Lg extends Component {
  render() {
    return (
       //flex
      <Fragment>
        <div><input/><button>吃饭</button></div>
        <ul>
          <li>螺蛳粉</li>
          <li>无界</li>
        </ul>
       </Fragment>
    )
  }
}
export default Lg;
```

+ React数据驱动，不直接操作dom
+ 增加数据在constructor里面增加

##### 3.1 初始化数据

```jsx
//constructor里面数据初始化 
constructor(props) {
    super(props)
    //所有的数据都要写在this.state里面

    this.state = {
      inputValue: 'cpp',
      list: []
    }
  }

```

##### 3.2 数据绑定

```jsx
//注意绑定this
<input value={this.state.inputValue} onChange={this.inputChange.bind(this)}/>

//绑定了还得触发事件才能有效果，在render函数外面
 inputChange(e) {
    // console.log(e.target.value)
    // console.log(this)
     
    //注意，不能直接在state里面操作，会报错
    // this.state.inputValue = e.target.value

    //调用setState方法，设置
    this.setState({
      inputValue: e.target.value
    })
  }

```

##### 3.3 动态循环数据

```jsx
//注意，在循环的列表上，要绑定key，最好不要直接用index
//map函数，遍历数组，有返回值，返回一个新数组
<ul>
    {
        this.state.list.map((item,index) => {
            return <li key={index+item}>
                {item}
            </li>
        })
    }
</ul>
```

##### 3.4 给button增加点击事件

```jsx
<button onClick={this.addList.bind(this)}>Love</button>

//render函数下面
addList() {
   this.setState({
     //扩展运算符
     list:[...this.state.list,this.state.inputValue],
     //点击后清空输入框
     inputValue: ''
    }) 
  }
```

##### 3.5 删除list列表选项

+ splice()用于添加或者删除数组中的元素，**会改变原始数组**
+ 返回值，如果仅删除一个元素，则返回一个元素的数组。 如果未删除任何元素，则返回空数组。

```jsx
//删除列表项
  deleteItem(index) {
    // this.setState({
    //   list: this.state.list.
    // }) 
    // console.log(index)
    //采用先声明局部变量，然后删除
    let list = this.state.list
    list.splice(index,1)
    this.setState({
      list: list
    })

    //也不能这样写,坑^-^,性能会受到很大影响
    // this.state.list.splice(index,1)
    // this.setState({
    //   list: this.state.list
    // })

    //自己写法，错了，因为这样是直接把返回值给了list，即删除的那一项
    //应该把改变后的list数组赋值给list
    //splice()方法会改变原数组，返回所删除的那一项
    // this.setState({
    //   list: this.state.list.splice(index,1)
    // })
  }
```

##### 3.6 JSX的坑

```jsx
<Fragment>
    {/* 第一次写注释 */}
    <div>
        {/* 注意要写成htmlFor,因为for也是js语法里的 */}
        <label htmlFor="lg">lg</label>
        <input id="lg" className='input' value={this.state.inputValue} onChange={this.inputChange.bind(this)}/>
        <button onClick={this.addList.bind(this)}>Love</button>
    </div>
    <ul>
        {
            this.state.list.map((item,index) => {
                return (
                    <li key={index+item} 
                        onClick={this.deleteItem.bind(this,index)}
                        //就可以解析html标签
                        dangerouslySetInnerHTML={{__html:item}}
                        >
                        {/* {item} */}
                    </li>
                )
            })
        }
    </ul>
</Fragment>
```

##### 3.7 拆分组件

```jsx
//LgItem组件
import React, { Component } from 'react';

class LgItem extends Component {
 
  render() { 
    return (  
      <li>小lg</li>
    );
  }
}
export default LgItem;
 
//在Lg组件里面导入
import LgItem from './LgItem'

//在模板里面应用LgItem组件
<ul>
{
    this.state.list.map((item,index) => {
        return (
            <div>
            	<LgItem></LgItem>
            </div>
        )
    })
}
</ul>
```

##### 3.8 父子组件传值

```jsx
//1.最好在constructor里面给方法绑定this，后面就可以直接写函数，不用bind
class Lg extends Component {
  constructor(props) {
    super(props)
    this.deleteItem = this.deleteItem.bind(this)
    this.inputChange = this.inputChange.bind(this)
    this.addList = this.addList.bind(this)
    this.onKeyup = this.onKeyup.bind(this)
    //所有的数据都要写在this.state里面

    this.state = {
      inputValue: '',
      list: ['cpp','cjz','clg']
    }
  }
    
//2.父子组件传值，值和方法，父组件中
<Fragment>
        {/* 第一次写注释 */}
        <div>
          <label htmlFor="lg">lg</label>
          <input id="lg" className='input' value={this.state.inputValue} onKeyUp={this.onKeyup} onChange={this.inputChange}/>
          <button onClick={this.addList}>Love</button>
        </div>
        <ul>
          {
            this.state.list.map((item,index) => {
              return (
                <LgItem 
                  key={index+item} 
                  content={item}
                  index={index}
                  deleteItem={this.deleteItem}
                  />
              )
            })
          }
        </ul>
      </Fragment>
    
    
//3.子组件中
class LgItem extends Component {
  constructor(props) {
    super(props)
    this.handleClick = this.handleClick.bind(this)
  }
  render() { 
    return (  
      <li onClick={this.handleClick}>{this.props.content}</li>
    );
  }
  handleClick() {
    // console.log(this.props.index)
    this.props.deleteItem(this.props.index)
  }
}
```

+ 给input输入框添加回车清空内容功能

```jsx
//onKeyUp事件，当回车键的keyCode:13
<input id="lg" className='input' value={this.state.inputValue} onKeyUp={this.onKeyup} onChange={this.inputChange}/>

//记得在constructo里面添加
this.onKeyup = this.onKeyup.bind(this)


//增加列表
addList() {
    this.setState({
      list: [...this.state.list,this.state.inputValue],
      inputValue: ''
    }) 
  }

//onKeyup函数
onKeyup(e) {
    console.log(e)
    if(e.keyCode === 13) {
      this.addList()
    }
  }

```

**React单向数据流**：要是想改变父组件，可以让父组件传递个方法给子组件，然后子组件通过这个方法改变。

**React可以和jquery一起使用**

**函数式编程**：

+ 代码会非常清晰
+ 函数式编程对于代码测试有了极大的方便，更容易地实现前端自动化测试。

##### 3.9 React调试

安装调试工具

##### 3.10 PropType校验传递的值

+ **在子组件引入**

```jsx
import PropTypes from 'prop-types'

LgItem.propTypes = {
  lover:PropTypes.string.isRequired,
  content: PropTypes.string,
  index: PropTypes.number,
  deleteItem: PropTypes.func
}
// 默认值
LgItem.defaultProps={
  name: 'lg'
}

//加了isRequired，说明必须传lover值
//defaultProps设置默认值
```

##### 3.11 ref的使用

```jsx
<Fragment>
        {/* 第一次写注释 */}
        <div>
          <label htmlFor="lg">lg</label>
          <input id="lg" 
                 className='input' 
                 value={this.state.inputValue} 
                 onKeyUp={this.onKeyup} 
                 onChange={this.inputChange}
                 ref={(input)=>{this.input=input}}/>
          <button onClick={this.addList}>Love</button>
        </div>
        <ul ref={(ul) => this.ul=ul}>
          {
            this.state.list.map((item,index) => {
              return (
                <LgItem 
                  lover="wqj"
                  key={index+item} 
                  content={item}
                  index={index}
                  deleteItem={this.deleteItem}
                  />
              )
            })
          }
        </ul>
      </Fragment>


//input输入框值改变的时候，不用再用e事件赋值
 inputChange() {
    // console.log(e.target.value)
    // console.log(this)
    // this.state.inputValue = e.target.value

    // this.setState({
    //   inputValue: e.target.value
    // })

    
    this.setState({
      inputValue:this.input.value
    })  
  }

//使用ref时的坑
//this.ul.querySelectorAll('li').length打印列表的数量，出错，因为setState()方法是异步，里面还没执行完就打印了后面，所以会先输出后面的打印语句
//处理方法，react里面增加了回调函数，在添加完数据后再调用该回调函数，所以不会出错

addList() {
    // setState是异步，所以直接下面输出的结果不对，对虚拟dom的渲染也是有时间的
    //所以react里面增加了回调函数
    if(this.state.inputValue === '') return
    this.setState({
      list: [...this.state.list,this.state.inputValue],
      inputValue: ''
    },()=> {
      console.log(this.ul.querySelectorAll('li').length)
    }) 
    // console.log(this.ul.querySelectorAll('li').length)
  }
```



+ 使用ref时的坑

setState()方法进行状态改变，是异步的方法，不是同步的

##### 3.12 react生命周期函数

```jsx
UNSAFE_componentWillMount() {
    console.log('UNSAFE_componentWillMount-------组件将要挂载到页面的时刻')
  }
  componentDidMount() {
    console.log('componentDidMount--------组件挂载完成的时刻')
  }
  shouldComponentUpdate() {
    console.log('shouldComponentUpdate-------组件更新之前执行')
    //返回false的时候直接不执行更新后的render，后面的UNSAFE_componentWillUpdate直接不执行
    // return false
    return true
  }
  
  UNSAFE_componentWillUpdate() {
    console.log('UNSAFE_componentWillUpdate')
  }
  componentDidUpdate() {
    console.log('componentDidUpdate------组件更新完毕，渲染了虚拟dom之后执行')
  }

render() {
    
}


//有prop的组件里面
//组件第一次存在于dom中，函数是不会执行的
  //如果已经存在于dom中，函数才会被执行
  UNSAFE_componentWillReceiveProps() {
    console.log('child-UNSAFE_componentWillReceiveProps')
  }

```

##### 3.13 解决性能问题

## React学习

### 起步

**定义**：React是用于构建用户界面的Javascript库

​	是一个将数据渲染为HTML视图的开源JavaScript库。

### React的特点

1. 采用组件化模式、声明式编程，提高开发效率及组件复用率。
2. 在React Native中可以使用React语法进行移动端开发。
3. 使用虚拟DOM+优秀的Diffing算法，尽量减少与真实DOM的交互。

#### React高效地原因

1. 使用虚拟（virtual）DOM不总是直接擦作页面真实DOM
2. DOM Diffing算法，最小化页面重绘。

### React的基本使用

#### Hello React

```jsx
// 1. 创建虚拟DOM
// 2. 渲染虚拟DOM到页面
```



#### 关于虚拟DOM

1. 本质是Object类型的对象（一般对象）
2. 虚拟DOM比较“轻”，真实DOM比较“重”，因为虚拟DOM是React内部在用，无需真实DOM上那么多属性。
3. 虚拟DOM最终会被转化为真实DOM，呈现在页面上

#### JSX

1. 全称Javascript XML
2. react定义的一种类似XML的扩展语法：JS+XML
3. 

**JSX语法规则**

1. 定义虚拟DOM时，不要写引号
2. 标签中混入JS表达式时要引用{}
3. JSX里面样式的类名不要用class，要用className
4. 内联样式，要用style={{key: value}}的形式去写。
5. 虚拟DOM必须只有一个根标签
6. 标签必须闭合
7. 标签首字母
   + 若小写字母开头，则将该标签改为html中同名元素，若html中无该标签对应的同名元素，则报错。
   + 若大写字母开头，react就去渲染对应的组件，若组件没有被定义，则报错。

### React面向组件编程

#### 1. 函数式组件

```html
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
    // 1. 函数式组件,首字母要大写
    // React调用的函数
    function MyComponent() {
      // 一般情况下此时this指向window，babel编译后，开启了严格模式，严格模式下没有window
      console.log(this)  // undefined
      return <h2>我使用函数定义的组件（适用于【简单组件】的定义）</h2>
    }
    // 2. 渲染组件到页面
    ReactDOM.render(<MyComponent/>, document.getElementById('test'))
    /*
      执行了ReactDOM.render(<MyComponent/>.....之后，发生了什么？)
        1. React解析组件标签，找到了MyComponent组件。
        2. 发现组件是使用函数定义的，然后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中
    */
  </script>
```

**注意**：我们用函数定义的组件（适用于【简单组件】的定义）

​			简单组件：无状态

#### 2. 类式组件

```js
<script type="text/babel">
// 1. 创建类式组件
class MyComponent extends React.Component {
  render() {
    // render是放在哪里的？-MyComponent（类）的原型对象上，供实例使用
    // redner中的this是谁？-MyComponent的实例对象<=>(MyComponent组件实例对象),(MyComponent组件对象)。
    console.log('render中的this:', this)
    return <h2>我是用类定义的组件（适用于【复杂组件】的定义）</h2>
  }
}
// 2. 渲染组件到页面
ReactDOM.render(<MyComponent/>, document.getElementById('test'))
/*
  执行了ReactDom.render(<MyComponent/>.....之后，发生了什么)
    1. React解析组件标签，找到了MyComponent组件
    2. 发现组件是使用类定义的，随后new出来该类的实例，并通过实例调用到原型上的render方法
    3. 将render返回的虚拟DOM转为真实DOM，随后呈现在页面中
*/
</script>
```

**注意**：类定义的组件（适用于【复杂组件】的定义）

​			复杂组件：有状态（state）

### 组件（实例）的三大核心属性

**注意**：这里指的组件是指用类定义的组件，因为只有类才有实例

#### 组件三大核心属性state

1. state是组件对象最重要的属性，值是对象（可以是包含多个key-value的组合）
2. 组件被称为“状态机"，通过更新组件的state来更新对应的页面显示（重新渲染组件）

 **强烈注意**

1. 组件中的render方法中的this为组件实例对象。

2. 组件自定义的方法中this为undefined，如何解决？

   a. 强制绑定this：通过函数对象的bind()

   b. 箭头函数

3.  状态数据不能直接修改，要借助setState

#### 组件三大核心属性props

**注意**：props是只读的不可以修改





#### 组件三大核心属性refs与事件处理

##### 1. 字符串形式的ref

**注意**：过时API，不推荐使用

**存在一些问题**：效率不高

```
<div>
<input ref="input1" type="text" placrholder="点击按钮提示数据"/>&nbsp;
<button ref="btn" onClick={ this.showData1 }>点我提示左侧的数据</button>&nbsp;
<input ref="input2" onBlur={ this.showData2 } type="text" placeholder="失去焦点提示数据"/>
</div>
```

##### 2. 回调函数形式的ref

```jsx
// 创建组件
class Demo extends React.Component{
  showData1 = () => {
    console.log(this)
    const { input1 } = this
    alert(input1.value)
  }
  showData2 = () => {
    // 展示右侧输入框的数据
    const { input2 } = this
    alert(input2.value)
  }
        render(){
            return (
  // 添加了ref后，输出this。里面的refs里面有对应的ref
  // a这个参数为ref属性所处的节点
  <div>
    <input ref={ currentNode => this.input1 = currentNode } type="text" placrholder="点击按钮提示数据"/>&nbsp;
    <button onClick={ this.showData1 }>点我提示左侧的数据</button>&nbsp;
    <input type="text" ref={c => this.input2 = c} onBlur={this.showData2}/>
  </div>
  )
 }
}
```

**注意**：如果ref回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数null，然后第二次会传入参数DOM元素。这是因为在每次渲染时会创建一个新的函数实例，所以React清空旧的ref并且设置新的。通过将ref的回调函数定义成class的绑定函数的方式可可以避免上述问题，但是大多数情况下它是无关紧要的。

##### 3. createRef的使用

```jsx
 // 创建组件
class Demo extends React.Component{
 // 给Demo的实例上追加一个a的属性
//  a = 1
/*
  React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点
  该容器是专人专用的
*/
 myRef1 = React.createRef()
 myRef2 = React.createRef()
 // 给Demo的实例上追加一个state的属性
 state = {isHot: false}
 showData1 = () => {
   console.log(this.myRef1)
   console.log(this.myRef1.current.value)
   alert(this.myRef1.current.value)
 }
 showData2 = () => {
   console.log(this.myRef2)
   console.log(this.myRef2.current.value)
   alert(this.myRef2.current.value)
 }
 render() {
   const { isHot } = this.state
   return (
     <div>
      {/*把ref所在的节点input存在this.myRef容器里面*/}
      <input type="text" ref={this.myRef1}/>
      <button onClick={this.showData1}>点我提示输入的数据</button>
      <input onBlur={this.showData2} type="text" ref={this.myRef2} placeholder="失去焦点提示数据"/>
    </div>
   )
 }
}
// 渲染组件到页面
ReactDOM.render(<Demo/>, document.getElementById('test'))
```

### 受控组件非受控组件

#### 1. 受控组件非受控组件

包含表单的组件分类

+ 受控组件

```jsx
// 定义组件
class Login extends React.Component {
  // 初始化状态
  state = {
    username: '',
    password: ''
  }

  // 保存用户名到状态中
  saveUsername = (event) => {
    this.setState({username: event.target.value})
  }

  // 保存密码到状态中
  savePassword = (event) => {
    this.setState({password: event.target.value})
  }

  // 表单提交的回调
  handleSubmit = (event) => {
    const {username, password} = this.state
    event.preventDefault()  // 组织表单默认提交，页面就i不会刷新，此时可以用ajax的方式发送提交请求，不会刷新页面
    alert(`你的用户名是${username},你的密码是${password}`)
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" name="username" onChange={this.saveUsername}/>
        <input type="text" name="password" onChange={this.savePassword}/>
        <button>点击提交</button>
      </form>
    )
  }
}
ReactDOM.render(<Login/>, document.getElementById('test'))
```



+ 非受控组件

```jsx
// 定义组件
class Login extends React.Component {
  handleSubmit = (event) => {
    event.preventDefault()  // 组织表单默认提交，页面就i不会刷新，此时可以用ajax的方式发送提交请求，不会刷新页面
    const {username, password} = this
    alert(`你的用户名是${username.value},你的密码是${password.value}`)
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" name="username" ref={currentNode => this.username = currentNode}/>
        <input type="text" name="password" ref={currentNode => this.password = currentNode}/>
        <button>点击提交</button>
      </form>
    )
  }
}
ReactDOM.render(<Login/>, document.getElementById('test'))
```

####  2. 高阶函数_函数柯里化

+ 对象的基本知识

```js
let a = 'name'
let obj = {} // {name:'tom'}
obj[a] = 'tom'
console.log(obj);
```

+ **高阶函数**：入股一个函数符合下面两个规范中的任何一个，那该函数就是高阶函数。

  1. 若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。
  2. 若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。

  + 常见的高阶函数有：Promise、setTimeout、arr.map()等等

```jsx
class Login extends React.Component {
  // 初始化状态
  state = {
    username: '',
    password: ''
  }
  // 保存表单数据到状态中
  saveFormData = (dataType) => {
    // let a = 'name'
    // let obj = {} // {name:'tom'}
    // obj[a] = 'tom'
    // console.log(obj);
    return (event) => {
      this.setState({[dataType]: event.target.value})
    }
  }
  // 表单提交的回调
  handleSubmit = (event) => {
    const {username, password} = this.state
    event.preventDefault()  
    alert(`你的用户名是${username},你的密码是${password}`)
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" name="username" onChange={this.saveFormData('username')}/>
        <input type="text" name="password" onChange={this.saveFormData('password')}/>
        <button>点击提交</button>
      </form>
    )
  }
}
```



+ **函数的柯里化**：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数函数编码形式。

```js
/* function sum(a,b,c){
    return a+b+c
} */

// 接受了三个参数，最终统一处理
function sum(a){
    return(b)=>{
        return (c)=>{
            return a+b+c
        }
    }
}
const result = sum(1)(2)(3)
console.log(result);
```

### 组件生命周期

#### 引出生命周期

**挂载**：mount

**卸载**：unmount

1. render初始化渲染、状态更新之后

```js
// 初始化渲染、状态更新之后
render() {

}
```

2. `componentDidMount`组件挂载完毕

```js
// 组件挂载完毕
componentDidMount() {

}
```

3. `componentWillUnmount`组件将要卸载时调用

```js
// 组件将要卸载调用
componentWillUnmount() {
  // 清除定时器
  clearInterval(this.timer)
}
```

4. `ReactDOM.unmountComponentAtNode`卸载组件

```js
ReactDOM.unmountComponentAtNode(document.getElementById('test'))
```

#### React生命周期

##### 组件生命周期(旧)

+ 组件生命周期图(旧)

<img src="C:/Users/wang/AppData/Roaming/Typora/typora-user-images/image-20210219142041023.png" alt="image-20210219142041023" style="zoom:50%;" />

1. **初始化阶段:** 由ReactDOM.render()触发---初次渲染
   1. constructor()
   2. componentWillMount()
   3. render()
   4. componentDidMount() =====> 常用, 一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息

2. **更新阶段:** 由组件内部this.setSate()或父组件render触发
   1. shouldComponentUpdate()
   2. componentWillUpdate()
   3. render() =====> 必须使用的一个
   4. componentDidUpdate()

3. **卸载组件:** 由ReactDOM.unmountComponentAtNode()触发
      1. componentWillUnmount() =====> 常用,一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息

##### 组件生命周期(新)

+ 组件生命周期图(新)

<img src="C:/Users/wang/AppData/Roaming/Typora/typora-user-images/image-20210219142135810.png" alt="image-20210219142135810" style="zoom:50%;" />



1. **初始化阶段**: 由ReactDOM.render()触发---初次渲染

​        1. constructor()

​        2. getDerivedStateFromProps 

​        3. render()

​        4. componentDidMount() =====> 常用，  一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息

​    2. **更新阶段:** 由组件内部this.setSate()或父组件重新render触发

​        1. getDerivedStateFromProps

​        2. shouldComponentUpdate()

​        3. render()

​        4. getSnapshotBeforeUpdate

​        5. componentDidUpdate()

​    3. **卸载组件**: 由ReactDOM.unmountComponentAtNode()触发

​        1. componentWillUnmount() =====> 常用，一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息



##### 新旧生命周期对比

新的生命周期和旧的生命周期相比，它废弃或者即将废弃`componentWillMount`、`componentWillReceiveProps`、`componentWillUpdate`这三个生命周期钩子，同时，它又提出两个新的钩子`getDederivedStateFromProps`、`getSnapshotBeforeUpdate`

+ `getSnapshotBeforeUpdate()`：

在最近一次渲染输出(提交到DOM节点)之前调用。它使得组件能在发生更改之前从DOM中捕获一些信息(例如，滚动位置)，此生命周期的任何返回值将作为参数传递给`componentDidUpdate()`

`scrollHeight`:内容区域的高度

##### 总结

+ **重要的钩子**

1. render：初始化渲染或更新渲染调用。
2. componentDidMount：开启监听，发送ajax请求。
3. componentWillUnmount：做一些收尾工作，如：清理定时器。

+ 即将废弃的钩子

1. componentWillMount
2. componentWillReceiveProps
3. componentWillUpdate

需要加上UNSAFE_前缀，以后可能被废弃，不建议使用。

### DOM的Diffing算法

  **经典面试题**:

1.  react/vue中的key有什么作用？（key的内部原理是什么？）

2.  为什么遍历列表时，key最好不要用index?



**虚拟DOM中key的作用**：

1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

  a.  旧虚拟DOM中找到了与新虚拟DOM相同的key：

​            (1).若虚拟DOM中内容没变, 直接使用之前的真实DOM

​            (2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

  b.  旧虚拟DOM中未找到与新虚拟DOM相同的key

​            根据数据创建新的真实DOM，随后渲染到到页面   

**用index作为key可能会引发的问题**：

1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作: 会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

2. 如果结构中还包含输入类的DOM：会产生错误DOM更新 ==> 界面有问题。

3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作， 仅用于渲染列表用于展示，使用index作为key是没有问题的。

**开发中如何选择key?:**

1. 最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。

2. 如果确定只是简单的展示数据，用index也是可以的。

  慢动作回放----使用index索引值作为key



   初始数据：

​     {id:1,name:'小张',age:18},

​     {id:2,name:'小李',age:19},

   初始的虚拟DOM：

     ```html
<li key=0>小张---18<input type="text"/></li>

<li key=1>小李---19<input type="text"/></li>
     ```





   更新后的数据：

​     {id:3,name:'小王',age:20},

​     {id:1,name:'小张',age:18},

​     {id:2,name:'小李',age:19},

   更新数据后的虚拟DOM：

     ```html
<li key=0>小王---20<input type="text"/></li>

<li key=1>小张---18<input type="text"/></li>

<li key=2>小李---19<input type="text"/></li>
     ```



 \-----------------------------------------------------------------



 **慢动作回放**----使用id唯一标识作为key



   初始数据：

​     {id:1,name:'小张',age:18},

​     {id:2,name:'小李',age:19},

   初始的虚拟DOM：

```html
<li key=1>小张---18<input type="text"/></li>

<li key=2>小李---19<input type="text"/></li>
```

   更新后的数据：

​     {id:3,name:'小王',age:20},

​     {id:1,name:'小张',age:18},

​     {id:2,name:'小李',age:19},

   更新数据后的虚拟DOM：

```html
<li key=3>小王---20<input type="text"/></li>

<li key=1>小张---18<input type="text"/></li>

<li key=2>小李---19<input type="text"/></li>
```

+ **经典面试题**



```jsx
 /*
      经典面试题
      1. 在react/vue中的key有什么作用？(key的内部原理是什么？)
      + **虚拟DOM中key的作用：**
        1. 简单地说：key是虚拟DOM对象的标识，在更新显示时，key起着极其重要的作用。
        2. 详细的说：当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】
                    随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：
            a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
              + 若虚拟DOM中内容没有变，直接使用之前的真实DOM
              + 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换页面中之前的真实DOM
            b. 旧虚拟DOM中未找到与新虚拟DOM相同的key
              + 根据数据创建新的真实DOM，随后渲染到页面
          
      + **用index作为key可能会引发的问题：**
        1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作：
          + 会产生没有必要的真实DOM更新 ———— 界面效果没问题，但效率低

        2. 如果结构中还包含输入类的DOM：
          + 会产生错误DOM更新 ———— 界面有问题

        3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作
          + 仅用于渲染列表用于展示，使用index作为key是没有问题的。

      + **开发中如何选择key**
        1. 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值。
        2. 如果确定只是简单的展示数据，用index是可以的。
      
      2. 为什么遍历列表时，key最好不要用index
    */


    /*
      慢动作回放-----------使用index索引值作为key
        
        初始数据：
          {id: 1, name: 'cpp', age: 2},
          {id: 2, name: 'cjz', age: 3},
        初始的虚拟DOM：
          <li key=0>cpp-----2 <input type="text"/></li>
          <li key=1>cjz-----3 <input type="text"/></li>

        更新后的数据：
          {id: 3, name: 'clg', age: 22},
          {id: 1, name: 'cpp', age: 2},
          {id: 2, name: 'cjz', age: 3},
        更新数据后的虚拟DOM：
          <li key=0>clg-----22 <input type="text"/></li>
          <li key=1>cpp-----2 <input type="text"/></li>
          <li key=2>cjz-----3 <input type="text"/></li>

    --------------------------------------------------------

      慢动作回放-----------使用id唯一标识作为key
          
          初始数据：
            {id: 1, name: 'cpp', age: 2},
            {id: 2, name: 'cjz', age: 3},
          初始的虚拟DOM：
            <li key=1>cpp-----2 <input type="text"/></li>
            <li key=2>cjz-----3 <input type="text"/></li>

          更新后的数据：
            {id: 3, name: 'clg', age: 22},
            {id: 1, name: 'cpp', age: 2},
            {id: 2, name: 'cjz', age: 3},
          更新数据后的虚拟DOM：
            <li key=3>clg-----22 <input type="text"/></li>  //询问真实DOM有没有key=3的，没有，则直接将这个虚拟DOM转为真实DOM，其他的有，便可以复用
            <li key=1>cpp-----2 <input type="text"/></li>
            <li key=2>cjz-----3 <input type="text"/></li>
    */
		class Person extends React.Component {
      state = {
        persons: [
          {id: 1, name: 'cpp', age: 2},
          {id: 2, name: 'cjz', age: 3}
        ]
      }
      add = () => {
        const {persons} = this.state
        const p = {id: persons.length+1, name: 'clg', age: 22}
        this.setState({persons: [p, ...persons]})
        // this.setState({persons: [...persons, p]})  //此时用index作为key不会有问题
      }
      render() {
        return (
        <div>
          <h2>展示人员信息</h2>
          <h3>使用index（索引值）作为key</h3>
          <button onClick={this.add}>添加一个老公</button>
          <ul>
            {
              this.state.persons.map((person, index) => {
                return <li key={index}>{person.name}-----{person.age} <input type="text"/></li>
              }) 
            }
          </ul>  
          <hr/>
          <hr/>
          <h3>使用id（数据的唯一标识）作为key</h3>
          <ul>
            {
              this.state.persons.map(person => {
                return <li key={person.id}>{person.name}-----{person.age} <input type="text"/></li>
              }) 
            }
          </ul>  
        </div>
        )
      }
    }
```

### React脚手架

#### 使用create-react-app创建react应用

##### 1. react脚手架

1. react提供了一个创建react项目的脚手架库create-react-app
2. 项目的整体技术架构为：react+webpack+es6+eslint
3. 使用脚手架开发项目的特点： 模块化、组件化、工程化

##### 2. 创建项目并启动

**第一步**：全局安装： `npm install -g create-react-app`

**第二步**：切换到想创项目的目录，使用命令：`create-react-app hello-react`

**第三步**： 进入项目文件夹： `cd hello-react`

**第四步**：启动项目：`npm start`

#### 3.  功能界面组件化编码流程



#### 组件的组合使用——TodoList案例

重要代码分析

**删除todo代码部分**

```jsx
// deleteTodo删除一个todo
deleteTodo = (id) => {
// console.log(id)
const { todos } = this.state
// todos.forEach((todo, index) => {
//  if (todo.id === id) {
//     todos.splice(index, 1)
//   }
// })
// this.setState({ todos })


// 这里用filter实现
const newTodos = todos.filter(todo => {
  return todo.id !== id
})
this.setState({todos: newTodos})
}
```

**defaultChecked只在第一次起作用**

```jsx
<label>
  <input type="checkbox" defaultChecked={doneCount === totalCount ? true : false}/>
</label>
<span>
```

**checked会报错，解决方法,借助onChange**

```jsx
<label>
  <input type="checkbox" onChange={ this.handleCheckAll } checked={ doneCount === totalCount && totalCount !== 0 ? true : false }/>
</label>
```

#### todoList案例相关知识点



  1.拆分组件、实现静态组件，注意：className、style的写法

  2.动态初始化列表，如何确定将数据放在哪个组件的state中？

​     ——某个组件使用：放在其自身的state中

​     ——某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）

  3.关于父子之间通信：

​    1.【父组件】给【子组件】传递数据：通过props传递

​    2.【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数

  4.注意defaultChecked 和 checked的区别，类似的还有：defaultValue 和 value

  5.状态在哪里，操作状态的方法就在哪里

### react ajax

#### 理解

##### 1. 说明

1. React本身只关注于界面，并不包含发送ajax请求的代码。
2. 前端应用需要通过ajax请求与后台进行交互(json数据)
3. react应用中需要集成第三方ajax库(或自己封装)

##### 2. 常用的 ajax 请求库

1.  **jQuery**： 比较重，不建议
2.  **axios** 轻量级，建议使用
   + 封装 XMLHttpRequest 对象的 ajax
   + promise 风格
   + 可以用在浏览器端和 node 服务器

#### axios



**package.json里面配置代理：**

+ 添加

```
"proxy":"http://localhost:5000"
```

**配置多个代理不能在package.json里面**:

+ 配置多个代理不能在package.json里面，在src目录下创建setupProxy.js文件，setupProxy里面不能写ES6的语法，因为不是给前端执行的，react会把这个代码加到webpack的配置里面，webpack用的都是node语法，CJS,所以这个文件里面要写CJS

+ **setupProxy**

```js
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    proxy('/api1', {  // 遇见/api1前缀的请求，就会触发该代理配置
      target: 'http://localhost:5000',  // 请求转发给谁
      changeOrigin: true,   // 控制服务器收到的响请求头中Host字段的值
      pathRewrite: {'^/api1':''}  // 重写请求路径
    }),
    proxy('/api2', {
      target: 'http://localhost:5001',
      changeOrigin: true,
      pathRewrite: {'^/api2':''}
    })
  )
}

```



**连续的解构赋值**

```js
let obj = {a: {b: { c: 1 } }}
// 一般拿c
console.log(obj.a.b.c)  // 1

// 连续解构赋值拿c
const {a:{b:{c}}} = obj
console.log(c)  // 1
```

**连续解构赋值的同时重命名**

```js
let obj = { a: { b: 1 } }
// 连续结构赋值不重命名
const { a: { b } }
console.log(b)  // 1

// 连续解构赋值的同时重命名
const { a: { b: data } }
console.log(data)  // 1
```



### github搜索案例

#### 父子传值方式

##### 获取用户输入

```js
// 获取用户的输入(不使用连续解构赋值)   
// const { inputNode } = this
// const value = inputNode.value
// console.log(inputNode.value)

// 获取用户的输入(使用连续解构赋值)
// const { inputNode : { value }} = this
// console.log(value)

// 获取用户的输入(使用连续解构赋值 + 重命名)
const { inputNode : { value : keyWord } } = this
console.log(keyWord)
```

##### 消息订阅-发布机制

1. 工具库：PubSubJS   [网址](https://github.com/mroderick/PubSubJS)
2. 下载：npm install pubsub-js --save
3. 使用
   + import PubSub from 'pubsub-js'
   + PubSub.subscribe('delete', function(data) {});  // 订阅
   + PubSub.publish('delete', data)  // 发布消息

**主要代码**

**List组件**：订阅方

```jsx
state = {
    users: [], // 初始化状态，users初始值为数组
    isFirst: true,  // 是否为第一次打开页面
    isLoading: false,  // 表示是否处于加载中
    err: '' //  存储请求相关的错误信息
  }  

  componentDidMount() {
    this.token = PubSub.subscribe('cpp', (_, stateObj) => {
      // console.log('List组件收到数据了', data)
      this.setState(stateObj)
    })
  }

  componentWillUnmount() {
    PubSub.unsubscribe(this.token)
  }

```

**Search组件**：发布方

#### github搜索案例相关知识点

1. 设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。

2. ES6小知识点：**解构赋值+重命名**

```js
let obj = {a:{b:1}}
const {a} = obj; //传统解构赋值
const {a:{b}} = obj; //连续解构赋值
const {a:{b:value}} = obj; //连续解构赋值+重命名
```



3. **消息订阅与发布机制**

​     1.先订阅，再发布（理解：有一种隔空对话的感觉）

​     2.适用于任意组件间通信

​     3.要在组件的**componentWillUnmount**中取消订阅

4. **fetch发送请求（关注分离的设计思想）**

     ```js
try {
    const response= await fetch(`/api1/search/users2?q=${keyWord}`)
    const data = await response.json()
    console.log(data);
    } catch (error) {
 console.log('请求出错',error);
 }
    ```





### React 路由

#### SPA 的理解

1. 单页Web应用(SPA)
2. 整个应用只有**一个完整的页面**
3. 点击页面中的链接**不会刷新**页面，只会做页面的**局部更新**
4. 数据都需要通过 ajax 请求获取，并在前端异步展现。

#### react-router-dom

+ 点击导航链接，引起路径的变化

+ 路径的变化被前端路由器检测到，进行匹配组件，从而展示



**注意点**

**BrowerRouter**：包在APP组件上。

**`<NavLink/>`**

+ `<NavLink/>`电机的时候自动添加active类名
+ 如果想改类名，不想叫active的话，`activeClassName="loveLg"`,点击的时候就会添加`loveLg`这个类名。

#### 路由的基本使用

1. 明确好界面中的导航区、展示区

2. 导航区的a标签改为Link标签

​      `<Link to="/xxxxx">Demo</Link>`

3. 展示区写Route标签进行路径的匹配

​      `<Route path='/xxxx' component={Demo}/>`

4. `<App>`的最外侧包裹了一个`<BrowserRouter>`或`<HashRouter>`



```jsx
// App.jsx

import { Link, Route } from 'react-router-dom'

{/*在React中靠路由链接实现切换组件---编写路由链接*/}
<Link className="list-group-item" to="/about">About</Link>
<Link className="list-group-item" to="/home">Home</Link>


// index.jsx
import {BrowserRouter} from 'react-router-dom'

// 渲染App组件到页面
ReactDOM.render(
  <BrowserRouter>
    <App/>
  </BrowserRouter>,
  document.getElementById('root')
  )
```



#### 路由组件 和 一般组件

**路由组件放到pages里面**

1. **写法不同**：

   + 一般组件：`<Demo/>`
   + 路由组件：`<Route path="/demo" component={ Demo }/>`

2. **存放位置不同**：

   + 一般组件： `components`
   + 路由组件：`pages`

3. **接收到的props不同**：

   + 一般组件：写组件标签时传递了什么，就能收到什么
   + 路由组件：接收到三个固定的属性：

   **history**:

​              go: ƒ go(n)

​              goBack: ƒ goBack()

​              goForward: ƒ goForward()

​              push: ƒ push(path, state)

​              replace: ƒ replace(path, state)

​         **location**:

​              pathname: "/about"

​              search: ""

​              state: undefined

​         **match**:

​              params: {}

​              path: "/about"

​              url: "/about"





#### NavLink封装MyNavLink

```jsx
// MyNavLink封装
import React, { Component } from 'react'

import { NavLink } from 'react-router-dom'
export default class MyNavLink extends Component {
  render() {
    return (
      <NavLink activeClassName="change-active" className="list-group-item" {...this.props}/>
    )
  }
}


{/* 使用封装后的MyNavLink组件 */}
<MyNavLink to="/about">About</MyNavLink>
<MyNavLink to="/home">Home</MyNavLink>
```

1. NavLink可以实现路由链接的高亮，通过activeClassName指定样式名
2. 标签体是一个特殊的标签属性
3. 通过`this.props.children`可以获取标签体内容



#### Switch的使用

1. 通常情况下, path和component是一一对应的关系
2. Switch可以提高路由匹配效率（单一匹配）

```jsx
import { Route, Switch } from 'react-router-dom'

// 加了Switch，如果不加Switch，会匹配所有路径对应的组件，会一个一个向下匹配，影响效率。
<Switch>
  <Route path="/about" component={About} />
  <Route path="/home" component={Home} />
  <Route path="/home" component={Test} />
</Switch>
```



**index.html是兜底的文件**

#### 解决多级路径刷新页面样式丢失的问题

​    1. public/index.html 中 引入样式时不写 ./ 写 / （常用）

​    2. public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）（适用于react）

    3. 使用HashRouter





#### 路由的严格匹配与模糊匹配

​    1. 默认使用的是模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且顺序要一致）

​    2. 开启严格匹配：`<Route exact={true} path="/about" component={About}/>`

    3. 严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由



#### **Redirect的使用** 

​    1.一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由

​    2.具体编码：

   ```jsx
<Switch>
  <Route path="/about" component={About} />
  <Route path="/home" component={Home} />
  {/* 重定向，都匹配不上就去redirect */}
  <Redirect to="/about"/>
</Switch>
   ```

+ 路由匹配都是从最开始注册到最后注册这个流程匹配的，App里面的都是先注册的。



#### 嵌套路由的使用

**嵌套路由**

​    1.注册子路由时要写上父路由的path值

​    2.路由的匹配是按照注册路由的顺序进行的

+ **Home组件里面注册了两个组件**

```jsx
<div>
    <h2>我是Home组件的内容</h2>
    <div>
      <ul className="nav nav-tabs">
        <li>
          <MyNavLink to="/home/news">News</MyNavLink>
        </li>
        <li>
          <MyNavLink to="/home/message">Message</MyNavLink>
        </li>
      </ul>
      {/* 注册路由 */}
      <Switch>
        <Route path="/home/news" component={News}/>
        <Route path="/home/message" component={Message}/>
        <Redirect to="/home/news"/>
      </Switch>
    </div>
  </div>
```





#### 向路由组件传递参数数据

**向路由组件传递参数**

##### params参数

​       路由链接(携带参数)：`<Link to='/demo/test/tom/18'}>详情</Link>`

​       注册路由(声明接收)：`<Route path="/demo/test/:name/:age" component={Test}/>`

​       接收参数：this.props.match.params

```jsx
// 传递params参数
<li key={ messageObj.id }>
  {/* 向路由组件传递params参数 */}
  <Link to={`/home/message/detail/${messageObj.id}/${messageObj.name}`}>{ messageObj.name }</Link>
</li>

{/* 声明接收params参数 */}
<Route path="/home/message/detail/:id/:name" component={Detail}/>

// 接收params参数
const { id, name } = this.props.match.params

```





##### search参数

​       路由链接(携带参数)：<Link to='/demo/test?name=tom&age=18'}>详情</Link>

​       注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`

​       接收参数：this.props.location.search

​       备注：获取到的search是urlencoded编码字符串，需要借助querystring解析

+ 借助**querystring**库

```jsx
{/* 向路由组件传递search参数 */}
<Link to={`/home/message/detail/?id=${messageObj.id}&name=${messageObj.name}`}>{ messageObj.name}</Link>

{/* search参数无需声明接收，正常注册路由 */}
<Route path="/home/message/detail" component={Detail} />

import qs from 'querystring'
// 接收search参数
const { search } = this.props.location
const { id, name } = qs.parse(search.slice(1))
```





##### state参数

​       路由链接(携带参数)：<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>

​       注册路由(无需声明，正常注册即可)：<Route path="/demo/test" component={Test}/>

​       接收参数：this.props.location.state

​       备注：刷新也可以保留住参数

    ```jsx
{/* 向路由组件传递state参数，to是一个对象 */}
<Link to={{ pathname: '/home/message/detail', state: { id: messageObj.id, name: messageObj.name } }}>{messageObj.name}</Link>

{/* state参数无需声明接收，向路由组件传递state参数 */}
<Route path="/home/message/detail" component={Detail} />

    ```

**默认路由跳转是push，可以自己调成replace**

```jsx
<Link replace={ true } to={{ pathname: '/home/message/detail', state: { id: messageObj.id, name: messageObj.name } }}>{messageObj.name}</Link>
```





##### 编程式导航

**编程式路由导航**

**借助this.prosp.history对象上的API对操作路由跳转、前进、后退**

 ```
this.prosp.history.push()
this.prosp.history.replace()
this.prosp.history.goBack()
this.prosp.history.goForward()
this.prosp.history.go()
 ```





##### withRouter的使用

对于一般组件

```jsx
import { withRouter } from 'react-router-dom'

export default withRouter(Header)
// withRouter可以加工一般组件， 让一般组件具备路由组件所特有的API
// withRouter的返回值是一个新组件
```



##### BrowserRouter与HashRouter的区别

1. 底层原理不一样：

​      BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。

​      HashRouter使用的是URL的哈希值。

2. path表现形式不一样

​      BrowserRouter的路径中没有#,例如：localhost:3000/demo/test

​      HashRouter的路径包含#,例如：localhost:3000/#/demo/test

3. 刷新后对路由state参数的影响

​      (1).BrowserRouter没有任何影响，因为state保存在history对象中。

​      (2).HashRouter刷新后会导致路由state参数的丢失！！！

4. 备注：HashRouter可以用于解决一些路径错误相关的问题。



### React UI组件库

#### material-ui（国外）

#### ant-design（国内蚂蚁金服）

1. [官网](https://ant.design/index-cn)



### Redux

1. Redux是一个专门用做状态管理的JS库，一般与redux配合使用。
2. 作用：集中式管理react应用中多个组件共享的状态。

+ redux只管理状态，不会渲染页面
+ setState修改状态的同时调用了一次render
+ redux-thunk
+ 借助connect创建容器组件
+ Provider，批量传递store
+ serve库，开启服务器
  + npm run build
  + npm i serve -g
  + serve build

```js
// 引入createStore
import { createStore, applyMiddleware } from 'redux'
// 引入为Count组件服务的reducer
import countReducer from './count_reducer'
// 引入redux-thunk，用于支持异步action
import  thunk from 'redux-thunk'
// 暴露store
export default createStore(countReducer, applyMiddleware(thunk))
```

```js
// trdux中的reducer必须是一个纯函数
[a, ...b]	// 新的array，新的地址
b.unshift(a)  //引用地址没变
```



**1.求和案例_redux精简版**

  (1).去除Count组件自身的状态

  (2).src下建立:

​      -redux

​       -store.js

​       -count_reducer.js

  (3).store.js：

​     1).引入redux中的createStore函数，创建一个store

​     2).createStore调用时要传入一个为其服务的reducer

​     3).记得暴露store对象

  (4).count_reducer.js：

​     1).reducer的本质是一个函数，接收：preState,action，返回加工后的状态

​     2).reducer有两个作用：初始化状态，加工状态

​     3).reducer被第一次调用时，是store自动触发的，

​         传递的preState是undefined,

​         传递的action是:{type:'@@REDUX/INIT_a.2.b.4}

  (5).在index.js中监测store中状态的改变，一旦发生改变重新渲染<App/>

​    备注：redux只负责管理状态，至于状态的改变驱动着页面的展示，要靠我们自己写。





### Hooks

#### React Hook/Hooks是什么？

+ Hook是React	16.8.0版本的新特性/新语法
+ 可以让你在函数组件中使用  `state`  以及其他的  React  特性

#### 三个常用的Hook

+ State  Hook:    React.useState()     // 状态
+ Effect  Hook:    React.useEffect()     // 生命周期
+ Ref  Hook:     React.useRef()

#### State  Hook



### 扩展

#### 扩展运算符

展开运算符不能展开一个对象，会报错，加了大括号就可以通过展开运算符复制一个对象

```js
const person = {
    name: 'cpp',
    age: 2
}
const person2 = {...person}

console.log(person2)
// {name: "cpp", age: 2}
```

react可以通过`ReactDOM.render(<Person {...person}/>, document.querySelector('.test'))`传值，在这里，`{...person}`中的大括号是react中遇到js时就要加大括号，但是为什么这里可以直接用展开运算符呢？——react+babel就可以用展开运算符展开一个对象了，仅仅适用于标签属性的传递

#### undefined参与数学运算为NaN

#### 构造器

```js
// 构造器是否接收props，是否传递给super，
// 类中的构造器一般能省略就省略
constructor(props) {
    super(props)
    console.log('constructor', this.props)
}
```

#### refs

##### 字符串ref： 废弃

##### 回调ref

**注意**:

1. 如果ref是以内敛的函数的方式定义的，在**更新过程**中它会被执行两次，第一次传入参数null，第二次传入参数DOM元素。
2. 通过将ref的回调函数定义成class的绑定函数的方式可以避免上述问题。

##### React.createRef:  调用后返回一个容器，该容器可以存储被ref所标识的节点。（最推荐

#### React中的事件：React通过onXxx属性指定事件处理函数

1. React中使用的是自定义（合成）事件，而不是使用原生的DOM事件——为了更好的兼容性。
2. React中的事件是通过事件委托的方式处理的（委托给组件最外层的元素）——为了高效。
3. 通过event.target得到发生事件的DOM元素。

#### React事件处理函数那里只能拿函数（如果直接调用的话必须保证调用后的函数也是一个函数。可以利用高阶函数

1. **高阶函数：**

   a. 若A函数接受的参数是一个函数，那么A可以称之为高阶函数。

   b. 若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。

   常见的高阶函数：Promise，setTimeout，arr.map等等

2. **函数的柯里化**：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。

```js
function sum(a) {
    return (b) => {
        return (c) => {
            return a+b+c
        }
    }
}
const result = sum(1)(2)(3)
```

#### 生命周期钩子

常用：

1. componentDidMount：一般做一些初始化的事

   如： 开启定时器，发送网络请求，订阅消息

2. componentWillUnmount：一般做一些收尾的事情

   如： 关闭定时器，取消订阅消息

#### 样式的模块化

```jsx
import React,{Component} from 'react'

// 我们平时引入css， 
// import './index.css'

// 模块化引入css
import hello from './index.module.css'
export default class Hello extends Component {
    render() {
        return <h2 className="heollo.title">Hello, React</h2>
    }
}
```



<img src="C:/Users/wang/AppData/Roaming/Typora/typora-user-images/image-20210609151852808.png" alt="image-20210609151852808" style="zoom:50%;" />

#### react配置代理的方式:比如3000端口请求5000端口的数据

1. 在package.json中添加：3000没有的话向5000要，只能向5000要数据。

```json
"proxy": "http://localhost:5000"
```

请求资源的时候如果代理的3000端口public有资源，不会发送请求，直接拿取public中的资源，如果没有的话会向5000端口发送请求，获取资源。如果都没有就会报错404

2. 想配置多个代理的话：创建setupProxy.js,不能用es6，只能用cjs（common.js）

```js
const proxy = require('http-proxy-middleware')
module.exports = function(app) {
    app.use(
    	proxy('/api1', {	// 遇见/api1前缀的请求，就会触发该代理配置
            target: "http://localhost:5000",	// 请求转发给谁
            changeOrigin: true,	// 控制服务器收到的请求头中Host字段的值
            pathWrite: {'^/api1': ''}	// 重写请求路径（必须）
        })，
        proxy('/api2', {
            target: "http://localhost:5001",
            changeOrigin: true,
            pathWrite: {'^/api2': ''}
        })
    )
}

// 获取数据的时候,要加api1的前缀，不加的话public里面没有就不会走代理
axios.get('http://localhost:3000/api1/students')

axios.get('http://localhost:3000/api2/students')
```



#### 前端路由原理

```html
 <a href="https://www.bilibili.com/video/BV1wy4y1D7JT?p=76" onclick="return push('/test1')">push test1</a><br/>
  <button onclick="push('/test2')">push test2</button><br/>
  <button onclick="replace('/test3')">replace test3</button><br/>
  <button onclick="back()">&lt;回退</button>
  <button onclick="forward()">前进&gt;</button>
  <script src="https://cdn.bootcss.com/history/4.7.2/history.js"></script>
  <script>
    // let history = History.createBrowserHistory() // 方法一： 直接使用h5推出的history身上的API
    let history = History.createHashHistory() // 方法二： hash值
    function push(path) {
      history.push(path)
      return false
    }
    function replace(path) {
      history.replace(path)
    }
    function back() {
      history.goBack()
    }
    function forward() {
      history.goForward()
    }
    history.listen((location) => {
      console.log('请求的路径变化了', location)
    })
  </script>
```

#### NavLink点谁给谁加一个默认的'active'的类名

如果想要加一个别的类名可以通过activeClassName来指定。

#### React默认会开启一个3000端口的服务器。

public是3000端口的默认根路径，如果请求的资源不存在，就会返回index.html，index.html为兜底文件

#### Switch组件：用Switch组件包裹路由，匹配路径的时候如果匹配到了，就不会继续向下继续匹配，如果不加默认匹配路由是如果匹配到了一个对应的路由，就会继续向下匹配其他路由，如果有匹配的继续展示，直到所有路由。

```jsx

```

#### 向路由组件传递参数

1. 向路由组件传递params参数

```jsx
return (
    <div>
      <h3>Messsage组件</h3>
      <ul>
        {
          messageArr.map(msgObj => {
            return (
              <li key={msgObj.id}>
                {/* 向路由组件传递params参数 */}
                <Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link>
              </li>
            )
          })
        }
      </ul>
      {/* 声明接收params参数 */}
      <Route path="/home/message/detail/:id/:title" component={Detail}/>
    </div>
  )

// Detail组件
 // 接收params参数
const {id, title} = props.match.params
console.log(id, title)
```

2. 向路由组件传递search参数

```jsx
{/* 向路由组件传递search参数 */}
<Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link>

{/* search参数无需声明接收,正常注册路由即可 */}
<Route path="/home/message/detail" component={Detail} />


// Detail组件
import qs from 'querystring'

// 接收search参数
const {search} = props.location
console.log(search.slice(1))
const {id, title} = qs.parse(search.slice(1))
```



3. 向路由组件传递state参数

```jsx
{/* 向路由组件传递state参数 */}
<Link to={{ pathname: '/home/message/detail', state: {id: msgObj.id, title: msgObj.title}}}>{msgObj.title}</Link>

/* state参数无需声明接收,正常注册路由即可 */}
<Route path="/home/message/detail" component={Detail} />

// Detail组件
// 接收state参数
  console.log(props)
  const {id, title} = props.location.state || {}
```

#### 编程式路由导航

```jsx
function pushShow(id, title) {
    // push携带跳转,携带params参数
    // props.history.push(`/home/message/detail/${id}/${title}`)

    // push跳转,携带search参数
    // props.history.push(`/home/message/detail/?id=${id}&title=${title}`)

    // push跳转, 携带state参数  push(pathname, state)
    props.history.push('/home/message/detail', {id, title})
}
function replaceShow(id, title) {
    // replace跳转,携带params参数
    // props.history.replace(`/home/message/detail/${id}/${title}`)

    // replace跳转,携带search参数
    // props.history.replace(`/home/message/detail/?id=${id}&title=${title}`)

    // replace跳转, 携带state参数  replace(pathname, state)
    props.history.replace('/home/message/detail', { id, title })
}



<button onClick={() => pushShow(id, title)}>push查看</button>
<button onClick={() => replaceShow(id, title)}>replace查看</button>
```

#### 想要在一般组件里面使用路由组件的API，可以使用withRouter包裹一般组件

1. withRouter可以加工一般组件，让一般组件具有路由组件所具有的属性。
2. withRouter的返回值是一个具有路由组件API的新组件

#### HashRouter和BorwserRouter的区别

1. 底层原理不一样：

   BrowserRouter使用的是H5的history API， 不兼容IE9及以下版本

   HashRouter使用的是url的hash值

2. url表现形式不一样

3. **刷新后对路由state的参数的影响**

   ​	**BrowserRouter没有任何影响，因为state保存在history中**

   ​	HashRouter刷新后会导致路由state参数的丢失

#### antd

```jsx
import {Button} from 'antd'
import 'antd/dist/antd/css'		// 引入样式
```

**antd样式的按需引入**

[antd自定义](https://3x.ant.design/docs/react/use-with-create-react-app-cn)

1. 引入react-app-rewried并修改package.json里的配置
2. 还需安装customize-cra

```
npm i react-app-rewried customize-cra --save
```





**antd自定义主题**：修改less里面样式对应的变量



#### 安装less，less-loader时报错， 按如下版本安装

```bash
npm install less@2.7.3 less-loader@4.1.0 -S
```

#### redux

##### reducers：初始化状态和加工状态

1. reducer的本质是一个函数，接收：preState和action两个参数，返回加工后的状态

2. reducer有两个作用：初始化状态，加工状态

3. reducer第一次被调用时，是store自动触发的

   传递的preState是undefined

   传递的action是type: "@@redux/INITv.f.l.p.2"

##### action

**1. 异步action**： 为函数

```jsx
// store.js
// 引入createStore专门用于创建redux的最为核心的store对象
import {createStore, applyMiddleware} from 'redux'
// 引入为Count组件服务的reducer
import countReducer from './count_reducer'

// 引入react-thunk用于支持异步action
import thunk from 'redux-thunk'
// 暴露store
export default createStore(countReducer, applyMiddleware(thunk))
```



**2. 同步actioc**： 为异步对象



#### react-redux

**类式组件**

所有的UI组件都应该包裹一个容器组件，他们是父子关系。

容器组件是真正和redux打交道的，里面可以随意地使用redux的api

容器组件会传给UI组件： 1. redux中所保存的状态。 2. 用于操作状态的方法

备注：容器组件给UI传递，：状态，操作状态的方法，均通过props传递。

1. **容器组件**

2. **UI组件**

<img src="C:/Users/wang/AppData/Roaming/Typora/typora-user-images/image-20210613171447025.png" alt="image-20210613171447025" style="zoom:50%;" />

```js
// 容器组件
import CountUI from '../../components/Count'
import {increment, decrement} from '../../redux/count_action'

// 引入connect用于连接UI组件与redux
import {connect} from 'react-redux'

// mapStateToProps用于传递状态
const mapStateToProps = state=> ({ count: state})

//  mapDispatchToProps用于传递操作状态的方法
constmapDispatchToProps = dispatch =>  ({
    handleIncrement: number => dispatch(increment(number)),
    handleDecrement: number => dispatch(decrement(number))
})

// 第一个里面接收的参数mapStateToProps，mapDispatchToProps都为函数，	
// mapStateToProps返回redux中保存的状态
// mapDispatchToProps返回用于操作状态的方法
// CountUI为UI组件
export default connect(mapStateToProps, mapDispatchToProps)(CountUI)


// UI组件里面通过this.props拿到对应的状态和方法
```

**整合上面代码**

```js
// 容器组件
import CountUI from '../../components/Count'
import {increment, decrement} from '../../redux/count_action'

// 引入connect用于连接UI组件与redux
import {connect} from 'react-redux'

export default connect(
    // 用于传递状态
    state=> ({ count: state}), 
    // 用于传递操作状态的方法
    dispatch =>  ({
        handleIncrement: number => dispatch(increment(number)),
        handleDecrement: number => dispatch(decrement(number))
 	})
)(CountUI)
```

##### 优化

1. **优化上面代码**

```js
// 容器组件
import CountUI from '../../components/Count'
import {increment, decrement} from '../../redux/count_action'

// 引入connect用于连接UI组件与redux
import {connect} from 'react-redux'

// mapDispatchToProps可以写成一个对象
export default connect(
    // 用于传递状态
    state=> ({ count: state}), 
    // 用于传递操作状态的方法, 简写，react-redux会自动分发（dispatch）
   {
       handleIncrement: increment,
       handleDecrement: decrement
   }
)(CountUI)
```



2. **继续优化**：把容器组件和UI组件整合到一个文件里面
3. **使用了react-redux不用自己监测**
4. **Provider**

```js
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import store from './redux/store'

import {Provider} from 'react-redux'
ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.querySelector('#root')
)
```





**函数式组件**

**useSelector，useDispatch**

**实现两个组件Count和Person组件间通信**

```js
// store.js

import {createStore, combineReducers} from 'redux'
import countReducer from '../reducers/count'
import personReducer from '../reducers/person'

const reducer = combineReducers({
  count: countReducer,
  persons: personReducer
})
export const store = createStore(reducer) 
```

**Count和Person组件获取store中的数据**：例如Person组件中

```jsx
import React, {useState} from 'react'
import {useSelector, useDispatch} from 'react-redux'
import {nanoid} from 'nanoid'
import { addPerson } from '../../redux/actions/person'

export default function Person() {
  const [userInfo, setUserInfo] = useState({
    name: '',
    age: '',
    id: null,
  })
  const state = useSelector(state => state)
  const {count, persons} = state
  const dispatch = useDispatch()


  const saveUserInfo = (dataType, e) => {
    setUserInfo({...userInfo, [dataType]: e.target.value,})
  }
  const handleAddPerson = () => {
    setUserInfo({...userInfo, id: nanoid()})
    dispatch(addPerson(userInfo))
  }

  return (
    <div>
      <h2>我是Perosn组件</h2>
      <h3>上方Count组件的和为：{count}</h3>
      <input type="text" placeholder="请输入姓名" value={userInfo.name} onChange={(e) => saveUserInfo('name', e)}/>
      <input type="text" placeholder="请输入年龄" value={userInfo.age} onChange={(e) => saveUserInfo('age', e)}/>
      <button onClick={handleAddPerson}>添加</button>
      <ul>
        {
          persons.map(item => (
            <li key={item.id}>name: {item.name}---------age: {item.age}</li>
          ))
        }
      </ul>
    </div>
  )
}

```



