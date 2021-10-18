# react学习

## 基础

### Form

React把表单分为**受控组件**和**非受控组件**

当我们给`input`元素加上`value`和`onChange`属性时，该元素被视为受控组件

### Ref和forwardRef



`forwardRef`能够暴露组件的内部DOM元素给外部使用

## Hooks

### useState

```tsx
// 返回一个state以及更新state的函数
const [state, setState] = useState(initialState)

// 如果更新的state需要通过先前的state计算得出，那么可以将函数传递给setState。该函数将接受先前的state，并返回一个更新后的值
<button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
```

**注意**： 

1. 如果你的更新函数返回后值与当前state完全相同，那么重渲染会被完全跳过。
2. initialState参数只会在组件的初始渲染时起作用，后续渲染时会被忽略。如果初始state需要通过复杂计算获得，则可以传入一个函数，在函数中计算并返回初始的state，此函数只在初始渲染时起作用。

```ts

const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});

```

3. react使用Object.is比较算法来比较state





### useEffect

使用`useEffect`完成副作用的操作，虽然useEffect会在浏览器绘制后延迟执行，但会保证在任何新的渲染前执行，在开始新的更新前，react总是会清空上一次effect



### useContext

接收一个context对象，（React.createContext的返回值）并返回该context的返回值。

当前的context值由上层组件中距离当前组件最近，<MyContext.Provider>的value prop决定

当组件上层最近的<MyContext.Provider>更新时，该Hook会触发重渲染，并使用最新传递给MyContext的provieder的context value值，即使祖先使用 [`React.memo`](https://zh-hans.reactjs.org/docs/react-api.html#reactmemo) 或 [`shouldComponentUpdate`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)，也会在组件本身使用 `useContext` 时重新渲染。

**注意**：

1. useContext的参数必须是context对象本身。`useContext(MyContext)`
2. 调用了useContext的组件总是会在context值变化时重新渲染，如果重渲染组件的开销较大，你可以通过使用memoization来优化

```ts
const context = useContext(MyContext)
```



### useMemo： 值

类似计算属性，根据依赖的变量缓存计算的结果



### useCallback： 函数

#### 使用memo+useCallback优化代码

+ 自组件没有用到changeName方法，但是点击changgeName的时候，还是会输出'Child render'
+ 使用useCallback+memo

```tsx
// Test父组件
import React, { useState, useEffect, useRef, useCallback } from 'react';
import TestModal from './TestModal';
import Child from './components/Child';
export default function Test() {
  const [count, setCount] = useState<number>(0);
  const [name, setName] = useState<string>('cpp');

  const handleIncrement = useCallback(() => {
    setCount(count + 1);
  }, [count]);
  const handleDecrement = useCallback(() => {
    setCount(count - 1);
  }, [count]);
  const changeName = () => {
    setName(name + 1);
  };
  return (
    <div>
      <h1>Test页面</h1>
      <h2>count: {count}</h2>
      <button onClick={handleIncrement}>+</button>
      <button onClick={changeName}>改变名字</button>
      <div>{count}</div>
      <div>{name}</div>
      <Child
        handleIncrement={handleIncrement}
        handleDecrement={handleDecrement}
      />
    </div>
  );
}


// Child子组件
import React, { memo } from 'react';
const Child = memo((props: any) => {
  console.log('Child render');
  const { handleIncrement, handleDecrement } = props;
  return (
    <div>
      <button onClick={handleIncrement}>+1</button>
      <button onClick={handleDecrement}>-1</button>
    </div>
  );
});
export default Child;

```



### useReducer

在函数组件中引入状态，类似`useState`但我们不是直接修改数据，而是通过`dispatch(action)`的形式修改数据。

### useRef

useRef返回一个可变的ref对象，其`.current`属性被初始化为传入的参数（initialValue），返回的对象在组件的整个生命周期内持续存在。

在组件里面使用useRef: 比如在页面点击一个按钮弹出modal

1. 首先要配合`React.forwardRef(TestModal);`使用
2. 配合`useImperativeHandle`做一些操作

```tsx
// 页面
const modalRef = useRef<any>(null);

<TestModal ref={modalRef} />

// modal
import React, { useImperativeHandle, useState } from 'react';
import type { ForwardRefRenderFunction } from 'react';

const TestModal: ForwardRefRenderFunction<any, any> = (props, ref) => {
  const [visible, setVisible] = useState<boolean>(false);
  const onVisibleChange = (visible: boolean) => {
    setVisible(visible);
  };

  useImperativeHandle(ref, () => ({
    open: () => {
      setVisible(true);
    },
  }));
  return (
    <ModalForm<{
      name: string;
      company: string;
    }>
      title="新建表单"
      modalProps={{
        onCancel: () => console.log('run'),
      }}
      onFinish={async (values) => {
        await waitTime(2000);
        console.log(values.name);
        message.success('提交成功');
        return true;
      }}
      visible={visible}
      onVisibleChange={onVisibleChange}
    >
      <ProForm.Group>
        <ProFormText
          width="md"
          name="name"
          label="签约客户名称"
          tooltip="最长为 24 位"
          placeholder="请输入名称"
        />

        <ProFormText
          width="md"
          name="company"
          label="我方公司名称"
          placeholder="请输入名称"
        />
      </ProForm.Group>
      <ProForm.Group>
        <ProFormText
          width="md"
          name="contract"
          label="合同名称"
          placeholder="请输入名称"
        />
        <ProFormDateRangePicker name="contractTime" label="合同生效时间" />
      </ProForm.Group>
      <ProForm.Group>
        <ProFormSelect
          options={[
            {
              value: 'chapter',
              label: '盖章后生效',
            },
          ]}
          width="xs"
          name="useMode"
          label="合同约定生效方式"
        />
        <ProFormSelect
          width="xs"
          options={[
            {
              value: 'time',
              label: '履行完终止',
            },
          ]}
          name="unusedMode"
          label="合同约定失效效方式"
        />
      </ProForm.Group>
      <ProFormText width="sm" name="id" label="主合同编号" />
      <ProFormText
        name="project"
        disabled
        label="项目名称"
        initialValue="xxxx项目"
      />
      <ProFormText
        width="xs"
        name="mangerName"
        disabled
        label="商务经理"
        initialValue="启途"
      />
    </ModalForm>
  );
};
export default React.forwardRef(TestModal);
```



#### ref和useRef

**ref**：如果将ref对象以`<div ref={myRef}/>`形式传入组件无论节点如何改变， React都会将ref对象的`.current`属性设置为相应的DOM节点。

**useRef**：useRef返回一个可变的ref对象，useRef比ref更有用，它可以很方便的保存任何可变值，其类似于在class中使用实例字段的方式。

这是因为它创建的是一个普通js对象，而`useRef()`和自建一个`{ current: ...}`对象的唯一区别是，useRef会在每次渲染时返回同一个ref对象。

当ref对象发生变化的时候，useRef并不会通知你，变更`.current	`属性并不会引发组件的重新渲染。如果想要在React解绑或者绑定DOM节点的ref时运行某些代码，则需要使用回调ref来实现

### useImperativeHandle

useImperativeHandle可以让你在使用时自定义暴露给父组件的实例值，`useImperativeHandle`应当与forwardRef一起使用。

### use LayoutEffect

它会在所有的 DOM 变更之后同步调用 effect，可以使用它来读取DOM布局并同步触发渲染，



## 顶层API



### React.Component



### React.PureComponent



### React.memo



### React.Children



### React.createRef



### React.forwardRef



### React.lazy



### React.Suspense



### React.Fragment



```tsx

var _state; // 把 state 存储在外面

function useState(initialValue) {
  _state = _state || initialValue; // 如果没有 _state，说明是第一次执行，把 initialValue 复制给它
  function setState(newState) {
    _state = newState;
    render();
  }
  return [_state, setState];
}

function Test() {
  let a = 100;
  const [count, setCount] = useState(1) = [_state, setState] 
  a = 200
  setCount(2)

  const B = useCallback(
    () => {
      
    },
    [],
  )
  return (
    <Child onClick={B}/>
  )
}


// 因为useRef创建的对象ref在函数重新渲染时地址不会改变，所以persistFn将持久化存储。
```

每次渲染都会重新调用函数，创建一个函数作用域

useState相当于在全局有一个作用域变量state

```tsx
// state = 2;

// globalFn = () => {
//   console.log(s);
// };

export default function Test() {
  const [s, setS] = useState(1);
  const tempFn = () => {
    console.log(s);
  };
  const fn = useCallback(tempFn, []);
  // console.log(fn === tempFn);

  return (
    <>
      {s}
      <div onClick={() => setS(s + 1)}>加一</div>
      <div onClick={fn}>输出s的值</div>
    </>
  );
}

```

第一次调用的时候

```tsx
// state = 2
// globalFn = () => {
// 	console.log(s)
// }

state没有值，使用传入的默认值1
将传入的默认值赋值给全局state
将state的值赋值给s
s = 1
临时函数tempFn指向全局函数globalFn，将tempFn传给useCallback，最后会将globalCallback赋值给函数fn，当第二个参数为空数组，没有填依赖值的时候，他会永远指向临时函数，以后重新渲染调用的时候，输出的还是之前作用域的s
// 当点击的时候s+1， 这个时候全局state=state+1=2
这个时候state = 2
所以s=2



```

### ahooks

#### usePersistFn: 不用考虑依赖项

**实现原理**：

1. 通过useRef+useCallback
2. useRef可以保证地址永远不变

```tsx
import { useRef } from 'react';

export type noop = (...args: any[]) => any;

function usePersistFn<T extends noop>(fn: T) {
  const fnRef = useRef<T>(fn);
  fnRef.current = fn;

  const persistFn = useRef<T>();
  if (!persistFn.current) {
    persistFn.current = function (...args) {
      return fnRef.current!.apply(this, args);
    } as T;
  }

  return persistFn.current!;
}

export default usePersistFn;
```

### refs

#### 何时使用refs

1. 管理焦点，文本选择或媒体播放
2. 触发强制动画
3. 集成第三方DOM库

**例如**：避免在Dialog组件里面暴露`open()`，`close()`方法，最好传递`isOpen`属性

#### 创建Refs

Refs是使用`React.createRef()`创建的，并通过ref属性附加到React元素上

```tsx
const myRef = useRef(null);
const handleClick = () => {
  console.log(myRef.current);
};

<button onClick={handleClick} ref={myRef}>
  click
</button>
```

#### 函数式组件中使用ref

**可以使用forwardRef**（可以与`useImperativeHandle`结合使用）

**Ref转发使组件可以像暴露自己的ref一样暴露自组件的ref。**



#### Ref转发

ref转发是一项将ref自动的通过组件传递到其一自组件的技巧，对于大多数应用中的组件来说，这通常不是必需的。但对某些组件，尤其是可重用的组件库是很有用的。

**1. 转发refs到DOM组件**

**Ref 转发是一个可选特性，其允许某些组件接收 `ref`，并将其向下传递（换句话说，“转发”它）给子组件。**

在下面的示例中，`FancyButton` 使用 `React.forwardRef` 来获取传递给它的 `ref`，然后转发到它渲染的 DOM `button`

```tsx
const FancyButton = React.forwardRef((props: any, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
// 这样，使用 FancyButton 的组件可以获取底层 DOM 节点 button 的 ref ，并在必要时访问
```

**注意**：

+ 第二个参数 `ref` 只在使用 `React.forwardRef` 定义组件时存在。常规函数和 class 组件不接收 `ref` 参数，且 props 中也不存在 `ref`。

**2. 在高阶组件中转发refs**

### render props

`render prop`是指一种在React组件之间使用一个值为函数的prop共享代码的简单技术

具有 render prop 的组件接受一个返回React元素的函数，并在组件内部通过调用此函数来实现自己的渲染逻辑

**render prop 是一个用于告知组件需要渲染什么内容的函数 prop。**

### 非受控组件

一般情况下，我们使用受控组件来处理表单数据，在一个受控组件中，表单数据是由react组件来管理的，

另一种替代方案是使用非受控组件，这时的表单数据将交由DOM节点来处理

**要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以使用ref来从DOM节点中获取表单数据**

### 自定义Hook

1. 自定义 Hook 是一个函数，其名称以 “`use`” 开头，函数内部可以调用其他的 Hook。

2. 自定义Hook是一种自然遵循Hook设计的约定，而并不是react的特性
3. 自定义Hook必须以use开头



### Hooks实现原理

#### 1. useState实现原理

+ 简单实现，只使用一个useState

```tsx
// 伪代码  这样每次都会重新调用组件，就会重新初始化
function _useState(initialState: any) {
  let _state = initialState;
  const _setState = (newState: any) => {
    _state = newState;
  };
  ReactDOM.render(<App />, rootElement);
  return [_state, _setState];
}

const [count, setCount] = _useState(0);

// 每次更新时，函数组件会被重新调用，也就是说 useState() 会被重新调用，为了使 state 的新值被记录（而不是一直被重新赋上 initialState），需要将其提到外部作用域声明
let _state: any;
function _useState(initialState: any) {
    _state = _state || initialState
    const _setState = (newState: any) => {
        _state = newState
    }
    ReactDOM.render(<App />, rootElement);
    return [_state, _setState]
}
const [count, setCount] = _useState(0)
```



+ 使用多个useState，将`_state`改成数组存储

```tsx
 // 使用了多个useState的时候
let _state: any[] = [],
  _index = 0;
function _useState(initialState: any) {
  let curIndex = _index;
  _state[curIndex] =
    _state[curIndex] === undefined ? initialState : _state[curIndex];
  const _setState = (newState: any) => {
    _state[curIndex] = newState;
    ReactDOM.render(<App />, rootElement);
    _index = 0; // 每更新一次都要将_index重新归零，才不会重复增加_state
  };
  _index += 1; // 下一个操作的索引
  return [_state[curIndex], _setState];
}
const [count, setCount] = _useState(0);
const [name, setName] = _useState('cpp');
const handleAdd = () => {
  console.log(count);
  setCount(count + 1);
};
const handleName = () => {
  setName('cjz');
};

<button onClick={handleAdd}>+1</button>
<button onClick={handleName}>修改名字</button>
```

+ 简易实现+效果演示

```tsx
import React from 'react';
import ReactDOM from 'react-dom';

let _state: any[] = [],
  _index = 0;
function _useState(initialState: any) {
  let curIndex = _index;
  _state[curIndex] =
    _state[curIndex] === undefined ? initialState : _state[curIndex];
  const _setState = (newState: any) => {
    _state[curIndex] = newState;
    ReactDOM.render(<HooksTest />, document.getElementById('root'));
    _index = 0; // 每更新一次都要将_index重新归零，才不会重复增加_state
  };
  _index += 1; // 下一个操作的索引
  return [_state[curIndex], _setState];
}

const HooksTest = () => {
  const [count, setCount] = _useState(0);
  const [name, setName] = _useState('cpp');
  const handleAdd = () => {
    console.log(count);
    setCount(count + 1);
  };
  const handleName = () => {
    setName('cjz');
  };
  return (
    <div>
      <h2>Hooks实现测试页面</h2>
      <div>
        count: {count} name: {name}
      </div>

      <button onClick={handleAdd}>+1</button>
      <button onClick={handleName}>修改名字</button>
    </div>
  );
};
export default HooksTest;
```

+ 使用回调的情况

```tsx
// 回调函数的形式
let _state: any[] = [],
  _index = 0;
function _useState(initialState: any) {
  let curIndex = _index;
  _state[curIndex] =
    _state[curIndex] === undefined ? initialState : _state[curIndex];
  const _setState = (newState: any) => {
    if (typeof newState === 'function') {
      newState = newState(_state[curIndex]);
    }
    _state[curIndex] = newState;
    ReactDOM.render(<HooksTest />, document.getElementById('root'));
    _index = 0; // 每更新一次都要将_index重新归零，才不会重复增加_state
  };
  _index += 1; // 下一个操作的索引
  return [_state[curIndex], _setState];
}

const HooksTest = () => {
  const [count, setCount] = _useState(0);
  const [name, setName] = _useState('cpp');
  const [age, setAge] = _useState(0);
  const handleAdd = () => {
    console.log(count);
    setCount(count + 1);
  };
  const handleNameChange = () => {
    setName('cjz' + Math.random());
  };
  const handleAgeChange = () => {
    setAge((age: number): number => age + 1);
  };
  return (
    <div>
      <h2>Hooks实现测试页面</h2>
      <div>
        count: {count} name: {name} 年龄: {age}
      </div>

      <button onClick={handleAdd}>+1</button>
      <button onClick={handleNameChange}>修改名字</button>
      <button onClick={handleAgeChange}>增加年龄</button>
    </div>
  );
};
export default HooksTest;
```

**注意**：

虽然使用数组存储`_state`成功模拟了多个`useState`的情况，但这要求我们保证`useState`的调用顺序，所以我们不能在循环、条件、或嵌套函数中调用`useState`

#### 2. useEffect的简单实=实现原理

```tsx
// 2. useEffect实现原理
let _deps: any; // _deps记录上一次的依赖
function _useEffect(callback: any, depArray?: any) {
  const hasNoDeps = !depArray; // 如果dependencies不存在
  const hasChangeDeps = _deps
    ? !depArray.every((el: any, i: any) => el === _deps[i])
    : true;
  // 如果 dependencies 不存在，或者 dependencies 有变化
  if (hasNoDeps || hasChangeDeps) {
    callback();
    _deps = depArray;
  }
}

const [number, setNumber] = _useState(0);
// const [number, setNumber] = _useState(0); 或者使用上面自定义的_useState
_useEffect(() => {
  setInterval(() => {
    setNumber(number + 1);
  }, 2000);
}, []);

```

**每一个**组件内的函数（包括事件处理函数，effects，定时器或者API调用等等）会捕获某次渲染中定义的props和state。

React只会在[浏览器绘制](https://medium.com/@dan_abramov/this-benchmark-is-indeed-flawed-c3d6b5b6f97f)后运行effects

```ts
let memoizedState = []; // hooks 存放在这个数组
let cursor = 0; // 当前 memoizedState 下标

function useState(initialValue) {
  memoizedState[cursor] = memoizedState[cursor] || initialValue;
  const currentCursor = cursor;
  function setState(newState) {
    memoizedState[currentCursor] = newState;
    render();
  }
  return [memoizedState[cursor++], setState]; // 返回当前 state，并把 cursor 加 1
}

function useEffect(callback, depArray) {
  const hasNoDeps = !depArray;
  const deps = memoizedState[cursor];
  const hasChangedDeps = deps
    ? !depArray.every((el, i) => el === deps[i])
    : true;
  if (hasNoDeps || hasChangedDeps) {
    callback();
    memoizedState[cursor] = depArray;
  }
  cursor++;
}
```



### FAQ问题学习解析

1. **如果我的 effect 的依赖频繁变化，我该怎么办？**

+ 设置为空数组`[]`

```tsx
const [count, setCount] = useState(0);

useEffect(() => {
  setInterval(() => {
    setCount(count + 1);
  }, 1000);
}, []);

<div>{count}</div>

// 伪代码分析
// 因为设置了[]，所以只有在挂载的时候才会调用setInterval
// useState的原理，有一个全局的_count
// 第一次调用（渲染）的时候
var _state	// _count
// 传入initialState为0
function useState(initialState) { 
  _state = _state || initialState // 第一次_count没有值,所以将initialState默认值赋值给_count
  const _setState = (newState) {  // setCount(count + 1)   =>   setCount(0 + 1)	newState为 0+1 = 1
    _state = newState	// _state = 1
  }
  return [_state, _setState]
}

// 此时_count为1， 但是因为设置的空数组[],所以值改变的时候不会再执行setInterval了
// 但是第一次调用的时候的setInterval里面的函数还是会每隔一秒setCount，但是这个作用域中的_count为0，所以永远都是setCount(0+1)，count永远都显示1。


// 设置为空数组，但是使用回调的形式的时候
// 因为当 effect 执行时，我们会创建一个闭包，并将 count 的值被保存在该闭包当中，且初值为 0。每隔一秒，回调就会执行 setCount(0 + 1)，因此，count 永远不会超过 1。
const [count, setCount] = useState(0);

useEffect(() => {
  setInterval(() => {
    setCount(c => c + 1);
  }, 1000);
}, []);

<div>{count}</div>


// 伪代码
let _count = 10
function useState() {
  function setCount(fn) {
    if (typeof fn === 'function') {
      _count = fn(_count)
    } else {
      _count = fn
    }
  }
  return [_count, setCount]
}

setCount(1)
setCount((c) => c + 1)
```

**分析**：



```tsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1); // ✅ 在这不依赖于外部的 `count` 变量
    }, 1000);
    return () => clearInterval(id);
  }, []); // ✅ 我们的 effect 不使用组件作用域中的任何变量

  return <h1>{count}</h1>;
}

```



### 扩展学习

#### Hook的闭包陷阱





