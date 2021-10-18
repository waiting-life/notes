# React Hooks

## useState



## useEffect



## useContext

```jsx
import React, {useState, useContext} from 'react'

const themes = {
  light: {
    foreground: '#000',
    background: '#eee'
  },
  dark: {
    foreground: '#fff',
    background: '#222'
  }
}
const ThemeContext = React.createContext({
  theme: themes.light,
  toggle: () => {

  }
})

export default function ReactContext() {

  const [theme, setTheme] = useState(themes.light)
  return  <ThemeContext.Provider value = {{
    theme, 
    toggle: () => {
      // console.log(111)
      setTheme(theme === themes.light ? themes.dark : themes.light)
    }
  }}>
    <ToolBar /> 
  </ThemeContext.Provider>
}

const ToolBar = () => {
  return <ThemeButton/>
}

const ThemeButton = () => {
  const context = useContext(ThemeContext)
  return (
    <button 
      style={{fontSize: '32px', color: context.theme.foreground, backgroundColor: context.theme.background}}
      onClick={() => context.toggle()}>
      Click Me !
    </button>
  )
}

```

## reducer

```jsx
import React, {useReducer} from 'react'
function reducer(state = {count: 0}, action) {
  // console.log(state, action)
  switch (action.type) {
    case 'add':
      return {...state, count: state.count+1}
    case 'sub':
      return {...state, count: state.count - 1}
    default:
      return state.count
  }
}
export default function Count() {
  const [counter, dispatch] = useReducer(reducer, {count: 0})
  return (
    <div>
      <div>Your counter is: {counter.count}</div>
      <button onClick={() => dispatch({type: 'add'})}>+</button>
      <button onClick={() => dispatch({type: 'sub'})}>-</button>
    </div>
  )
}
```

## 引用行为ref

````jsx
import React, {useRef, useState} from 'react'

export default function ReactRef() {
  const refInput = useRef()
  const [counter, setCounter] = useState(0)
  
  // 可以通过useRef保存上一次的状态，prev是一个引用
  const prev = useRef(null)
  console.log(prev)
  return (
    <div>
      <input type="text" ref={refInput}/>
      <button 
        onClick={() => {
          refInput.current.focus()
        }}> 
        Focus
      </button>

      <hr/>
      <p>当前值：{counter}</p>
      <p>之前的值：{prev.current}</p>
      <button onClick={() => {
        prev.current =  counter
        setCounter(counter + 1)
      }}>Click me to add</button>
      <button onClick={() => {
        prev.current = counter
        setCounter(counter - 1)
      }}>Click me to remove</button>
    </div>
  )
}

````

## 缓存

### 缓存一个函数(useCallback)



### 缓存一个值(useMemo):

+ 类似计算属性，根据依赖的变量缓存计算的结果

```jsx
import React, {useState, useMemo} from 'react'

export default function ReactUseMemo() {
  const [count, setCount] = useState(0)
  const memorizedText =   useMemo(() => {
    console.log('run useMemo function');
    return  `this is a memorized text ${Date.now()}`
  }, [Math.floor(count / 10)])
  return (
    <div>
      <div>{memorizedText}</div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  )
}

```



## Tips

### 1. 使用React.memo减少重绘次数



### 2. hooks同步问题

```jsx
import React, {useEffect, useState} from 'react'

export default function ReactTips() {
  const [count, setCount] = useState(0)
  useEffect(() => {
    let timer = setInterval(() => {
      console.log(count)
      setCount(count + 1)
    }, 2000);
    return () => {
      clearInterval(timer)
    }
  }, [count])
  return (
    <div>
      {count}
    </div>
  )
}
```

### 3. 可以构造自己的hooks封装行为





## React扩展

### 1. setState更新状态的两种写法

**注意**：setState调用完后引起的后续动作是异步的（react状态的更新是异步的）

#### setState(stateChange, [callback])------对象式的setState

+ stateChange为状态改变对象(该对象可以体现出状态的更改)

+ callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用

```jsx
import React, { Component } from 'react'

export default class SetState extends Component {
  state = {
    count: 0
  }
  increment = () => {
    // 1. 获取原来的状态值
    const {count} = this.state
    // 2. 更新状态
    this.setState({count: count + 1}, () => {
      console.log(this.state.count)
    })
    console.log(this.state.count)
  }
  render() {
    return (
      <div>
        <h1>SetState....组件</h1>
        <h2>当前求和为： {this.state.count}</h2>
        <button onClick={this.increment}>点我+1</button>
      </div>
    )
  }
}

```



#### setState(updater, [callback])------函数式的setState

+ updater为返回stateChange对象的函数。
+ updater可以接收到state和props。
+ callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。

```jsx
this.setState((state, props) => {
  console.log(state, props)
  return {count: count + 1}
})
// this.setState(state => ({count: count + 1}))
```

**总结:**

  1.对象式的setState是函数式的setState的简写方式(语法糖)

  2.使用原则：

​    (1).如果新状态不依赖于原状态 ===> 使用对象方式

​    (2).如果新状态依赖于原状态 ===> 使用函数方式

​    (3).如果需要在setState()执行后获取最新的状态数据, 

​     要在第二个callback函数中读取





### 2. lazyLoad



```jsx
// Suspense当网速慢的时候显示对应的加载组件，比如loading....
import React, {lazy, Suspense} from 'react'
import { Route, Switch, NavLink } from 'react-router-dom'

// import Home from '../Home'
// import About from '../About'

import Loading from '../../components/Loading'
import './index.css'

const Home = lazy(() => import('../Home'))
const About = lazy(() => import('../About'))


export default function LazyLoad() {
    return (
      <div className="app-container">
        <div className="main-content">
          <div className="nav-list">
            <NavLink to="/home">Home</NavLink>
            <NavLink to="/about">About</NavLink>
          </div>
          <div className="show-container">
            <Suspense fallback={<Loading/>}>
              <Switch>
                <Route path='/home' component={Home} />
                <Route path='/about' component={About} />
              </Switch>
            </Suspense>
          </div>
        </div>
      </div>
    )
}
```

### 3. Hooks

#### useSatte

#### useEffect

#### useRef



### 4. Fragment：允许写key值

空标签不允许写



### 5. Context

```jsx
// 创建Context容器对象
const XxxContext = React.createContext()
// 渲染子组件时，外面包裹xxxContext.Provider, 通过value属性给后代组件传递数据
<XxxContext.Provider value={数据}>
	子组件	      
</XxxContext.Provider>



// 后代组件读取数据
//	第一种方式：是用于类组件
static contextType = XxxContext	// 声明接收context
this.context	// 读取context中的value值


// 第二种方式：函数组件和类组件都可以
<XxxContext.Consumer>
    {
    	value => (
    	要显示的内容
   	 	) 
	}
</XxxContext.Consumer>

```





#### 1. context.js

```js
import React, { useContext } from 'react'

const MyContext = React.createContext()
export const MyProvider = MyContext.Provider
export function useMyContext() {
  const context = useContext(MyContext);
  return context;
}
```

#### 2. index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import {BrowserRouter} from 'react-router-dom'
import App from './App'
import { MyProvider } from './context'


ReactDOM.render(
<MyProvider value={{name: 'w', age: 1}}>
  <BrowserRouter>
    <App/>
  </BrowserRouter>
</MyProvider>,
document.querySelector('#root'))
```



#### 3. 使用context的地方

```jsx
import React from 'react'
import { useMyContext } from '../../../../context'
export default function ChildB() {
  const context = useMyContext()
  return (
    <div>
      <h3>我是B组件，我从祖组件接收到的lover是： </h3>
      <div>{JSON.stringify(context)}</div>
      <div>{context.name}</div>
      <div>{context.age}</div>
    </div>
  )
}

```

**注意**：react中，<div>{obj}</div>当在大括号里面写对象时会报错

### 6. optimize优化

Vue和React都是数据驱动视图的更新。Vue采用对数据的劫持，因此知道具体数据的改变，当父组件发生重新渲染时子组件不一定会重新渲染；而React通常只要父组件发生了重新渲染，子组件也会进行重新渲染，这种组件被称为**非纯组件**。

为了避免额外的开销我们可以使用**纯组件**，**父组件的渲染不会触发纯组件的渲染。**

##### 类组件： React.PureComponent

```jsx
import React, { PureComponent } from 'react'

export default class index extends PureComponent {
  render() {
    return (
      <div>
        
      </div>
    )
  }
}
```



##### 函数式组件：React.memo()

```jsx
import React, {useState, memo} from 'react'

export default function ReactMemo() {
  const [count, setCount] = useState(0)
  const [name, setName] = useState('cpp')
  const increment = () => {
    setCount(count+1)
  }
  const toggleName = () => {
    setName(name === 'cpp' ? 'cjz':'cpp')
  }
  console.log('parent组件渲染')
  return (
    <div>
      <h1>父组件的count为： {count}</h1>
      <h1>父组件的name为： {name}</h1>
      <button onClick={increment}>点击+1</button>
      <button onClick={toggleName}>点击改变name</button>
      <Child name={name}/>
    </div>
  )
}

const Child = memo((props) => {
  console.log('child子组件渲染')
  console.log(props)
  return (
    <div>
      <div>子组件</div>
      <h2>子组件从父组件接收到的name为：{props.name}</h2>
    </div>
  )
})


```

### 7. render props

```jsx
import React from 'react'
import './index.css'
export default function Parent() {
  return (
    <div className="parent">
      <h1>我是parent组件</h1>
      <A render = {(name) => <B name={name}/>}>
        <div>Hello</div>
        <B/>
      </A>
    </div>
  )
}

function A(props) {
  console.log(props)
  const name = 'cpp'
  return (
    <div className="a">
      <h2>我是A组件</h2>
      {props.render(name)}
    </div>
  )
}

function B(props) {
  return (
    <div className="b">
      <h3>我是B组件, {props.name}</h3>
    </div>
  )
}
```



### 8. 错误边界： ErrorBoundary

#### 类组件： 

```
static getDerivedStateFromError()`或`componentDidCatch()
```