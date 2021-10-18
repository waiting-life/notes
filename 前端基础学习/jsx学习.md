## jsx

### 虚拟DOM



#### 1. jsx创建虚拟DOM

```jsx
const VDOM = (
    <h1 id="title">
    	<span>Hello, React</span>
    </h1>
)

// 胫骨哦babel翻译
const VDOM = React.createElement('h1', {id: 'title'}, React.createElement('span', {}, 'Hello. React'))
```



####  2. js创建虚拟DOM

```js
const VDOM = React.createElement('h1', {id: 'title'}, React.createElement('span', {}, 'Hello. React'))
```



**总结**：**jsx是为了更方便的创建虚拟DOM**



#### 3. 虚拟DOM

+ 虚拟DOM本质上是Object类型的对象（一般对象）
+ 虚拟DOM属性少， 真实DOM属性多，因为虚拟DOM是React内部在用，无需真实DOM上那么多属性。
+ 虚拟DOM最终会被转化成真实DOM，展现在页面上



### jsx语法规则

1. 定义虚拟DOM时，不要写引号
2. 标签中混入js表达式时要用{}
3. 样式的类名指定要用className
4. 内联样式，比如`<span style="{{color: 'white';fontSize: '30px'}}">哈哈哈哈哈</span>`要这样写
5. 虚拟DOM必须只有一个根标签
6. 标签必须闭合
7. 标签首字母
   + 若小写字母开头，则将该标签改为html中同名元素，若html中无该标签对应的同名元素，则报错。
   + 若大写字母开头，react就去渲染对应的组件，若组件没有被定义，则报错。